# F-03 — Search query DOM XSS exposes the browser session token

## Status and scope

- Status: CONFIRMED in local training lab only
- Target: OWASP Juice Shop 20.4.0-SNAPSHOT, 127.0.0.1:3000
- Location: search route query parameter q
- Test date: 2026-11-23
- Account: synthetic account created for this test

## Summary

The search page renders the q parameter as HTML. A harmless local marker executed as script in the browser. With the synthetic account logged in, the same-origin script could read the authentication token from localStorage. The token was not transmitted or recorded.

## Minimal reproduction

1. Log in with the disposable synthetic account.
2. Open the local search page with a q value containing an image element whose error handler sets a visible DOM marker and checks whether localStorage contains the token.
3. Observe the marker and the test result showing that the token is readable.
4. Clear localStorage and the browser token cookie, then replay the in-memory token to the local whoami endpoint; it still returned HTTP 200 for the same synthetic account.

Evidence: ../evidence/F-03/dom-xss-executed.png, ../../week3/evidence/F-07/xss-session-chain.png, and ../../week3/evidence/F-07/session-replay.json. The token value is not in any evidence file.

## Impact demonstrated

An attacker who can persuade a logged-in user to open a crafted search URL can execute script in the app origin. The local proof showed the script can read the bearer token and that the old token remains accepted after client-side storage is cleared. No token exfiltration was attempted. The local app's signing code configures a six-hour expiry; the test did not wait six hours.

## CVSS v4.0

- Score: 5.1 Medium
- Vector: CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:A/VC:N/VI:N/VA:N/SC:L/SI:L/SA:N
- Rationale: the victim must consciously open a crafted link, so User Interaction is Active. The local proof confirmed script execution and token-read capability. Score recalculated with the CVSS v4.0 calculator; see the official FIRST guidance for reflected XSS and clicking a link.

## Remediation

- Render the query using Angular interpolation or a safe text API; remove the unsafe HTML bypass.
- Apply context-appropriate output encoding and avoid assigning untrusted strings to innerHTML.
- Prefer short-lived, HttpOnly, Secure, SameSite cookies for session tokens, with server-side revocation or rotation on logout where the design requires it.
- Deploy a strict Content Security Policy as defense in depth.

## Retest

NOT RETESTED — no fix was applied to the lab build.
