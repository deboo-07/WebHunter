# Ahmed's demo notes | F-07 and F-08

## F-07 | Session handling

Open the [evidence summary](evidence/F-07/xss-session-chain.png), then the [finding](findings/F-07-client-only-session-clear.md). In one local synthetic session, the server returned HTTP 200 for a previously issued credential after browser session data was cleared. This is an unscored supporting observation linked to F-03. The test covered client-side clearing. Server-side logout behavior and expiry were outside the verified scope.

**Fix:** Support server-side session invalidation where logout is expected to end the session.

## F-08 | Directory listing

Open the [Incognito screenshot](../week2/evidence/F-08/directory-listing.png) first, then the [Burp HTTP history](../week2/evidence/F-08/http-history.png). On 27 November 2026, the local lab displayed filenames and Burp recorded HTTP 200. The test also opened legal.md. The other listed file contents were unverified. F-08 is an unscored supporting observation.

**Fix:** Disable directory indexing and publish only intended files. Post-fix retest is pending.
