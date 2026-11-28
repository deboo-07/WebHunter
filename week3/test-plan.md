# Week 3 test plan and results

## Authorization record

- Lab: OWASP Juice Shop 20.4.0-SNAPSHOT at 127.0.0.1:3000.
- Test accounts: synthetic customer A and B; a separate synthetic account for the browser token test.
- Test date: 2026-11-23.
- Scope limits: no real accounts, external services, broad scans, or high-volume requests.

## Test areas

| Area | Result | Evidence / limitation |
|---|---|---|
| Broken authentication | SQL injection login bypass confirmed as F-02; baseline 401 vs injected 200 | ../week2/evidence/F-02/ |
| Token storage and XSS | Same-origin XSS could read synthetic session token from localStorage | evidence/F-07/xss-session-chain.png |
| Token replay after client-side clearing | Old token still returned HTTP 200 after browser token/cookie clearing | evidence/F-07/session-replay.json; token value omitted |
| Basket BOLA | Account A read account B's populated synthetic basket; read-only | evidence/F-05/cross-account-basket.png |
| Excessive field request | Default whoami omitted password; explicit fields=password returned own hash only | evidence/F-06/password-field-redacted.png |
| JWT signature tampering | Not tested | — |
| Session fixation | Not tested | — |
| Token expiration | Expiration not awaited; source indicates six-hour expiry | — |
| Rate limiting | Only a few low-volume requests; threshold not established | Inconclusive |
| Business logic | Not comprehensively tested | Not run |

## False-positive checks

- SQLi compared an invalid-user baseline with two identical boolean SQLi attempts using different invalid passwords.
- BOLA used two tester-owned accounts; account B's basket was populated before account A requested it.
- Hash exposure compared the default response with the explicit field request and did not request any other account's hash.
- Session replay used only a test-owned token, kept in memory, and checked only the same synthetic account's email field.
