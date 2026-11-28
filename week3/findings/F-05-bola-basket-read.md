# F-05 — Broken object-level authorization on basket read

## Status and scope

- Status: CONFIRMED in the local training lab; read-only
- Target: OWASP Juice Shop 20.4.0-SNAPSHOT, 127.0.0.1:3000
- Endpoint: GET /rest/basket/{id}
- Test date: 2026-11-23
- Accounts: two synthetic customer accounts owned by the tester
- Registration: synthetic accounts were created through the self-service `POST /api/Users` endpoint (HTTP 201; see `week3/evidence/api-validation.json`).

## Summary

Account B created a basket containing one product row. While authenticated as account A, the tester requested account B's known synthetic basket ID. The API returned HTTP 200 and the populated basket. The request did not enumerate identifiers and did not access anyone else's data.

## Impact demonstrated

An authenticated customer can read another customer's basket when the object identifier is known. The observed object contained one synthetic product row; no personal or payment information was requested.

## CVSS v4.0

- Score: 6.9 Medium
- Vector: CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:N/VC:L/VI:N/VA:N/SC:N/SI:N/SA:N
- Rationale: the local registration endpoint self-provisions accounts, so an attacker can create an account as part of the attack and Privileges Required is None. Only limited read access to another account's basket was demonstrated. Score recalculated with the CVSS v4.0 calculator.

## Evidence and false-positive checks

Evidence: evidence/F-05/cross-account-basket.png and evidence/api-validation.json. Account B's basket was populated first; the response returned the same synthetic object with one product while using account A's token.

## Remediation

- Enforce basket ownership on every read and mutation: compare the authenticated user ID with the basket's owner on the server.
- Return 403/404 for an object that does not belong to the caller.
- Add regression coverage for two-user object access, including read, update, delete, and checkout operations.

## Retest

NOT RETESTED — no fix was applied to the lab build.
