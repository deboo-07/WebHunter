# Executive summary

## Assessment

- System: OWASP Juice Shop 20.4.0-SNAPSHOT, local instance at 127.0.0.1:3000.
- Assessment period: 2026-11-23 to 2026-11-28.
- Scope: limited manual web and API tests using synthetic accounts; controlled SSRF sink at 127.0.0.1:3001.
- Exclusions: active testing of both supplied public URLs, other users' data, denial of service, external SSRF destinations, and destructive account actions.
- Authorization: active tests were confined to the local lab requested for this coursework.

## Overall result

Three security findings with demonstrated impact were scored in the deliberately vulnerable local training app: an unauthenticated SQL injection login bypass, DOM XSS that can read a logged-in browser token, and cross-account basket reading. Server-side URL fetching to the controlled loopback sink was also confirmed, but no confidentiality, integrity, or availability impact was demonstrated; F-04 is therefore not assigned a CVSS Base score.

No claim is made about the public Heroku host. No active payload was sent to that host because authorization for active testing was not verified.

## Finding counts

- Critical: 1
- High: 0
- Medium: 2
- Low: 0
- Unscored confirmed behavior: 1 (F-04, loopback request proof; impact unverified)
- Informational observations: 2 (verbose development error response; authenticated user can request their own password hash)

## Recommended actions

1. Parameterize all database queries, starting with the login route.
2. Remove unsafe HTML rendering of search input and reduce session token exposure to same-origin script.
3. Restrict server-side image fetches to approved destinations and block loopback/private addresses.
4. Enforce server-side ownership checks for every basket operation.
5. Apply fixes and retest against the same local reproduction steps.

## Retest status

No post-fix retest was performed because the lab application was not modified. F-01 and F-02 were revalidated on a fresh local instance on 28 November, and F-06's own-account behavior was checked again; those checks reconfirmed the vulnerable build and do not validate a fix. The three scored findings and F-04 behavior remain open.

## Tooling limitation

Burp Suite Community was used as available. A native disk-based Burp project is PENDING; it was not fabricated. Sanitized request records, screenshots, and a Postman collection are included instead.
