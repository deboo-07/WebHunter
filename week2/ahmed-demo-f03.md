# Ahmed's demo notes | F-03

**Scope:** Local OWASP Juice Shop training lab. Team finding confirmed on 23 November 2026. CVSS v4.0: 5.3 (Medium), as recorded in the team assessment.

1. Open the [F-03 screenshot](evidence/F-03/dom-xss-executed.png) and point to the marker in the search results.
2. Explain that the frontend treated untrusted search text as HTML. Browser-side script execution exposed session data accessible to page scripts in the local test.
3. Open the [finding](findings/F-03-dom-xss.md). Explain that the fix is to render the query as text and remove the unsafe HTML bypass. A post-fix retest is pending.

Continue with the [F-07 and F-08 demo notes](../week3/ahmed-demo-f07-f08.md).
