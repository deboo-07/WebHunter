# Web Application & API Penetration Test Report

## Document control

- Project: DEPI Project 2 — Web Application & API Penetration Test
- Assessment period: 2026-11-23 to 2026-11-28
- Report version: 1.1
- Author: Not supplied
- Assessed system: OWASP Juice Shop 20.4.0-SNAPSHOT, local lab
- Distribution: coursework submission

## 1. Executive summary

This assessment exercised a local, isolated OWASP Juice Shop instance with synthetic accounts. Three security findings have scored impact: SQL injection bypasses login to the seeded administrator account; DOM XSS executes in the search page and can read a logged-in browser token; and one synthetic account can read another synthetic account's populated basket. The profile image feature also made a server-side request to a tester-controlled loopback service, but the test did not demonstrate a security impact, so F-04 remains unscored.

The SQL injection was scored Critical (CVSS v4.0 9.3); XSS was scored Medium (5.1) and the BOLA read Medium (6.9). F-04 is confirmed server-side request behavior, but it has no CVSS Base score because the loopback-sink test did not demonstrate confidentiality, integrity, or availability impact. These scores apply only to demonstrated behavior in the local training lab, not either public URL.

Neither public URL was subjected to active payload testing. In particular, authorization for active testing of juice-shop.herokuapp.com was not verified. No real customer data, real credentials, external SSRF destinations, denial-of-service actions, or token exfiltration were used.

## 2. Scope and rules of engagement

### In scope

- Local application: http://127.0.0.1:3000
- Temporary tester-controlled SSRF sink: http://127.0.0.1:3001
- Synthetic accounts and one synthetic basket item created for the local tests
- Manual, low-volume browser and API validation

### Out of scope

- Active testing of https://juice-shop.herokuapp.com/ or https://juice-shop.github.io/juice-shop/
- Other users' data, payment data, production infrastructure, cloud metadata, or external destinations
- Broad scanning, brute force, denial of service, destructive file actions, and high-volume rate-limit testing

### Environment limitation

The application was built from the official Juice Shop repository as version 20.4.0-SNAPSHOT and run bound to loopback. The local instance contains intentional training vulnerabilities and seeded challenge data. Findings must not be attributed to the public service operator. The official project labels its preview deployment as not intended for hacking; that warning does not grant or establish permission for the separate Heroku URL.

## 3. Methodology and evidence

The work included local page mapping, normal browser navigation, manual API requests, one baseline comparison per tested input, controlled two-account ownership comparison, and browser verification for XSS. Evidence is sanitized: JWT values, passwords, hashes, account identifiers, and basket IDs are omitted. Absolute local paths in the error screenshot are replaced with [LOCAL_PATH].

CVSS Base scores use version 4.0 and the FIRST calculator. The 2026-11-28 review corrected F-02 availability to None, F-03 user interaction to Active for a crafted-link click, and F-05 privileges required to None because registration is self-service. F-04 remains unscored because no impact was demonstrated. Business impact and deployment context remain separate from CVSS.

On 2026-11-28, F-01's unsupported-route error and F-02's login bypass were reproduced again on a fresh local instance; F-06's own-account password-field behavior was also checked. These checks reconfirmed behavior in the vulnerable build. No application fix was applied, so none is a post-fix retest.

## 4. Results overview

| ID | Finding | CVSS v4.0 | Status | Retest |
|---|---|---:|---|---|
| F-02 | SQL injection login bypass | 9.3 Critical | Open | Reconfirmed 28 Nov; no post-fix retest |
| F-03 | Search DOM XSS exposes browser token | 5.1 Medium | Open | No post-fix retest |
| F-04 | Authenticated profile image SSRF behavior; impact unverified | Not scored | Open | No post-fix retest |
| F-05 | Cross-account basket read (BOLA) | 6.9 Medium | Open | No post-fix retest |

Two additional behaviors are documented as observations, not scored findings: a stack trace on an unsupported route in the local development configuration (F-01) and the ability of an authenticated account to request its own password hash (F-06). F-07 records the session replay evidence supporting the XSS chain; it is not scored separately.

## 5. Findings

### F-02 — SQL injection bypasses login

**Severity:** Critical — CVSS v4.0 9.3

**Vector:** CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N
**Endpoint:** POST /rest/user/login  
**Status:** Confirmed in the local training app and reproduced again on a fresh instance on 2026-11-28; open; no post-fix retest.

**Description and proof.** A nonexistent synthetic account returned HTTP 401. Two requests using the boolean expression ' OR 1=1-- in the email field returned HTTP 200 and authenticated as the seeded administrator account. The password values were invalid synthetic strings. The UI displayed the Login Admin challenge success. No token value was stored.

**Impact.** An unauthenticated request established the local app's administrator session. No administrative data was changed and no destructive action was performed.

