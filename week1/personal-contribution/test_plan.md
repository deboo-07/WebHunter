# Week 1 - Baseline Test Plan (Juice Shop v20.4.0)

Scope: local training instance at `http://127.0.0.1:3000`; normal navigation and synthetic-account flows only.

## Week 1 checks

1. Map the main pages and visible routes through normal browser navigation.
2. Record HTTP methods, paths, status codes, and content types for ordinary requests.
3. Observe registration and valid login with a synthetic account; document where the session token is stored and sent without copying its value.
4. Configure Burp Proxy and record whether traffic is captured.
5. Review and sanitize the notes before consolidating them into the Week 1 pack.

## Out of scope for Week 1

- XSS payload testing.
- SQL injection payload testing or authentication bypass attempts.
- Testing against public hosts.

The XSS and SQL injection assessments are documented separately in Week 2.
