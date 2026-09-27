---
title: "Blanket 401 auto-logout swallows the login endpoint's own error"
created: 2026-09-27
type: lesson
status: seedling
source: "leo-customer360 SSO session fix, 2026-09-27"
tags: [auth, api-client, ux, gotcha]
---

# Blanket 401 auto-logout swallows the login endpoint's own error

A shared API client that reacts to *any* 401 by clearing storage and reloading the page will also catch the 401 that means "wrong password". The login form's `showError("Invalid username or password")` never renders, because the page reloads out from under it — so a user typing a bad password sees the login screen blink and silently reset, with no explanation.

The same handler breaks a refresh flow more seriously: a 401 from the refresh endpoint triggers a logout+reload *while* the refresh is being awaited, and a 401 from the login/callback endpoint could recurse into another refresh attempt.

The distinction to encode: on `/auth/*` endpoints a 401 **is the answer**, not a symptom of a stale token. Exclude them from both the auto-logout and the auto-refresh paths, and let the caller render the error.

```js
function isSessionEndpoint(path) {
  return String(path || "").indexOf("/auth/") === 0;
}
```

General principle: "401 means my session died" is only true for *resource* endpoints. For endpoints whose job is to establish or renew a session, 401 is an ordinary domain response the caller asked for.

## Related

- [[Single-flight token refresh prevents concurrent grants from invalidating each other]]
