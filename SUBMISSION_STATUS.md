# Submission status — Project 2

## Current state

This pack now contains a real, limited assessment of an isolated OWASP Juice Shop 20.4.0-SNAPSHOT instance on `127.0.0.1:3000`, with synthetic test accounts and sanitized screenshots. It is a lab assessment, not a report about either public URL supplied with the project.

### Confirmed findings and behavior

| ID | Finding / confirmed behavior | Status | CVSS v4.0 Base |
|---|---|---|---|
| F-02 | SQL injection enables administrator login bypass | Confirmed | 9.3 Critical |
| F-03 | Search page DOM XSS; script can read the logged-in token | Confirmed | 5.1 Medium |
| F-04 | Authenticated profile image fetch reaches a tester-controlled loopback service | Request behavior confirmed; impact unverified | Not scored |
| F-05 | Account A can read a populated basket belonging to synthetic account B | Confirmed, read-only; account registration is self-service | 6.9 Medium |

Three findings have CVSS Base scores (one Critical, two Medium). F-04 remains an unscored confirmed request behavior because the local sink proof did not demonstrate confidentiality, integrity, or availability impact. See [the final report](week4/final-report.md) for vectors, scope, impact limits, remediation, and evidence.

## Work products

- Local test evidence and screenshots are under `week2/evidence/` and `week3/evidence/`.
- The API request inventory, auth/session matrix, and a sanitized Postman collection are in `week3/`.
- A requirements traceability matrix is in `REQUIREMENTS_TRACEABILITY.md`.
- A proposed six-person allocation across all four weeks is in [TEAM_WORK_PLAN.md](TEAM_WORK_PLAN.md).
- The per-person repository file list is in [UPLOAD_CHECKLIST.md](UPLOAD_CHECKLIST.md).
- A PDF report is provided alongside the editable Markdown report.

## Items still pending or limited

- **Burp native project: PENDING.** Burp Suite Community was used as the available edition; it does not save disk-based project files. No `.burp` file has been fabricated. See `week1/BURP_PROJECT_STATUS.md`.
- **Public-host testing: NOT PERFORMED.** The Heroku URL was not actively tested because authorization for active testing was not verified. The repository's preview-host warning does not grant permission for the separate Heroku host.
- **Post-fix retest: NOT PERFORMED.** No application fixes were applied. F-01 and F-02 were reproduced again on a fresh local instance on 28 November, and F-06's own-account behavior was checked again; these revalidations do not count as post-fix retesting. The three scored findings and F-04 behavior remain open in this training build.
- CSRF, LFI, RFI, session fixation, broad rate-limit behavior, and business-logic edge cases are marked not run or inconclusive in the test matrix.
- Two observations were not elevated to scored findings: a stack trace on an invalid route in the local development configuration, and an authenticated user's ability to request their own password hash. Neither established meaningful cross-user impact in this run.

## Submission caveat

The vulnerabilities belong to the deliberately vulnerable training application running locally. Do not present them as findings against `juice-shop.herokuapp.com` or another operator. If the course requires an actual `.burp` file or retest results, those remain additional steps outside this Community-edition evidence pack.

## 27–28 November supplement

F-08 documents directory index visibility as an unscored supporting observation. On 28 November, F-01 and F-02 were revalidated on a fresh local instance, and F-06's own-account behavior was checked again. The same review corrected CVSS metrics for F-02, F-03, and F-05 and left F-04 unscored pending demonstrated impact. No application fix was applied, so post-fix retesting remains pending. See the final report and CVSS worksheet.

## Deliverables in this archive

- [Professional PDF assessment](DEPI_Assessment_Professional.pdf)
- [Editable final report](week4/final-report.md)
- [Week 1 contribution](week1/personal-contribution/README.md)
- [Updated test coverage](week2/test-plan.md)

CSRF remains inconclusive and login rate-limit coverage remains incomplete. Native Burp project and post-fix retest remain pending.

See [Open items and acceptance criteria](OPEN_ITEMS.md).
