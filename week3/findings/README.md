# Week 3 findings and observations

- F-05 — Cross-account basket read (BOLA): confirmed using two synthetic accounts; read-only; CVSS v4.0 6.9 Medium (self-service account registration; PR:N).
- F-06 — Authenticated user can request their own password hash: confirmed API behavior, no separate impact established, not scored.
- F-07 — Client-side token clearing does not revoke the issued token: confirmed in the synthetic XSS chain, not scored separately.

See ../../week4/final-report.md for the overall report and limitations.
