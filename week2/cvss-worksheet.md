# CVSS v4.0 scoring record

Scores are Base scores for demonstrated impact in the local training app; no environmental adjustment is applied. Revised 2026-11-28: F-02 sets availability to None because no availability impact was demonstrated; F-03 uses Active user interaction because exploitation requires opening a crafted link; F-05 uses Privileges Required None because registration is self-service. F-04 remains unscored because the local-sink proof did not demonstrate a confidentiality, integrity, or availability impact.

| Finding | Score / rating | Vector | Notes |
|---|---|---|---|
| F-02 SQLi login bypass | 9.3 Critical | CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:H/VI:H/VA:N/SC:N/SI:N/SA:N | Admin login established; no availability impact was demonstrated. Corrected 2026-11-28; score unchanged. |
| F-03 DOM XSS and token access | 5.1 Medium | CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:A/VC:N/VI:N/VA:N/SC:L/SI:L/SA:N | Opening a crafted link requires conscious user interaction; token value was never transmitted. |
| F-04 Authenticated SSRF behavior | Not scored | — | The controlled loopback sink received a request; no restricted data disclosure, modification, or availability impact was demonstrated. |
| F-05 Basket BOLA read | 6.9 Medium | CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:N/SI:N/SA:N | Account registration is self-service; limited read of one tester-owned synthetic basket. |

F-01, F-04, F-06, and F-07 are not assigned separate Base scores in this pack. F-04 is confirmed server-side request behavior with impact unverified; F-01, F-06, and F-07 remain supporting observations. See the final report for limitations.

References: [FIRST CVSS v4.0 specification](https://www.first.org/cvss/v4.0/specification-document), [FIRST CVSS v4.0 FAQ on XSS interaction](https://www.first.org/cvss/v4.0/faq), and the [FIRST CVSS v4.0 calculator](https://www.first.org/cvss/calculator/cvsscalc40).
