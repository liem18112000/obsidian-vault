---
ai_hash: 861e32e25059451b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- leo-customer360 SSO session settings
- admin
- customer360 admin UI
- SSO session
- Keycloak realm
- deployments/sso/bootstrap-realm.py
- accessTokenLifespan
- ssoSessionIdleTimeout
- ssoSessionMaxLifespan
- KEYCLOAK_TOKEN_EXPIRES_MINUTES
- KEYCLOAK_SSO_SESSION_IDLE_MINUTES
- KEYCLOAK_SSO_SESSION_MAX_MINUTES
- API
- core/auth.py
- settings.keycloak_token_expires_minutes
- _token_lifetime_seconds()
- Redis cache TTL
- browser
- customer360-frontend/static/js/common/config.js
- refresh token
- POST /auth/refresh
- bootstrap re-run
- commit 7471b28
- Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer
- Token age checks need clock-skew leeway and must match the IdP lifespan
- leo-customer360 deploy-sso.sh only restarts Keycloak; the realmrole bootstrap is
  the separate sso-realm step
- Keycloak
- token
- iat bound
- IdP
- client attributes
source: fix/sso/too-short-session branch, 2026-09-27
status: seedling
tags:
- leo-customer360
- sso
- keycloak
- session
- config
title: leo-customer360 SSO session settings live in three places that must agree
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer]]
- [[Token age checks need clock-skew leeway and must match the IdP lifespan]]
- [[Discarding the refresh token caps an SPA session at one access-token lifetime]]
- [[leo-customer360 frontend SSO=false because CD deploys the API with SSO_LOGIN=false]]
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]

**Relations:**
- leo-customer360 SSO session settings — *configured in* — Keycloak realm
- leo-customer360 SSO session settings — *configured in* — API
- leo-customer360 SSO session settings — *configured in* — browser
- admin — *uses* — customer360 admin UI
- admin — *has* — SSO session
- SSO session — *duration influenced by* — Keycloak realm
- SSO session — *duration influenced by* — API
- SSO session — *duration influenced by* — browser
- deployments/sso/bootstrap-realm.py — *sets* — accessTokenLifespan
- deployments/sso/bootstrap-realm.py — *sets* — ssoSessionIdleTimeout
- deployments/sso/bootstrap-realm.py — *sets* — ssoSessionMaxLifespan
- deployments/sso/bootstrap-realm.py — *sets* — client attributes
- accessTokenLifespan — *controlled by* — KEYCLOAK_TOKEN_EXPIRES_MINUTES
- ssoSessionIdleTimeout — *controlled by* — KEYCLOAK_SSO_SESSION_IDLE_MINUTES
- ssoSessionMaxLifespan — *controlled by* — KEYCLOAK_SSO_SESSION_MAX_MINUTES
- deployments/sso/bootstrap-realm.py — *validates* — session bounds
- core/auth.py — *rejects* — token older than settings.keycloak_token_expires_minutes
- settings.keycloak_token_expires_minutes — *is an alias for* — KEYCLOAK_TOKEN_EXPIRES_MINUTES
- _token_lifetime_seconds() — *is source for* — iat bound
- _token_lifetime_seconds() — *is source for* — Redis cache TTL
- customer360-frontend/static/js/common/config.js — *manages* — refresh token
- customer360-frontend/static/js/common/config.js — *sends request to* — POST /auth/refresh
- Keycloak realm — *configuration change requires* — bootstrap re-run
- commit 7471b28 — *set* — realm lifetimes to 1800 s
- commit 7471b28 — *pinned* — SSO session to 30 minutes
- Keycloak ssoSessionMaxLifespan is an absolute session cap, not an idle timer — *explains* — ssoSessionMaxLifespan
- Token age checks need clock-skew leeway and must match the IdP lifespan — *relates to* — token
- leo-customer360 deploy-sso.sh only restarts Keycloak; the realmrole bootstrap is the separate sso-realm step — *relates to* — Keycloak
- Keycloak — *is an* — IdP
- Keycloak — *manages* — Keycloak realm
- Keycloak — *issues* — token
- token — *has* — lifetime
- SSO session — *has* — idle timeout
- SSO session — *has* — max lifespan
- Keycloak realm — *has* — accessTokenLifespan
- Keycloak realm — *has* — ssoSessionIdleTimeout
- Keycloak realm — *has* — ssoSessionMaxLifespan
- KEYCLOAK_TOKEN_EXPIRES_MINUTES — *is an* — env var
- KEYCLOAK_SSO_SESSION_IDLE_MINUTES — *is an* — env var
- KEYCLOAK_SSO_SESSION_MAX_MINUTES — *is an* — env var

%% ai-graph-end %%