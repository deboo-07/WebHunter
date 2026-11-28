# F-07 and F-08: Session replay after client-side clearing, and directory index visibility

Supporting observations from the session and file-exposure checks. Neither is scored on its own: F-07 completes the F-03 XSS chain, and F-08 is an unscored supporting note. The one scored finding in this part of the work is F-03, in the Week 2 report.

Both tests ran against OWASP Juice Shop 20.4.0-SNAPSHOT at http://127.0.0.1:3000, a local lab with synthetic accounts only.

---

# F-07: Client-side logout does not revoke an issued token

| Field | Value |
|---|---|
| ID | F-07 |
| Severity | Not scored separately, reported as part of the F-03 chain |
| Target | Local lab, authenticated account API |
| Assessment date | 2026-11-23 |
| Account | Synthetic account, single test session |
| Status | Confirmed session behavior, open, not retested |

## Summary

After browser session data was cleared, an already issued credential still received HTTP 200 for the same synthetic account. The report records the result without retaining the credential.

## Root Cause

Clearing the browser's copy of the token does not end the session: the issued token stayed valid on the server after localStorage and the token cookie were cleared. The app configures a six-hour token expiry, and the test did not wait it out, so how long the token stays accepted was not measured. The observed design has no server-side revocation for this path. Because the token lives in localStorage, any same-origin script can hold a copy that outlives the browser's own.

## Evidence

- [session-replay.json](evidence/F-07/session-replay.json): the sanitized replay summary (requests, status codes, no token value).
- [xss-session-chain.png](evidence/F-07/xss-session-chain.png): summary image of the read-and-replay chain, not a raw capture.
- `week3/api-inventory.csv`, row API-009: the endpoint, fields, and HTTP 200 result.

## Impact

This confirms the session remains usable after the browser forgets it, so a token read through F-03 keeps working after the user clears their browser. It does not by itself prove how every logout path behaves in every deployment. The test used one synthetic account, did not wait out the configured six-hour expiry, and did not test a real server-side logout endpoint. It is therefore supporting evidence for F-03 rather than a separately scored finding.

## Fix

- Support server-side revocation or token rotation when logout is expected to invalidate a session.
- Reduce the token lifetime.
- Stop exposing bearer tokens to same-origin script, using an HttpOnly, Secure, SameSite cookie instead.

Retest: NOT RETESTED.

---

# F-08: Directory index visible at /ftp/

| Field | Value |
|---|---|
| ID | F-08 |
| Severity | Not scored, supporting observation |
| Target | Local lab, /ftp/ |
| Test date | 2026-11-27 |
| Browser | Incognito window, Burp recording |
| Status | Observation only, open, retest pending |

## Summary

The local application's `/ftp/` page displayed a directory index in an Incognito window. Burp recorded HTTP 200 for the index. The tester also opened `legal.md`. Access to the contents of the other listed files was not verified.

## Root Cause

The static file directory is served with directory indexing enabled, so a request for the folder returns a generated listing instead of being refused or redirected to an index page. Nothing filters the request to a set of intended files.

## Evidence

- [http-history.png](../week2/evidence/F-08/http-history.png): Burp HTTP history showing HTTP 200 for the index.
- [directory-listing.png](../week2/evidence/F-08/directory-listing.png): the directory index as displayed.
- `week4/final-report.md`, section 6: the written observation.

The F-08 evidence sits in the Week 2 evidence folder. There is no screenshot of `legal.md` itself, and the contents of the other listed files were not opened.

## Impact

The index discloses the filenames present in that directory, which helps an attacker choose what to request next. It was not shown that the other listed files can be read, and no local file inclusion or path traversal was established: a traversal check returned HTTP 403 with no file data (`week2/payload-log.csv`, row W2-006). Because the disclosure is limited to filenames on a local training build, this stays an unscored observation.

## Fix

- Disable directory indexing on the server.
- Publish only intended files explicitly, and review what currently sits in `/ftp/`.
- Serve a defined index page or return 403/404 for the folder itself.

Retest: pending.
