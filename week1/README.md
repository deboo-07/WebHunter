# Week 1 — Web Fundamentals & Burp Setup

## Work completed

- Studied the observed app flow on a local OWASP Juice Shop 20.4.0-SNAPSHOT instance bound to `127.0.0.1:3000`.
- Recorded a compact route map, HTTP inventory, and authentication/session flow from the local run.
- Reproduced the UI and API flows with the browser and direct HTTP requests; sanitized screenshots and request summaries are in later-week evidence folders.
- Week 1 is limited to baseline mapping and setup. XSS and SQL injection testing are excluded from this week's scope; their separate Week 2 results remain documented in the Week 2 materials.
- Burp Suite Community was available for manual proxy/repeater work. A native project file is **PENDING** because this edition does not save disk-based Burp projects. See [BURP_PROJECT_STATUS.md](BURP_PROJECT_STATUS.md).

## Scope

Active testing in this submission was limited to the local Juice Shop clone at `127.0.0.1:3000` and a temporary test-only HTTP sink at `127.0.0.1:3001`. Synthetic accounts and data were used. Neither public URL provided with the project was subjected to active payload testing.

## Deliverables

- [Application map](application-map.md)
- [Request/response inventory](request-response-inventory.csv)
- [Auth/session flow](auth-session-flow.md)
- [HTTP fundamentals notes](http-fundamentals.md)
- [Test plan and outcomes](test-plan.md)
- [Burp project status](BURP_PROJECT_STATUS.md)
- [Six-person work plan for all four weeks](../TEAM_WORK_PLAN.md)
