---
title: "Discarding the refresh token caps an SPA session at one access-token lifetime"
created: 2026-09-27
type: lesson
status: seedling
source: "leo-customer360 SSO session fix, 2026-09-27"
tags: [oauth, oidc, spa, session, gotcha]
---

# Discarding the refresh token caps an SPA session at one access-token lifetime

If a single-page app persists only `access_token` and `id_token` from the token response and drops `refresh_token` on the floor, its session can never outlive one access-token lifetime — regardless of how generous the identity provider's session settings are. There is simply no credential left with which to ask for a new token.

This is a quiet bug because nothing errors. The code looks complete, the login works, and the symptom only shows up as a user complaint ("it keeps logging me out") whose duration exactly matches `accessTokenLifespan`. That exact-match is the diagnostic tell: if the forced re-login interval equals the token lifetime to the minute, suspect a missing refresh path before suspecting the IdP.

Where it hides — a persist function that enumerates the fields it cares about:

```js
function setSsoSession(tokenResponse) {
  localStorage.setItem(KEYS.accessToken, tokenResponse.access_token || "");
  localStorage.setItem(KEYS.idToken, tokenResponse.id_token || "");
  // refresh_token was in the response and is silently dropped here
}
```

Two related details once you do store it:
- Keycloak **rotates** the refresh token on each use, so persist the new one from every refresh response.
- A refresh response may omit `id_token`. Overwrite it only when present — blanking it costs logout its `id_token_hint` for the IdP end-session call.

## Related

- [[Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer]]
- [[Single-flight token refresh prevents concurrent grants from invalidating each other]]
