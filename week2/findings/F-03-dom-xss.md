# F-03 - Search page DOM XSS

**Status:** Confirmed in the local OWASP Juice Shop training lab.
**CVSS v4.0:** 5.3 Medium, as recorded in the team assessment.

The assessment observed browser-side script execution on the search page. Script execution could access session data available to browser JavaScript. The evidence applies to the local lab.

**Remediation:** Render untrusted input as text, review unsafe DOM sinks, and reduce script access to session secrets. After applying a fix, verify that normal search still works and input is rendered safely.

[Sanitized lab evidence](../evidence/F-03/dom-xss-executed.png)