**Evidence.** [Sanitized API reproduction](../week2/evidence/F-02/reproduction.json) and [browser screenshot](../week2/evidence/F-02/sql-injection-login.png).

**Remediation.** Replace SQL string concatenation with parameterized queries or a safe ORM lookup; bind user input as data; return generic authentication errors; review other query-building paths and add regression checks.

**Score rationale.** Network-reachable, no privilege or victim interaction required; the verified result was administrator authentication. Availability is None because no availability impact was demonstrated. The corrected vector remains 9.3 Critical; it was reviewed against the FIRST v4.0 calculator on 2026-11-28.

### F-03 — Search DOM XSS can access a logged-in token

**Severity:** Medium — CVSS v4.0 5.1

**Vector:** CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:A/VC:N/VI:N/VA:N/SC:L/SI:L/SA:N
**Location:** search route query parameter q  
**Status:** Confirmed in the local training app; open; not retested.

**Description and proof.** The search page renders q as HTML. A harmless marker executed in the browser. With a synthetic account logged in, the script could read the token from localStorage. After the browser's local token and cookie were cleared, replaying the in-memory token returned HTTP 200 for the same synthetic account. No token was transmitted or written to the report.

**Impact.** A crafted link opened by a logged-in user can run script in the app origin. The local proof demonstrates token readability and authenticated replay. The tested response exposed only the synthetic account's email field; the test did not wait for the configured six-hour token expiry.

**Evidence.** [XSS browser screenshot](../week2/evidence/F-03/dom-xss-executed.png), [sanitized session-chain screenshot](../week3/evidence/F-07/xss-session-chain.png), and [replay summary](../week3/evidence/F-07/session-replay.json).

**Remediation.** Render search input as text; remove unsafe HTML bypasses; apply context-appropriate encoding; reduce bearer-token exposure to page script; use an HttpOnly, Secure, SameSite cookie design with appropriate revocation/rotation; add a strict Content Security Policy as defense in depth.

**Score rationale.** The victim must consciously open a crafted link, so User Interaction is Active. The local proof confirmed script execution and token-read capability. The vector assigns Low subsequent-system confidentiality and integrity impact. Score recalculated with the CVSS v4.0 calculator and checked against the FIRST guidance for reflected XSS and link clicks.

### F-04 — Profile image URL causes server-side fetch

**Severity:** Confirmed SSRF behavior; impact unverified; not scored
**Endpoint:** POST /profile/image/url  
**Status:** Server-side fetch confirmed to a controlled loopback sink only; no security impact demonstrated; open; not retested.

**Description and proof.** An authenticated synthetic account submitted a profile image URL pointing to http://127.0.0.1:3001/test.png. The application returned HTTP 302 and the temporary local sink recorded one request. No public, cloud metadata, or sensitive internal destination was contacted.

**Impact.** The local server made a user-directed request to the controlled loopback sink. The test did not show restricted information returned to the tester, unauthorized state change, or service disruption. Impact against another deployment is unknown and was not tested.

**Evidence.** [Controlled local sink screenshot](../week2/evidence/F-04/controlled-local-sink.png) and [class validation record](../week2/evidence/input-class-validation.json).

**Remediation.** Avoid arbitrary server-side URL fetches. Use a strict scheme/host allowlist, block loopback/private/link-local/multicast IPv4 and IPv6 destinations, revalidate redirects and DNS results, and apply outbound firewall and response-size/time limits.

**CVSS assessment.** No Base score or vector is assigned. The observed request proves server-side URL fetching to this sink, but does not establish a confidentiality, integrity, or availability consequence. Under the FIRST CVSS v4.0 impact definitions, an impact value must be supported by the demonstrated consequence. Reassess only if further authorized testing demonstrates impact.

### F-05 — Basket read lacks object-level authorization

**Severity:** Medium — CVSS v4.0 6.9
**Endpoint:** GET /rest/basket/{id}  
**Status:** Confirmed read-only using two synthetic accounts; open; not retested.

**Description and proof.** Account B first created a basket with one synthetic product row. While authenticated as account A, a request for account B's known synthetic basket returned HTTP 200 and the populated object. The test used only tester-created accounts and did not enumerate identifiers.

**Impact.** An attacker can create a basic account through the self-service registration endpoint, then read another account's basket when its identifier is known. The observed object contained a synthetic product row; no personal or payment data was requested.

**Evidence.** [Sanitized cross-account screenshot](../week3/evidence/F-05/cross-account-basket.png) and [API validation record](../week3/evidence/api-validation.json).

**Remediation.** Enforce ownership server-side on every basket read and mutation. Return 403/404 when the authenticated user does not own the requested object. Add two-account regression coverage for read, update, delete, and checkout operations.

