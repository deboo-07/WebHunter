# F-02 — SQL injection bypasses login

## Status and scope

- Status: CONFIRMED in local training lab only
- Target: OWASP Juice Shop 20.4.0-SNAPSHOT, 127.0.0.1:3000
- Endpoint: POST /rest/user/login
- Test dates: 2026-11-23 (original), 2026-11-28 (re-test from scratch, this file)
- Accounts: no real credentials; invalid synthetic passwords only
- Tool: direct API requests (Python urllib, equivalent to Postman / curl). No public host tested.

## Summary

The email value in the login request changes the SQL predicate because it is interpolated directly into a query. The baseline requests returned 401. Two controlled boolean-expression attempts in the email field, with two different invalid passwords, returned 200 and authenticated as the seeded administrator account.

## Root cause (verified in source, 2026-11-28)

File: `routes/login.ts`, line 34

```ts
models.sequelize.query(`SELECT * FROM Users WHERE email = '${req.body.email || ''}' AND password = '${security.hash(req.body.password || '')}' AND deletedAt IS NULL`, { model: UserModel, plain: true })
```

The email is concatenated into SQL. The password is hashed before interpolation, which explains why injection in the password field (P3 below) did not work.

With email `' OR 1=1--` the query becomes, in effect:

```sql
SELECT * FROM Users WHERE email = '' OR 1=1--' AND password = '...' AND deletedAt IS NULL
```

`OR 1=1` is always true and `--` comments out the password check, so the first row — the seeded administrator — is returned.

## Minimal reproduction

1. Send a baseline login with a nonexistent synthetic email `not-a-real-user@example.test` and invalid password 1; observe HTTP 401. (B1)
2. Send a baseline login with `admin@juice-sh.op` and the same invalid password 1; observe HTTP 401. This shows a correct admin password is normally required. (B2, new in re-test)
3. Send the same request with email value `' OR 1=1--` and invalid password 1; observe HTTP 200 and authentication as `admin@juice-sh.op`. (P1)
4. Repeat with invalid password 2 (a different invalid password); observe HTTP 200 as the same administrator account. (P2)
5. No token value was saved. Evidence is in `results-sanitized.json` in the re-test folder and in the original `../evidence/F-02/reproduction.json`.

Full request (sanitized):

```json
{
  "email": "' OR 1=1--",
  "password": "<invalid-synthetic-redacted>"
}
```

## Negative tests (new in re-test, 2026-11-28)

| Test | Input | Result | Meaning |
|---|---|---|---|
| P3 — injection in password field | email `admin@juice-sh.op`, password `' OR 1=1--` | 401 | Password is hashed before interpolation, so this field is not injectable with this payload. |
| P4 — variant without comment | email `' OR '1'='1`, invalid password 1 | 401 | Without `--`, the password hash check remains (`AND` binds tighter than `OR`), so login fails. |
| S1 — same payload in search | `GET /rest/products/search?q=' OR 1=1--` | 500 SQLITE_ERROR: incomplete input | Search SQLi NOT confirmed with this payload. Baseline `q=apple` returned 200 with 3 products. `routes/search.ts:23` interpolates search criteria separately; that is a different UNION challenge and out of scope for F-02. |

## Impact demonstrated

An unauthenticated request established an administrator session in this deliberately vulnerable lab. No administrative changes, destructive actions, data extraction, or tests against other hosts were performed.

## CVSS v4.0 — corrected

- Score: 9.3 Critical
- Vector: CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N
- Correction: VA changed from L to N. The original files set VA:L with the rationale "denial of service was not tested". No availability impact was demonstrated, so VA:N is correct. The score remains 9.3 Critical after recalculation.
- Rationale: network reachable, no prior privileges or victim interaction; successful login grants the seeded administrator's application privileges. Score calculated with the FIRST CVSS v4.0 calculator and cross-checked with the `cvss` Python library.

## Remediation

- Replace the interpolated query in `routes/login.ts:34` with a parameterized query. The project's own correct codefix uses bound parameters:
  ```ts
  models.sequelize.query(`SELECT * FROM Users WHERE email = $1 AND password = $2 AND deletedAt IS NULL`,
    { bind: [req.body.email, security.hash(req.body.password)], model: models.User, plain: true })
  ```
- Keep user-controlled values out of SQL syntax and bind them as data.
- Return a generic authentication error and add regression checks for boolean SQLi inputs, including the exact payload `' OR 1=1--` and the negative cases P3/P4 above.
- Review similar query construction in all authentication and search routes (`routes/search.ts:23` is a separate case).

## Retest

NOT RETESTED after a fix — no fix was applied to the lab build in this re-test. The re-test above re-confirmed the original finding on a fresh local instance on 2026-11-28; it did not test a fixed build.
