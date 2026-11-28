# F-01 — Verbose error response (local development observation)

**Classification:** observed behavior; not a scored production finding.

- Target: OWASP Juice Shop 20.4.0-SNAPSHOT, local instance at http://127.0.0.1:3000
- Test dates: 2026-11-23 (original), 2026-11-28 (re-test from scratch)
- Endpoint: GET /rest/products
- Status: CONFIRMED as observed behavior in the local lab only

## What was observed

GET /rest/products returned HTTP 500 with an error page:

- Status: 500 Internal Server Error
- Content-Type: text/html; charset=utf-8
- Title: `Error: Unexpected path: /rest/products`
- Body contains an Express stack trace (`#stacktrace` div) with absolute local build paths. The paths are replaced with `[LOCAL_PATH]` in the screenshot.

The route is unsupported in this version, so the test did not demonstrate a flaw in a valid business operation. This is recorded as an observation, not a scored finding.

Evidence: `../evidence/F-01/error-response-redacted.png` (stack trace, paths redacted), `../evidence/F-01/reproduction.md`, `../evidence/F-01/retest-2026-11-28.json`.

The other screenshots in this folder are context only:
- `error-handling-challenge.png` — the Error Handling challenge in the lab
- `home-catalog.png` — the lab home page loading normally

## Re-test 2026-11-28

On a fresh local instance, GET /rest/products again returned 500, Content-Type text/html, with a stack trace present and absolute `/home/...` path patterns present in the response (counted, not stored). No paths, tokens or credentials are stored in evidence.

## Recommended improvement

Return a generic 404/500 response to clients and keep detailed traces in protected server logs. Recheck the production error-handling configuration separately — this lab result must not be presented as a production finding.

Retest: NOT RETESTED after a fix — no fix was applied to the lab build.
