# Week 1 - Login and Session Flow (Juice Shop v20.4.0)

1. Browser sends POST /rest/user/login with a synthetic email and password in the JSON body.
2. Server replies 200 with a JWT and account metadata. Secret and account-specific values are not retained in this pack.
3. The frontend stores the JWT in localStorage and in a cookie named token.
4. Authenticated requests may carry both Authorization: Bearer and Cookie: token headers. Follow-up verification on 2026-11-28 found that the cookie is required for GET /rest/user/whoami to identify the account; Bearer alone returned an empty user object.

## Before login vs after login
- Before login: GET /rest/user/whoami returns an empty user object.
- After login: whoami returns the synthetic user's fields when the token cookie is sent. No basket or account identifiers are recorded here.

## Observations
- Session is a JWT, not a server-side session ID.
- Credentials travel in the JSON body (over plain HTTP here, since this is a local lab).
