# Reproduction record — F-01

Target: OWASP Juice Shop 20.4.0-SNAPSHOT, local instance bound to 127.0.0.1:3000
Method/path: GET /rest/products
HTTP status: 500
Response Content-Type: text/html; charset=utf-8
Response title: Error: Unexpected path: /rest/products
Observed: 500 Internal Server Error; the response includes an Express stack trace.
Stack trace present: yes
Absolute local paths in response: present (counted in re-test, not stored)
Screenshot: error-response-redacted.png (absolute local paths replaced with [LOCAL_PATH] before export).
Scope note: this is a deliberately vulnerable training instance; result is lab-only.

## Re-test 2026-11-28

On a fresh local instance, GET /rest/products again returned 500, text/html, with a stack trace div present and absolute path patterns present. No paths, tokens or credentials were stored.
