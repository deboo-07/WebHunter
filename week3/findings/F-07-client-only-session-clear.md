# F-07 - Client-side session clearing

**Status:** Tested in the local OWASP Juice Shop training lab. Unscored supporting observation.

The lab evidence shows that a previously issued session token remained valid after session data was cleared from the browser. An authenticated response was observed for the synthetic account. The observation concerns the local training setup.

**Remediation:** Invalidate or revoke server-side session credentials at logout. After remediation, verify that the prior credential is rejected and a fresh login works.

[Sanitized evidence](../evidence/F-07/xss-session-chain.png) | [Validation summary](../evidence/F-07/session-replay.json)
