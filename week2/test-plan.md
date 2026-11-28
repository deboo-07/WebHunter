# Week 2 test plan and results

## Authorization and scope record

- Target: local OWASP Juice Shop 20.4.0-SNAPSHOT, 127.0.0.1:3000.
- Controlled callback: temporary HTTP sink, 127.0.0.1:3001, used for one profile image fetch.
- Accounts/data: synthetic accounts and one synthetic basket item.
- Public URLs: no active payload testing.
- Method: manual, low-volume, one input at a time. No broad scanning, brute force, destructive file operations, or external SSRF targets.

## Test matrix

| Class          | Test and result                                                                                                                                              | Status                                 | Evidence                                                    |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- | ----------------------------------------------------------- |
| SQL injection  | Baseline login returned 401. Two controlled boolean SQLi requests in the email field returned 200 and authenticated as the seeded administrator.             | CONFIRMED — F-02                       | evidence/F-02/reproduction.json and sql-injection-login.png |
| XSS            | Search query payload executed a harmless DOM marker. In a logged-in synthetic session, it could read the token from localStorage.                            | CONFIRMED — F-03                       | evidence/F-03/dom-xss-executed.png                          |
| CSRF           | No browser-backed cross-site state-change proof completed.                                                                                                   | NOT RUN                                | —                                                           |
| SSRF           | Authenticated profile image fetch reached only the tester-controlled 127.0.0.1:3001 sink. No sensitive response, state change, or service impact was demonstrated. | BEHAVIOR CONFIRMED — F-04; impact unverified; not scored | evidence/F-04/controlled-local-sink.png                     |
| File upload    | Harmless 25-byte .txt accepted with HTTP 204; no persistence or execution observed.                                                                          | CHECKED — no scored finding            | evidence/input-class-validation.json                        |
| Path traversal | One encoded slash attempt to the public file route returned 403.                                                                                             | CHECKED — no finding established       | evidence/input-class-validation.json                        |
| LFI            | No safe file-include proof completed; data-erasure route was not exercised because it creates a deletion request.                                            | NOT RUN                                | —                                                           |
| RFI            | No remote include flow identified in the tested routes.                                                                                                      | NOT APPLICABLE                         | —                                                           |
| Verbose error  | Invalid/unsupported API path returned 500 with stack details in development mode; paths redacted.                                                            | OBSERVATION — not scored               | evidence/F-01/error-response-redacted.png                   |

## False-positive checks

- SQLi: ordinary nonexistent-user baseline returned 401; two different invalid password values with the same SQL expression returned administrator login.
- XSS: verified DOM marker executed in the browser, not merely reflected as text.
- SSRF: the only destination was a temporary local mock server; its request counter incremented. No security impact was demonstrated, so no CVSS Base score is assigned.
- Upload: accepted status alone was not treated as exploitation; no stored or executable content was demonstrated.
- Traversal: response was 403 and no file content was returned.

## Remaining limitations

The broad CSRF, LFI, RFI, upload-impact, and path traversal classes are not comprehensively assessed. This is a bounded lab exercise, not a full test of every Juice Shop challenge.

## Supplemental test on 27 November 2026

Directory listing observed in the local lab. See [F-08 evidence](evidence/F-08/directory-listing.png) and [HTTP history](evidence/F-08/http-history.png).

## Additional coverage recorded on 27 November 2026

| Area | Status | Evidence or limit |
|---|---|---|
| Search input checks | Performed | Response lengths varied. |
| Directory listing | Confirmed | [HTTP response](evidence/F-08/http-history.png) and [browser view](evidence/F-08/directory-listing.png). |
| Unauthenticated directory access | Observed in the Incognito test | Scope limited to access observed during the local test. |
| File name disclosure | Confirmed | The index displayed filenames. |
| Security misconfiguration | Confirmed | Classification of F-08. |
| LFI | Unconfirmed | Reviewed evidence does not establish file inclusion. |
| Path traversal | Unconfirmed | Reviewed evidence does not establish traversal. |
| CSRF | Inconclusive | Profile data changed. A browser-initiated cross-site change remains unverified. |
| Login rate limiting | Incomplete | One failed login returned HTTP 401. Behavior across repeated attempts remains undetermined. |
