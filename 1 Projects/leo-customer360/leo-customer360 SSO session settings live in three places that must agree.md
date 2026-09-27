---
title: "leo-customer360 SSO session settings live in three places that must agree"
created: 2026-09-27
type: lesson
status: seedling
source: "fix/sso/too-short-session branch, 2026-09-27"
tags: [leo-customer360, sso, keycloak, session, config]
---

# leo-customer360 SSO session settings live in three places that must agree

How long an admin stays signed in to the customer360 admin UI is decided by three independent layers, and the shortest one wins:

1. **The Keycloak realm** — `deployments/sso/bootstrap-realm.py` sets `accessTokenLifespan`, `ssoSessionIdleTimeout` and `ssoSessionMaxLifespan` (plus the matching client attributes), now from `KEYCLOAK_TOKEN_EXPIRES_MINUTES` / `KEYCLOAK_SSO_SESSION_IDLE_MINUTES` / `KEYCLOAK_SSO_SESSION_MAX_MINUTES`. Defaults: 60 min token, 8 h idle, 10 h max. The script refuses a config where the session bounds don't exceed one token lifetime.
2. **The API** — `core/auth.py` rejects any token older than `settings.keycloak_token_expires_minutes` (same env var name; `_token_lifetime_seconds()` is the single source for both the `iat` bound and the Redis cache TTL). Setting it below the realm's value silently expires tokens Keycloak still accepts.
3. **The browser** — `customer360-frontend/static/js/common/config.js` stores the refresh token and renews against `POST /auth/refresh` a minute before expiry, retrying a 401 once before giving up.

Because the realm change only takes effect on a bootstrap re-run, changing the env var alone does nothing: re-run `bootstrap-realm.py`, and remember tokens already issued keep their original lifetime until they expire.

The regression worth remembering: commit `7471b28` set all three realm lifetimes to 1800 s, which pinned the SSO session to 30 minutes with no way to refresh — see [[Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer]].

## Related

- [[Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer]]
- [[Token age checks need clock-skew leeway and must match the IdP lifespan]]
- [[leo-customer360 deploy-sso.sh only restarts Keycloak; the realmrole bootstrap is the separate sso-realm step]]
