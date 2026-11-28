# F-06 — Explicit request returns the authenticated user's password hash

**Classification:** confirmed API behavior; no separate vulnerability severity assigned.

- Target: OWASP Juice Shop 20.4.0-SNAPSHOT, local instance at http://127.0.0.1:3000
- Test dates: 2026-11-23 (original), 2026-11-28 (re-test from scratch)
- Account: synthetic test account only, created for this test
- Status: CONFIRMED as observed behavior in the local lab only

## What was observed

For a synthetic authenticated account:

- GET /rest/user/whoami (with Cookie token) returned 200 with id, email, lastLoginIp and profileImage. The password property was omitted by default.
- GET /rest/user/whoami?fields=password (with Cookie token) returned 200 with the password property present. The hash value is redacted and was not stored.

No other user's data was requested, and no cross-account hash disclosure was established.

Because the caller can only retrieve its own hash in the tested flow and already controls the account, this run did not demonstrate independent confidentiality impact. It is retained as an API data-minimization observation rather than a scored vulnerability.

## Important reproduction detail (added 2026-11-28)

The `Cookie: token=<token>` header is required. With `Authorization: Bearer <token>` alone, GET /rest/user/whoami returned 200 with `{"user":{}}` — an empty user object. With the Cookie token added, the same request returned the user fields above. The original finding file did not state this; without it the test is not reproducible.

Evidence: `evidence/F-06/password-field-redacted.png` (sanitized summary card, hash value [REDACTED]) and `evidence/F-06/reproduction.json` (booleans only — no hash, token, or account identifier stored).

## Recommended improvement

Maintain an explicit allowlist of safe response fields and never include password hashes in API output, even when explicitly requested with `?fields=password`.

Retest: NOT RETESTED after a fix — no fix was applied to the lab build.
