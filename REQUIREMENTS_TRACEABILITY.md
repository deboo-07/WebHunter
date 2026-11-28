# Requirements traceability

The attached screenshot is treated as the coursework rubric. Its tasks are requirements to assess, not authorization to test public hosts. Active testing in this pack was limited to localhost.

## Week 1 — Web fundamentals and Burp setup

| Requirement | Status | Deliverable / gap |
|---|---|---|
| HTTP methods, headers, cookies, sessions, authentication flow | PARTIAL | Observed request inventory and auth/session flow in week1; only routes used in this test are inventoried. |
| Burp proxy, Repeater, Intruder, site map | PARTIAL | Burp Community used for manual work; native project unavailable. No Intruder campaign was run. |
| Map Juice Shop target | COMPLETE FOR OBSERVED FLOWS | week1/application-map.md |
| Application map | COMPLETE | week1/application-map.md |
| Native Burp project file | PENDING | Burp Suite Community has no disk-based project; see week1/BURP_PROJECT_STATUS.md. |
| Request/response inventory | PARTIAL | week1/request-response-inventory.csv, based on observed local requests. |
| Auth/session flow diagram | COMPLETE FOR OBSERVED FLOW | week1/auth-session-flow.md |
| Test plan | COMPLETE | week1/test-plan.md and week2/week3 result matrices. |

## Week 2 — Input-based vulnerabilities

| Requirement | Status | Evidence / gap |
|---|---|---|
| SQL injection | CONFIRMED | F-02; sanitized API reproduction and UI screenshot. |
| XSS | CONFIRMED | F-03; browser execution screenshot; token-read chain in week3/F-07. |
| CSRF | NOT RUN | No browser-backed cross-site state-change proof. |
| SSRF | REQUEST BEHAVIOR CONFIRMED; IMPACT UNVERIFIED | F-04; loopback mock sink only; no sensitive/internal service queried; no CVSS Base score assigned. |
| File upload | CHECKED, NO FINDING | Harmless .txt accepted, but no persistence or execution impact observed. |
| Path traversal | CHECKED, NO FINDING ESTABLISHED | One encoded traversal returned 403; no data returned. |
| LFI | NOT RUN | Candidate data-erasure route has a deletion side effect; not exercised. |
| RFI | NOT APPLICABLE TO OBSERVED FLOWS | No remote include function identified. |
| Manual and Burp validation | PARTIAL | Manual browser/API checks; no native Burp project or automated scan. |
| Validate impact and false positives | COMPLETE FOR SCORED FINDINGS; F-04 IMPACT UNVERIFIED | Baselines and controlled comparisons are recorded; the SSRF sink proof did not establish a security consequence. |
| PoCs and payload list | PARTIAL | Minimal reproduction steps and payload log included for confirmed tests; not every class has a PoC because many were not run. |
| Screenshots/evidence | COMPLETE FOR CONFIRMED FINDINGS | Sanitized PNGs and response summaries under week2/evidence and week3/evidence. |
| CVSS scores | COMPLETE FOR SCORED FINDINGS | Three FIRST CVSS v4.0 scores/vectors in week2/cvss-worksheet.md and final report; F-04 is explicitly unscored because impact was not demonstrated. |
| Remediation notes | COMPLETE FOR CONFIRMED FINDINGS | Each finding and remediation-plan.csv. |

## Week 3 — Authentication, session, access control, and API

| Requirement | Status | Evidence / gap |
|---|---|---|
| JWT weaknesses | PARTIAL | XSS could read the bearer token; replay after client-side clearing confirmed. Signature tampering and fixation were not tested. |
| Session hijacking/fixation | PARTIAL | Synthetic token read/replay behavior documented; no third-party token or real victim session involved. |
| IDOR/BOLA | CONFIRMED | F-05 read-only cross-account basket test with two synthetic accounts. |
| Broken authentication | CONFIRMED | F-02 SQLi login bypass. |
| Excessive data exposure | CHECKED, LIMITED | Own password hash returned only when requested; no cross-user hash access; recorded as observation, not scored. |
| Rate limiting | NOT ESTABLISHED | Only low-volume attempts; no threshold claim. |
| Business logic flaws | NOT RUN COMPREHENSIVELY | No payment or destructive business flows were exercised. |
| API request collection | COMPLETE, SANITIZED | week3/api-collection/collection-template.json; variables are blank and default host is localhost. |
| API findings report | COMPLETE FOR CONFIRMED FINDINGS | week3/findings and week4/final-report.md. |
| IDOR/BOLA PoC | COMPLETE | F-05 reproduction and sanitized screenshot. |
| Auth/session findings | COMPLETE FOR TESTED BEHAVIOR | F-03/F-07 evidence and week1 flow diagram. |
| Impact statements | COMPLETE FOR SCORED FINDINGS; F-04 IMPACT UNVERIFIED | Findings and final report distinguish demonstrated impact from the loopback-only SSRF behavior. |

## Week 4 — Chaining, reporting, and retest

| Requirement | Status | Deliverable / gap |
|---|---|---|
| Chain findings into a realistic attack path | COMPLETE FOR XSS/SESSION PATH | week4/attack-chain.md; each step has local evidence. |
| Final report | COMPLETE | week4/final-report.md and DEPI_Assessment_Professional.pdf. |
| Retest | NOT RUN | No fixes were applied; week4/retest-checklist.csv marks confirmed issues NOT RETESTED. |
| Executive summary | COMPLETE | week4/executive-summary.md and report. |
| Attack-chain diagram | COMPLETE | week4/attack-chain.md. |
| Retest checklist | COMPLETE, PENDING FIXES | week4/retest-checklist.csv. |
| Remediation plan | COMPLETE | week4/remediation-plan.csv. |
| Demo video/screenshots | SCREENSHOTS COMPLETE; VIDEO NOT CREATED | week4/demo-checklist.md. |

## Overall handoff

This pack is ready as a local-lab coursework submission with explicit gaps labeled. A Burp native project, full class coverage, public-host authorization, and retest evidence remain pending.
