# Web Application & API Penetration Test Report

## Document control

- Project: DEPI Project 2 — Web Application & API Penetration Test
- Assessment date: 2026-11-23
- Report version: 1.0
- Author: Not supplied
- Assessed system: OWASP Juice Shop 20.4.0-SNAPSHOT, local lab
- Distribution: coursework submission

## 1. Executive summary

This assessment exercised a local, isolated OWASP Juice Shop instance with synthetic accounts. Four findings were confirmed: SQL injection bypasses login to the seeded administrator account; DOM XSS executes in the search page and can read a logged-in browser token; the authenticated profile image feature makes a server-side request to a tester-controlled loopback service; and one synthetic account can read another synthetic account's populated basket.

The SQL injection was scored Critical (CVSS v4.0 9.3). The XSS, SSRF, and BOLA read were scored Medium (5.3 each) using the vectors below. These are scores for the vulnerable training behavior, not an assessment of either public URL.

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

CVSS Base scores use version 4.0 and the FIRST calculator. The vectors and score rationales are included below. Business impact and deployment context remain separate from CVSS.

## 4. Results overview

| ID | Finding | CVSS v4.0 | Status | Retest |
|---|---|---:|---|---|
| F-02 | SQL injection login bypass | 9.3 Critical | Open | Not retested |
| F-03 | Search DOM XSS exposes browser token | 5.3 Medium | Open | Not retested |
| F-04 | Authenticated profile image SSRF | 5.3 Medium | Open | Not retested |
| F-05 | Cross-account basket read (BOLA) | 5.3 Medium | Open | Not retested |

Two additional behaviors are documented as observations, not scored findings: a stack trace on an unsupported route in the local development configuration (F-01) and the ability of an authenticated account to request its own password hash (F-06). F-07 records the session replay evidence supporting the XSS chain; it is not scored separately.

## 5. Findings

### F-02 — SQL injection bypasses login

**Severity:** Critical — CVSS v4.0 9.3  
**Vector:** CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:L/SC:N/SI:N/SA:N  
**Endpoint:** POST /rest/user/login  
**Status:** Confirmed in the local training app; open; not retested.

**Description and proof.** A nonexistent synthetic account returned HTTP 401. Two requests using the boolean expression ' OR 1=1-- in the email field returned HTTP 200 and authenticated as the seeded administrator account. The password values were invalid synthetic strings. The UI displayed the Login Admin challenge success. No token value was stored.

**Impact.** An unauthenticated request established the local app's administrator session. No administrative data was changed and no destructive action was performed.

**Evidence.** [Sanitized API reproduction](../week2/evidence/F-02/reproduction.json) and [browser screenshot](../week2/evidence/F-02/sql-injection-login.png).

**Remediation.** Replace SQL string concatenation with parameterized queries or a safe ORM lookup; bind user input as data; return generic authentication errors; review other query-building paths and add regression checks.

**Score rationale.** Network-reachable, no privilege or victim interaction required; the verified result was administrator authentication. Availability impact is Low because service disruption was not tested. Score calculated with the FIRST v4.0 calculator.

### F-03 — Search DOM XSS can access a logged-in token

**Severity:** Medium — CVSS v4.0 5.3  
**Vector:** CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:P/VC:N/VI:N/VA:N/SC:L/SI:L/SA:N  
**Location:** search route query parameter q  
**Status:** Confirmed in the local training app; open; not retested.

**Description and proof.** The search page renders q as HTML. A harmless marker executed in the browser. With a synthetic account logged in, the script could read the token from localStorage. After the browser's local token and cookie were cleared, replaying the in-memory token returned HTTP 200 for the same synthetic account. No token was transmitted or written to the report.

**Impact.** A crafted link opened by a logged-in user can run script in the app origin. The local proof demonstrates token readability and authenticated replay. The tested response exposed only the synthetic account's email field; the test did not wait for the configured six-hour token expiry.

**Evidence.** [XSS browser screenshot](../week2/evidence/F-03/dom-xss-executed.png), [sanitized session-chain screenshot](../week3/evidence/F-07/xss-session-chain.png), and [replay summary](../week3/evidence/F-07/session-replay.json).

