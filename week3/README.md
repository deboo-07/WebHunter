# Week 3 — Authentication, Session, Access Control & API

## Scope and results

Tests used two synthetic customer accounts and a separate synthetic account for the browser session check, all on the local Juice Shop instance. No real-user identifiers or data were used.

| Area | Result | Evidence |
|---|---|---|
| SQLi broken authentication | Confirmed in Week 2 as F-02 | ../week2/evidence/F-02/ |
| JWT storage and XSS readability | Confirmed as part of F-03 chain | evidence/F-07/ |
| Client-side logout / old-token replay | Old token returned HTTP 200 after local storage and cookie were cleared; value not saved | evidence/F-07/ |
| Basket object authorization | Account A read account B's populated synthetic basket; read-only | evidence/F-05/ |
| Own password hash field | Explicit request returned the authenticated user's hash; no cross-user data; recorded as an observation only | evidence/F-06/ |
| Rate limiting | No throttling threshold established; only a few low-volume login attempts | NOT RUN / INCONCLUSIVE |
| Business logic and session fixation | Not comprehensively tested | NOT RUN |

## Deliverables

- API request inventory: api-inventory.csv
- Auth/session matrix: auth-session-matrix.csv
- Sanitized Postman collection: api-collection/collection-template.json
- Findings and limitations: findings/
- Sanitized evidence: evidence/

Burp project remains PENDING in Community edition; see ../week1/BURP_PROJECT_STATUS.md.

See [the updated six-person allocation](../TEAM_WORK_PLAN.md) for Week 2/3 finding leads and peer-review assignments.
