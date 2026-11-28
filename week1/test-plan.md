# Week 1 test plan and results

## Scope

- Tested: local clone, http://127.0.0.1:3000.
- Local test sink: http://127.0.0.1:3001, used only to confirm the image-fetch behavior.
- Accounts: synthetic accounts created in the local lab.
- Public hosts: no active testing.

## Results

| Check | Result | Evidence |
|---|---|---|
| Home page and product catalog load | PASS | week2/evidence/F-01/home-catalog.png |
| HTTP/API route inventory | PARTIAL | request-response-inventory.csv |
| Login baseline and authentication bypass | PASS; bypass finding F-02 | week2/evidence/F-02/ |
| Login baseline, admin account | PASS; admin + wrong password returned 401 (B2, added 2026-11-28) | week2/evidence/F-02/retest-2026-11-28.json |
| Search SQLi check | NOT CONFIRMED; `q=' OR 1=1--` returned 500, not a confirmed injection (tested 2026-11-28) | week2/evidence/F-02/retest-2026-11-28.json |
| Two-account object ownership comparison | PASS; BOLA finding F-05 | week3/evidence/F-05/ |
| Browser session and token storage | PARTIAL; XSS token access and replay confirmed | week3/evidence/F-07/ |
| Burp project save | PENDING | BURP_PROJECT_STATUS.md |

## Constraints

The local instance uses a development build and seeded challenge data. This does not represent the configuration of the public Heroku host. The Burp native project is pending because Community edition has no disk-based project option.
