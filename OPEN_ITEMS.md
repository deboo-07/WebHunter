# Open items and acceptance criteria

| Item | Current status | Evidence needed for closure |
|---|---|---|
| CSRF | Inconclusive. The profile update was observed. | Document the implemented protection and verify that the server rejects unintended cross-site changes while normal profile updates still work. |
| Login rate limiting | Incomplete. One failed login returned HTTP 401. | Record the configured policy and defensive test results showing that it is enforced while legitimate sign-ins remain available. |
| Native Burp project | Pending. The package contains the Community-edition status and exported evidence. | Include a genuine native project saved by an edition that supports project files, if required by the rubric. |
| Post-fix retest | Pending. No fixed build is identified in the submitted evidence. | Record the fixed build, corrected behavior, checks of normal functionality, and a pass or fail for each applicable finding. |
