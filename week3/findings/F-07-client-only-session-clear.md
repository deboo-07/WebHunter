# F-07 — Client-side logout does not revoke an issued token

**Classification:** confirmed session behavior; reported as part of the F-03 XSS-to-session chain, not separately scored.

In a synthetic local session, the XSS script could read the bearer token from localStorage. The test then cleared localStorage and the token cookie in the browser and replayed the same in-memory token to GET /rest/user/whoami?fields=email. The API returned HTTP 200 for the same synthetic account. The app configures a six-hour token expiry; the test did not wait for expiry.

No token value was printed or saved, and no network exfiltration was performed. This confirms the session remains usable after the browser forgets it; it does not by itself prove how every logout path behaves in every deployment.

Evidence: evidence/F-07/xss-session-chain.png and evidence/F-07/session-replay.json.

Recommended improvement: use a session design that supports server-side revocation or token rotation when logout is expected to invalidate a session, reduce token lifetime, and avoid exposing bearer tokens to same-origin script.

Retest: NOT RETESTED.
