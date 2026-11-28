# F-04 — Profile image URL triggers server-side requests

## Status and scope

- Status: CONFIRMED to a tester-controlled loopback sink only
- Target: OWASP Juice Shop 20.4.0-SNAPSHOT, 127.0.0.1:3000
- Endpoint: POST /profile/image/url
- Test date: 2026-11-23
- Authentication: synthetic local account

## Summary

The authenticated profile image feature accepts a user-supplied URL and the server fetches it. A temporary HTTP receiver bound only to 127.0.0.1:3001 recorded a request to /test.png. No public destination, cloud metadata endpoint, or sensitive internal service was queried.

## Minimal reproduction

1. Start the controlled mock image sink on 127.0.0.1:3001.
2. Authenticate as a synthetic local account.
3. Submit imageUrl=http://127.0.0.1:3001/test.png to POST /profile/image/url.
4. Observe HTTP 302 from the application and one request at the local sink.

Evidence: ../evidence/F-04/controlled-local-sink.png and ../evidence/input-class-validation.json.

## Impact demonstrated

The server made an authenticated, user-directed request to a loopback service. This establishes server-side URL fetching and loopback reachability in the local build. Access to any real internal data or another service was not attempted; the real deployment impact depends on its network and URL controls.

## CVSS v4.0

- Score: 5.3 Medium
- Vector: CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:L/VI:N/VA:N/SC:L/SI:N/SA:N
- Rationale: authentication is required; a supplied URL reached a controlled service on the local network. Confidentiality impact is kept Low and limited to the demonstrated request behavior. Score calculated with the FIRST CVSS v4.0 calculator.

## Remediation

- Do not fetch arbitrary user-supplied URLs. Use a strict hostname and scheme allowlist.
- Resolve and validate every address; block loopback, private, link-local, multicast, and IPv4/IPv6 mapped destinations.
- Revalidate redirect targets and DNS results to reduce redirect and rebinding bypasses.
- Apply outbound network controls and response-size/time limits; validate image content by decoding it.

## Retest

NOT RETESTED — no fix was applied to the lab build.