**Remediation.** Render search input as text; remove unsafe HTML bypasses; apply context-appropriate encoding; reduce bearer-token exposure to page script; use an HttpOnly, Secure, SameSite cookie design with appropriate revocation/rotation; add a strict Content Security Policy as defense in depth.

**Score rationale.** A victim must open the crafted URL; the demonstrated script runs in the browser and can read the authenticated token. The vector assigns Low subsequent-system confidentiality and integrity impact. Score calculated with the FIRST v4.0 calculator.

### F-04 — Profile image URL causes server-side fetch

**Severity:** Medium — CVSS v4.0 5.3  
**Endpoint:** POST /profile/image/url  
**Status:** Confirmed to a controlled loopback sink only; open; not retested.

**Description and proof.** An authenticated synthetic account submitted a profile image URL pointing to http://127.0.0.1:3001/test.png. The application returned HTTP 302 and the temporary local sink recorded one request. No public, cloud metadata, or sensitive internal destination was contacted.

**Impact.** The local server made a user-directed request to a loopback service. The impact against any real deployment depends on the network and URL controls there; no claim is made that sensitive internal data was read.

**Evidence.** [Controlled local sink screenshot](../week2/evidence/F-04/controlled-local-sink.png) and [class validation record](../week2/evidence/input-class-validation.json).

**Remediation.** Avoid arbitrary server-side URL fetches. Use a strict scheme/host allowlist, block loopback/private/link-local/multicast IPv4 and IPv6 destinations, revalidate redirects and DNS results, and apply outbound firewall and response-size/time limits.

**Score rationale.** Authentication is required; the only demonstrated disclosure is limited loopback reachability to a controlled receiver. Score calculated with the FIRST v4.0 calculator.

### F-05 — Basket read lacks object-level authorization

**Severity:** Medium — CVSS v4.0 5.3  
**Endpoint:** GET /rest/basket/{id}  
**Status:** Confirmed read-only using two synthetic accounts; open; not retested.

**Description and proof.** Account B first created a basket with one synthetic product row. While authenticated as account A, a request for account B's known synthetic basket returned HTTP 200 and the populated object. The test used only tester-created accounts and did not enumerate identifiers.

**Impact.** A basic account can read another account's basket contents when its identifier is known. The observed object contained a synthetic product row; no personal or payment data was requested.

**Evidence.** [Sanitized cross-account screenshot](../week3/evidence/F-05/cross-account-basket.png) and [API validation record](../week3/evidence/api-validation.json).

**Remediation.** Enforce ownership server-side on every basket read and mutation. Return 403/404 when the authenticated user does not own the requested object. Add two-account regression coverage for read, update, delete, and checkout operations.

**Score rationale.** A basic account is required and only limited read access was demonstrated. No write or delete access was tested. Score calculated with the FIRST v4.0 calculator.

## 6. Supporting observations

### F-01 — Verbose error in local development configuration

GET /rest/products returned HTTP 500 with an Express stack trace on this unsupported route. Absolute local paths were redacted in the screenshot. Because the request did not exercise a valid business route and this was a local development build, it is recorded as an observation and not scored. Return generic client errors and keep diagnostic traces in protected logs.

Evidence: [redacted error screenshot](../week2/evidence/F-01/error-response-redacted.png).

### F-06 — Own password hash field

The default whoami response omitted the password property. A synthetic authenticated account could explicitly request fields=password and receive its own hash. No other account's hash was requested; this run did not establish independent cross-user impact, so it is not a scored finding. Use an explicit response-field allowlist and never return hashes.

Evidence: [redacted field screenshot](../week3/evidence/F-06/password-field-redacted.png).

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

Prioritize F-02, then F-03/session handling, F-04, and F-05. Proposed fixes and verification steps are listed in [remediation-plan.csv](remediation-plan.csv). No fixes were applied during this engagement, so all retest rows remain NOT RETESTED.

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
- [PortSwigger Burp project files](https://portswigger.net/burp/documentation/desktop/projects)
