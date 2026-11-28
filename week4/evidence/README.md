# Evidence index

All images and response summaries are from the local Juice Shop training instance. Sensitive values are omitted or redacted.

| Evidence | Demonstrates |
|---|---|
| week2/evidence/F-01/error-response-redacted.png | Local development error response; absolute paths redacted; observation only. |
| week2/evidence/F-01/home-catalog.png | Local lab home page loaded. |
| week2/evidence/F-02/sql-injection-login.png | UI confirmation of seeded admin login through SQLi in local lab. |
| week2/evidence/F-02/reproduction.json | 401 baseline vs two controlled 200 SQLi logins; JWT values not recorded. |
| week2/evidence/F-03/dom-xss-executed.png | Search DOM XSS executed in a logged-in synthetic account; marker shown. |
| week2/evidence/F-04/controlled-local-sink.png | Local SSRF sink received one request on 127.0.0.1:3001. |
| week2/evidence/input-class-validation.json | SSRF, harmless upload, and traversal outcome summaries. |
| week3/evidence/F-05/cross-account-basket.png | Account A read account B's synthetic populated basket; IDs redacted. |
| week3/evidence/F-06/password-field-redacted.png | Explicit own password field was present; hash value redacted. |
| week3/evidence/F-07/xss-session-chain.png | XSS could read token and the old token still authenticated after browser storage clear. |
| week3/evidence/api-validation.json | Sanitized two-account API comparisons; no tokens, hashes, passwords, or IDs. |
| week3/evidence/F-07/session-replay.json | Replay result and token redaction status. |

No native Burp project file is included; see week1/BURP_PROJECT_STATUS.md.
