# Week 2 — Input-Based Vulnerabilities

## Scope

Tests were run on an isolated local OWASP Juice Shop 20.4.0-SNAPSHOT at 127.0.0.1:3000. A temporary local sink on 127.0.0.1:3001 was used only for the SSRF validation. No active tests were sent to either public URL.

## Confirmed findings and behavior

| ID | Class | Result | Evidence |
|---|---|---|---|
| F-02 | SQL injection | Login bypass to the seeded administrator account | evidence/F-02/ |
| F-03 | DOM XSS | Search parameter executed script in browser; when logged in, script read the synthetic account token from localStorage | evidence/F-03/ |
| F-04 | SSRF behavior | Authenticated image URL fetch reached the tester-controlled loopback sink; impact was not demonstrated, so it is unscored | evidence/F-04/ |

## Other checks

- F-04 confirms server-side URL-fetch behavior to the controlled loopback sink only. No confidentiality, integrity, or availability impact was demonstrated, so it has no CVSS Base score.
- File upload: harmless .txt accepted with HTTP 204, but no persistence or execution was observed; not scored as a finding.
- Path traversal: one encoded slash traversal attempt returned 403; no file content was returned.
- CSRF and LFI: not run; no safe end-to-end test was completed in this pass.
- RFI: not applicable to the observed Juice Shop flow; SSRF is a different class.
- Verbose error: a stack trace appeared on an invalid/unsupported route in this local development configuration. Absolute paths are redacted; treated as an observation, not a scored production finding.

See the test matrix, payload log, and Week 4 report.

See [the updated six-person allocation](../TEAM_WORK_PLAN.md) for Week 2/3 finding leads and peer-review assignments.