**Vector:** CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:N/SI:N/SA:N
**Score rationale.** Registration is self-service in the tested application, so the attacker can create an account as part of the attack and Privileges Required is None. Only limited read access was demonstrated; no write or delete access was tested. Score recalculated with the CVSS v4.0 calculator.

## 6. Supporting observations

### F-01 — Verbose error in local development configuration

GET /rest/products returned HTTP 500 with an Express stack trace on this unsupported route. Absolute local paths were redacted in the screenshot. Because the request did not exercise a valid business route and this was a local development build, it is recorded as an observation and not scored. Return generic client errors and keep diagnostic traces in protected logs.

Evidence: [redacted error screenshot](../week2/evidence/F-01/error-response-redacted.png), [reproduction record](../week2/evidence/F-01/reproduction.md), and [28 November recheck](../week2/evidence/F-01/retest-2026-11-28.json).

### F-06 — Own password hash field

The default whoami response omitted the password property. A synthetic authenticated account could explicitly request fields=password and receive its own hash. This behavior was checked again on 28 November. No other account's hash was requested; this run did not establish independent cross-user impact, so it is not a scored finding. Use an explicit response-field allowlist and never return hashes.

Evidence: [redacted field screenshot](../week3/evidence/F-06/password-field-redacted.png) and [sanitized reproduction record](../week3/evidence/F-06/reproduction.json).

### F-07 — Client-side session clearing

The previously issued synthetic token remained accepted after the local browser token and cookie were cleared. This supports the F-03 chain and is not scored independently. The test did not wait for the six-hour expiry and no server-side revocation fix was attempted.

### F-08 — Directory listing visible in the local lab
On 27 November 2026, the local application's /ftp/ page displayed a directory index in an Incognito window. Burp recorded HTTP 200 for the index. The tester also opened legal.md. Access to the contents of the other listed files was not verified. The index discloses filenames. Disable directory indexing and explicitly publish only intended files. Retest pending.

[HTTP history](../week2/evidence/F-08/http-history.png) and [directory index](../week2/evidence/F-08/directory-listing.png).

## 7. Other test classes

| Class | Result |
|---|---|
| File upload | A harmless 25-byte .txt was accepted with HTTP 204; no persistence or execution observed. No finding. |
| Path traversal | One encoded slash attempt returned 403 and no file data. No finding established. |
| CSRF | Not run; no browser-backed cross-site state-change proof completed. |
| LFI | Not run; the candidate data-erasure route creates a deletion request and was not exercised. |
| RFI | Not applicable to the observed Juice Shop flow; no remote include behavior was identified. |
| JWT tampering / fixation | Not comprehensively tested. Token read and replay behavior is recorded under F-03/F-07. |
| Rate limiting | Only low-volume requests were used; a throttling threshold was not established. |
| Business logic | Not comprehensively tested. |

The detailed test matrix is in [Week 2 test plan](../week2/test-plan.md); API routes are listed in [Week 3 inventory](../week3/api-inventory.csv).

## 8. Attack chain

The evidence-backed chain is: crafted search URL -> victim opens it while logged in -> DOM XSS reads the browser token -> the same token still authenticates after client-side clearing -> API returns an authenticated synthetic-account response. See [the chain diagram and evidence table](attack-chain.md). No token exfiltration was attempted.

## 9. Remediation and retest plan

Prioritize F-02, F-03/session handling, and F-05. Apply the F-04 URL-fetch restrictions as defense in depth and reassess its impact only through authorized testing. Proposed fixes and verification steps are listed in [remediation-plan.csv](remediation-plan.csv). No fixes were applied during this engagement, so all retest rows remain NOT RETESTED.

## 10. Tooling and submission limitations

- Burp Suite Community was available. Its temporary project is memory-only; the native .burp file is therefore marked PENDING, not fabricated. See [Burp project status](../week1/BURP_PROJECT_STATUS.md).
- Sanitized HTTP evidence and a Postman collection are included.
- Public-host authorization was not verified. No active testing was conducted on the Heroku URL.
- The assessment is a bounded coursework lab exercise and does not provide production assurance.

## References

- [OWASP Juice Shop repository and preview warning](https://github.com/juice-shop/juice-shop)
- [OWASP Web Security Testing Guide v4.2](https://owasp.org/www-project-web-security-testing-guide/stable/)
- [FIRST CVSS v4.0 calculator](https://www.first.org/cvss/calculator/cvsscalc40)
- [FIRST CVSS v4.0 specification](https://www.first.org/cvss/v4.0/specification-document)
- [FIRST CVSS v4.0 FAQ on XSS interaction](https://www.first.org/cvss/v4.0/faq)
- [PortSwigger Burp project files](https://portswigger.net/burp/documentation/desktop/projects)
