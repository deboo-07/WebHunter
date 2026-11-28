# Week 1 - Test Plan (Juice Shop v20.4.0)

Scope: http://localhost:3000 (local lab only)

## Priority targets
1. /rest/products/search?q= : test the q parameter for injection and XSS.
2. /rest/user/login : test authentication logic and error messages.
3. JWT token : check how it is built and whether the server validates it properly.
4. /rest/basket/{id} : test whether changing the ID exposes another user's basket (IDOR).
5. /rest/admin/ : test what the admin endpoints expose without login.
6. /api/Users/ : test registration fields and what the response reveals.
7. /rest/products/{id}/reviews : test whether the review author data can be abused.

## Order of work
1. DOM XSS on the search feature (next task).
2. Authentication and JWT checks.
3. Access control checks (IDOR on basket, admin paths).
4. Review the remaining endpoints in the site map.

## Tools
- Burp Proxy and HTTP history for capture
- Burp Repeater for manual testing
