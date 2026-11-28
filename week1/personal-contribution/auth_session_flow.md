# Week 1 - Login and Session Flow (Juice Shop v20.4.0)

1. Browser sends POST /rest/user/login with JSON body: email + password.
2. Server replies 200 with authentication object: token (JWT), bid (basket ID), umail (email).
3. Browser stores the JWT in a cookie named token.
4. Every later request carries the JWT in two places: Authorization: Bearer <token> header and the token cookie.
5. Server identifies the user from the JWT (e.g. /rest/user/whoami, /rest/basket/6).

## Before login vs after login
- Before login: GET /rest/user/whoami returns an empty user object.
- After login: whoami returns the user, and GET /rest/basket/6 works (UserId 26).

## Observations
- Session is a JWT, not a server-side session ID.
- Credentials travel in the JSON body (over plain HTTP here, since this is a local lab).
