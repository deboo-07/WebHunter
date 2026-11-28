# API collection index

Create or export a Burp collection/Postman collection only from an authorized local or explicitly scoped lab. Keep requests minimal and remove secrets before sharing.

## Suggested folders

- `01-public-read`: unauthenticated read-only endpoints
- `02-auth-session`: login/logout/refresh flows using lab accounts; redact credentials and tokens
- `03-account-a`: operations and synthetic objects owned by tester account A
- `04-account-b`: operations and synthetic objects owned by tester account B
- `05-retest`: minimal requests for confirmed findings and fixes

## Collection handling

- Store environment variables for base URL and tokens locally; never commit live secrets.
- Use placeholders such as `{{BASE_URL}}` and `{{TOKEN_A}}` in shared copies.
- Do not include requests to external services or public targets without explicit authorization.
- Record the source of each request and link it to an API inventory row.
