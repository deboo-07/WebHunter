# Four-Week Team Work Plan — Six Members

This is the updated proposed allocation for the current Week 1–4 deliverables. رحمه leads Week 1, دبيان leads Week 4, and إيمان، أحمد، حمد، همام each own one confirmed finding across both Weeks 2 and 3. This plan does not claim who completed the historical work already in the pack. See [UPLOAD_CHECKLIST.md](UPLOAD_CHECKLIST.md) for the exact repository paths assigned to each person.

## Allocation by member

| Member | Week 1 | Week 2 | Week 3 | Week 4 |
|---|---|---|---|---|
| **رحمه — Week 1 lead** | Own the Week 1 map, request inventory, normal auth/session flow, Burp status, baseline plan, and handoff. | — | — | — |
| **إيمان — F-02 SQLi lead** | — | Validate the local SQLi baseline and controlled F-02 reproduction; update the payload/reproduction evidence. Also check F-01 verbose-error evidence. | Trace the authentication/API impact and limits; document the F-06 own-password-hash observation as a separate unscored item. Finalize the report input by the end of Week 3. | — |
| **أحمد — F-03 XSS lead** | — | Verify the harmless DOM marker and token-read behavior in a synthetic session; sanitize browser evidence. Also review F-08 directory-listing evidence. | Complete the F-07 token replay/session-chain notes and connect them to F-03 without treating F-07 as a separate scored finding. Finalize the report input by the end of Week 3. | — |
| **حمد — F-04 SSRF lead** | — | Validate the authenticated fetch only against the tester-controlled loopback sink; record the request and impact limits. Also check harmless file-upload behavior. | Review the API record and define the SSRF impact boundary and retest case; do not add any unverified destination or impact claim. Finalize the report input by the end of Week 3. | — |
| **همام — F-05 BOLA lead** | — | Prepare the two-synthetic-account ownership matrix and test data; record the normal owner baseline, without claiming a cross-account flaw before the Week 3 check. Also check the bounded path-traversal result. | Run the controlled, read-only cross-account basket check and document F-05 evidence and limits. Finalize the report input by the end of Week 3. | — |
| **دبيان — Week 4 lead** | — | — | — | Own Week 4: assemble the final report, executive summary, attack chain, CVSS references, remediation/retest tables, demo checklist, traceability, and submission package from the Week 1 handoff and the four completed finding packages. |

The four middle members each have one primary confirmed finding and a supporting check. Each has work in **both Week 2 and Week 3**: Week 2 validates or prepares the finding; Week 3 completes its API/session analysis, controlled proof, or impact boundary.

## Week 2 and Week 3 collaboration

The finding lead owns the technical notes and evidence for their finding, while another middle member reviews the comparison, redaction, and limits. Rotate reviewers so every lead both gives and receives review:

| Week | Lead / finding | Peer reviewer |
|---|---|---|
| 2 | إيمان — F-02 SQLi and F-01 observation | همام |
| 2 | أحمد — F-03 XSS and F-08 observation | إيمان |
| 2 | حمد — F-04 SSRF and file-upload check | أحمد |
| 2 | همام — F-05 test preparation and traversal check | حمد |
| 3 | إيمان — F-02 auth/API impact and F-06 observation | أحمد |
| 3 | أحمد — F-03/F-07 session chain | همام |
| 3 | حمد — F-04 API context and retest case | إيمان |
| 3 | همام — F-05 two-account proof | حمد |

The four middle members also maintain their own rows in the shared test matrix and API inventory. Before Week 4, they jointly check that the four findings have consistent IDs, evidence links, impact limits, and remediation steps.

## Handoffs and scope

- رحمه completes and hands off the Week 1 scope and baseline materials before Week 2 starts.
- إيمان، أحمد، حمد، همام finish their evidence-backed finding packages, remediation notes, and honest coverage limits by the end of Week 3. Their assigned work is limited to Weeks 2 and 3.
- دبيان owns Week 4 and assembles the package from those handoffs and the existing evidence; no Week 2/3 lead is assigned a Week 4 task.
- دبيان preserves the recorded findings and statuses; the report must not claim a test result or retest that was not completed.
- Keep active testing inside the isolated local lab with synthetic data. Do not test public URLs without verified authorization.
- Preserve incomplete-work status: no fixes have been applied, so retests remain **NOT RETESTED**; the Burp native project remains **PENDING** in Community edition. Keep tokens, passwords, hashes, and personal data out of shared evidence.

## Review of the current four-week pack

- Four findings are confirmed in the local training app: F-02 SQL injection, F-03 DOM XSS, F-04 loopback-only SSRF, and F-05 read-only BOLA. F-01, F-06, and F-08 are supporting observations; F-07 is chain evidence, not a separate scored finding.
- Public-host testing was not performed. The current evidence must not be attributed to either public URL.
- Retest evidence is pending because no fixes were applied. CSRF remains inconclusive; LFI/RFI, session fixation, broad rate-limit behavior, and business-logic coverage remain incomplete or not run. The demo checklist has screenshots, but no video.
- The professional PDF is `DEPI_Assessment_Professional.pdf` in the project root; it is not a `week4/final-report.pdf` file.

