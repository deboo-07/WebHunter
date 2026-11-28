# HTTP/HTTPS quick notes

## Request/response shape

An HTTP request has a method and path, headers, and sometimes a body. A response has a status code, headers, and sometimes a body. The browser and server exchange these messages; Burp sits between them when the browser is configured to use its proxy.

```http
GET / HTTP/1.1
Host: juice-shop.herokuapp.com
Accept: text/html
Cookie: example=value
```

## Methods to recognize

- `GET`: retrieve a resource; normally should not change server state.
- `POST`: submit data or request an action, often with a body.
- `PUT` / `PATCH`: replace / partially update a resource.
- `DELETE`: remove a resource.
- `OPTIONS`: discover communication options; often used for CORS preflight.

Do not assume a method is safe just from its name; observe the application behavior.

## Headers, cookies, and browser storage

Headers carry metadata such as `Host`, `Origin`, `Referer`, `Accept`, `Content-Type`, `Authorization`, `Cookie`, and response `Set-Cookie`, cache, and CORS headers. A cookie is sent on later requests when its domain/path and security rules match. Check attributes such as `HttpOnly`, `Secure`, and `SameSite`.

Apps may also keep tokens or identifiers in browser storage (Local Storage or Session Storage) and send them in headers such as `Authorization: Bearer …`. Storage is not automatically sent like a cookie: application code usually reads it and adds it to a request. Record the actual mechanism without copying live secrets into reports.

## Sessions and authentication

A common flow is: submit credentials → server validates → server returns a session cookie or token → browser stores it → later requests include it → logout/expiry invalidates it. Capture this only with the lab account you create/use. Compare requests before and after login, and after logout. Never include raw passwords, tokens, or session cookies in screenshots or reports.

## HTTPS and Burp

HTTPS protects traffic between the browser and the TLS endpoint. To inspect HTTPS through Burp, the browser trusts Burp's local CA certificate and Burp creates a separate TLS connection to the site. For the selected HTTPS Juice Shop target, Burp certificate trust is needed if you inspect TLS traffic in a dedicated browser. Use Burp's built-in browser if available. Do not bypass browser certificate warnings for unrelated sites.

## Status-code groups

- `2xx`: request succeeded.
- `3xx`: redirect or other redirection behavior.
- `4xx`: client/request/auth/access issue (for example `401`, `403`, `404`).
- `5xx`: server-side error; record exact request and response and avoid assuming it proves a vulnerability.

## Burp tools

- **Proxy / HTTP history:** observe browser traffic.
- **Target / Site map:** organize discovered hosts, paths, and parameters.
- **Repeater:** manually resend and compare a single request at a time.
- **Intruder:** automate payload placement; in this first week, keep it to tiny, low-rate lab-only checks. No credential brute force.
