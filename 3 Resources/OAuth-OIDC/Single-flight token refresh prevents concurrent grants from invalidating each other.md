---
title: "Single-flight token refresh prevents concurrent grants from invalidating each other"
created: 2026-09-27
type: howto
status: seedling
source: "leo-customer360 SSO session fix, 2026-09-27"
tags: [oauth, oidc, refresh-token, concurrency, technique]
---

# Single-flight token refresh prevents concurrent grants from invalidating each other

When an access token expires, every in-flight API call fails at once — a dashboard that fans out six requests gets six simultaneous 401s. If each one independently starts a refresh, you fire six concurrent refresh grants with the *same* refresh token.

With an IdP that rotates refresh tokens (Keycloak does), that is self-defeating: the first grant consumes the token and issues a new one, and the remaining five now present a retired token. Depending on the provider's replay detection, they either fail outright or invalidate the whole token family — turning a routine renewal into a forced logout.

The fix is a module-level in-flight promise that all waiters attach to, so N concurrent 401s produce exactly one grant:

```js
var refreshInFlight = null;

function refreshSession() {
  if (refreshInFlight) return refreshInFlight;
  if (!CONFIG.refreshToken) return $.Deferred().reject().promise();
  refreshInFlight = rawApi("/auth/refresh", { refresh_token: CONFIG.refreshToken }, "POST")
    .done(function (resp) { setSsoSession(resp); })
    .always(function () { refreshInFlight = null; });
  return refreshInFlight;
}
```

Callback ordering matters: register the `done` that persists the new tokens *before* the `always` that clears the latch, so a waiter resuming off the same promise already sees the fresh access token.

Pairs with a retry wrapper that, on 401, refreshes once and replays the original request — and only ends the session if the *retry* also 401s.

## Related

- [[Discarding the refresh token caps an SPA session at one access-token lifetime]]
