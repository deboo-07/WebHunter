# Repository Structure

This is the target layout after all team members upload their assigned files. The initial repository setup contains the root planning files and week-folder placeholders; each owner fills their folder using the upload checklist.

```text
/
├── README.md
├── REPO_STRUCTURE.md
├── TEAM_WORK_PLAN.md
├── UPLOAD_CHECKLIST.md
├── SUBMISSION_STATUS.md
├── REQUIREMENTS_TRACEABILITY.md
├── OPEN_ITEMS.md
├── DEPI_Assessment_Professional.pdf
├── week1/                     # رحمه — mapping and setup
├── week2/                     # إيمان, أحمد, حمد, همام — input tests
│   ├── findings/
│   └── evidence/
├── week3/                     # same four — auth, API, and access control
│   ├── api-collection/
│   ├── findings/
│   └── evidence/
└── week4/                     # دبيان — reporting and handoff
    ├── findings/
    └── evidence/
```

## Ownership

- **Week 1:** رحمه owns the map, request inventory, normal auth/session notes, baseline plan, and Burp status.
- **Weeks 2 and 3:** إيمان owns F-02 SQL injection; أحمد owns F-03 XSS and its F-07 session chain; حمد owns F-04 SSRF; همام owns F-05 BOLA. Each person contributes during both weeks.
- **Week 4:** دبيان integrates the finding handoffs into the report, remediation plan, retest checklist, traceability, and final package.

The folder and file assignments are listed in [UPLOAD_CHECKLIST.md](UPLOAD_CHECKLIST.md).

## Integration rule

`week1/application-map.md` and `week1/request-response-inventory.csv` are the single canonical Week 1 files in the consolidated pack. Week 1 contains baseline mapping and normal-flow records; later-week payloads and findings stay in `week2/` and `week3/`. When integrating parallel pull requests, update these canonical files in one branch and rebase dependent branches instead of adding the same paths again.
