---
ai_hash: c2ad604bfeb2c178
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: leo-customer360 SSO session fix, 2026-09-27
status: seedling
tags:
- keycloak
- oidc
- sso
- session
- gotcha
title: Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer
type: lesson
---

# Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer

`ssoSessionMaxLifespan` is a hard wall measured from login: once it elapses, Keycloak kills the SSO session outright and no refresh token can revive it. It is unrelated to `ssoSessionIdleTimeout`, which is a sliding window reset by activity.

The failure mode this creates: if you set all three lifetimes to the same value — say `accessTokenLifespan = ssoSessionIdleTimeout = ssoSessionMaxLifespan = 1800` — the SSO session expires at exactly the moment the *first* access token does. Refreshing becomes structurally impossible, because by the time the token needs renewing the session that would authorize the renewal is already gone. Users get bounced to the login page on a fixed timer no matter what they are doing.

**Rule:** the session bounds must be strictly greater than one access-token lifetime. The token is the short-lived thing; the session is the long-lived thing that keeps minting them.

Keycloak's own defaults encode this correctly:

| Setting | Default |
| --- | --- |
| `accessTokenLifespan` | 5 min |
| `ssoSessionIdleTimeout` | 30 min |
| `ssoSessionMaxLifespan` | 10 hours |

A "sensible admin console" shape is something like 60 min token / 8 h idle / 10 h max. Worth enforcing the invariant in whatever provisions the realm, so a well-meaning security tightening can't collapse the three values onto each other again.

## Related

- [[Discarding the refresh token caps an SPA session at one access-token lifetime]]
- [[LEO CDP tenant isolation fails closed and needs a Keycloak tenant_id claim mapper]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 SSO session settings live in three places that must agree]]
- [[Discarding the refresh token caps an SPA session at one access-token lifetime]]
- [[Token age checks need clock-skew leeway and must match the IdP lifespan]]
- [[Single-flight token refresh prevents concurrent grants from invalidating each other]]
- [[LEO CDP tenant isolation fails closed and needs a Keycloak tenant_id claim mapper]]

%% ai-graph-end %%