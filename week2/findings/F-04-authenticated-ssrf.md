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

- Score: Not assigned — impact was not demonstrated
- Vector: Not assigned
- Rationale: the test confirms that the application issued a request to the tester-controlled loopback sink, but it does not show disclosure of restricted information, unauthorized modification, or service impact. Under the FIRST CVSS v4.0 impact definitions, the demonstrated consequence must support the impact metrics; this evidence is not enough to assign them. Reassess only if authorized testing establishes a concrete impact.

## Remediation

- Do not fetch arbitrary user-supplied URLs. Use a strict hostname and scheme allowlist.
- Resolve and validate every address; block loopback, private, link-local, multicast, and IPv4/IPv6 mapped destinations.
- Revalidate redirect targets and DNS results to reduce redirect and rebinding bypasses.
- Apply outbound network controls and response-size/time limits; validate image content by decoding it.

## Retest

NOT RETESTED — no fix was applied to the lab build.
