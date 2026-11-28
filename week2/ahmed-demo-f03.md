# F-03: DOM XSS in the search page exposes the browser session token

| Field | Value |
|---|---|
| ID | F-03 |
| Severity | Medium, CVSS v4.0 5.1 |
| Vector | CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:A/VC:N/VI:N/VA:N/SC:L/SI:L/SA:N |
| Target | OWASP Juice Shop 20.4.0-SNAPSHOT, http://127.0.0.1:3000 (local lab) |
| Location | Search route, query parameter `q` |
| Test date | 2026-11-23 |
| Account | Disposable synthetic account created for this test |
| Status | Confirmed in the local training lab only, open, not retested |

## Summary

The search page takes the `q` value from the URL and renders it into the page as HTML instead of as text. A harmless local marker sent through `q` executed as script in the browser. With the synthetic account logged in, that same-origin script could read the authentication token stored in localStorage. The token was never transmitted, printed, or saved.

## Root Cause

The search page sends untrusted query text to an HTML-rendering sink through an unsafe frontend bypass. This allows script execution in the application origin. Session data stored in localStorage is readable by page scripts.

## Evidence

- [dom-xss-executed.png](evidence/F-03/dom-xss-executed.png): the browser screenshot. It shows the marker executing; it does not contain the token value.
- [session-replay.json](../week3/evidence/F-07/session-replay.json): the sanitized replay summary showing the token read and the HTTP 200 response after client-side clearing.
- [xss-session-chain.png](../week3/evidence/F-07/xss-session-chain.png): a summary image of the chain, not a raw capture.
- `week2/payload-log.csv`, row W2-002: the test category, baseline, and observed result.
- `week2/cvss-worksheet.md`: the score and vector entry.

The token value appears in no evidence file.

## Impact

An attacker who persuades a logged-in user to open a crafted search URL can run script in the application origin. The local proof shows the script can read the bearer token, and that the same token still authenticates after the browser's local storage and cookie are cleared. No token exfiltration was attempted. The replay only returned the synthetic account's email field, and the configured six-hour token expiry was not waited out. The finding belongs to the deliberately vulnerable local training build, not to any public deployment.

## Fix

- Render the search query with Angular interpolation or a safe text API, and remove the unsafe HTML bypass.
- Apply context-appropriate output encoding, and never assign untrusted strings to `innerHTML`.
- Keep session tokens in short-lived, HttpOnly, Secure, SameSite cookies, with server-side revocation or rotation where the design requires it, so page script cannot read them.
- Deploy a strict Content Security Policy as defense in depth.

Retest: NOT RETESTED. No fix was applied to the lab build.
