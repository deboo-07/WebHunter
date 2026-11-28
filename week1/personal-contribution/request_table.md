# Week 1 - Request Table (Juice Shop v20.4.0)

| Method | Endpoint | Status | Notes |
|---|---|---|---|
| GET | /rest/products/search?q= | 200 | Returns all products as JSON, no login needed |
| GET | /rest/products/1/reviews | 200 | Returns reviews, exposes user emails (e.g. admin@juice-sh.op), no login needed |
| GET | /rest/user/whoami?fields=email | 200 | Returns empty user when not logged in |
| POST | /api/Users/ | 201 | Registers a new account |
| POST | /rest/user/login | 200 | Body: email + password (JSON). Response: token (JWT), bid, umail |
| GET | /rest/basket/6 | 200 | Needs Authorization: Bearer + token cookie. Returns the basket (UserId 26) |
| GET | /rest/admin/application-version | 200 | Admin path, reachable without login |
