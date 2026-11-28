# Week 1 baseline plan and results

## Scope

- Tested: local clone, http://127.0.0.1:3000.
- Accounts: synthetic accounts created in the local lab.
- Public hosts: no active testing.

## Results

| Check | Result | Evidence |
|---|---|---|
| Home page and product catalog load | PASS | application-map.md |
| HTTP/API route inventory | PARTIAL | request-response-inventory.csv |
| Normal synthetic-account login/session flow | DOCUMENTED | auth-session-flow.md |
| Burp project save | PENDING | BURP_PROJECT_STATUS.md |

Week 1 did not test XSS or SQL injection. Those topics and their evidence are in the Week 2 assessment materials.

## Constraints

The local instance uses a development build and seeded challenge data. This does not represent the configuration of the public Heroku host. The Burp native project is pending because Community edition has no disk-based project option.
