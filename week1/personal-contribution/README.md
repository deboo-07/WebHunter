# Week 1 contribution

This appendix contains the application map, request inventory, authentication notes, test plan and browser evidence from the separate Week 1 archive.

## DOM XSS observation

The local Juice Shop search interface displayed a browser alert during DOM XSS testing. The alert screenshot supplements team finding F-03. The recommended fix is to render untrusted input as text and review the frontend for unsafe HTML insertion.

[Browser evidence](xss_alert.png) | [Team finding F-03](../../week2/findings/F-03-dom-xss.md)

## Lab notes

The tester recorded issues with the browser extension, requests held by the proxy, and a cached search response with status 304. These observations describe the test environment.
