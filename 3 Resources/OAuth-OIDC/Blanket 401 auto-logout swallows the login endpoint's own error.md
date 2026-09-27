---
ai_hash: 3783a5667d89985e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: leo-customer360 SSO session fix, 2026-09-27
status: seedling
tags:
- auth
- api-client
- ux
- gotcha
title: Blanket 401 auto-logout swallows the login endpoint's own error
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Single-flight token refresh prevents concurrent grants from invalidating each other]]
- [[Returning 401 for a permission failure causes infinite login loops]]
- [[Discarding the refresh token caps an SPA session at one access-token lifetime]]
- [[Fail-open bearer auth middleware antipattern]]
- [[A null-guarded tenant check fails open, so a renamed path parameter disables isolation]]

%% ai-graph-end %%