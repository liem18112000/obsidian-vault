---
ai_hash: 029300cafe7c743d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities:
- LEO CDP
- Keycloak
- LEO Customer 360's API
- Multi-tenant isolation
- Bearer token
- Postgres session vars
- app.tenant_id
- app.user_id
- Row-Level Security (RLS)
- tenant_policy
- customer360 schema
- migrations/001_harden_tenant_rls_policies.sql
- SSO_LOGIN
- tenant_id custom claim
- Protocol mapper
- leocdp client
- sys_user
- sys_userinfo
- HS256 dev JWT
- DEV_JWT_SECRET
- OIDC token introspection
- Redis
- /health endpoint
- /api/v1/metadata endpoint
- /api/v1/auth/* endpoints
- LEO CDP schema migrations
- dbmate
- alembic
- Keycloak tenant_id claim mapper
source: leo-customer360 release-doc work, session 2026-08-26
status: seedling
tags:
- leo-cdp
- auth
- keycloak
- rls
- multitenancy
- gotcha
title: LEO CDP tenant isolation fails closed and needs a Keycloak tenant_id claim
  mapper
type: gotcha
---

# LEO CDP tenant isolation fails closed and needs a Keycloak tenant_id claim mapper

LEO Customer 360's API enforces **fail-closed multi-tenant isolation**: the auth middleware resolves `(tenant_id, user_id)` from the bearer token and pushes them into Postgres session vars `app.tenant_id` / `app.user_id`, which drive Row-Level Security `tenant_policy` policies (FORCE RLS on the `customer360` schema; hardened by `migrations/001_harden_tenant_rls_policies.sql` so a blank `app.tenant_id` matches nothing instead of everything). If no `tenant_id` can be resolved, the request is rejected **401 'Tenant context could not be resolved'**.

Operational gotcha: under `SSO_LOGIN=true`, the Keycloak access token **must carry a `tenant_id` custom claim** (via a protocol mapper on the `leocdp` client). Without it, users authenticate successfully but see **empty data** (and first-login provisioning of `sys_user`/`sys_userinfo` is refused). 'Logged in but no data' almost always means the missing tenant_id mapper.

Auth has two modes via `SSO_LOGIN`: false = local HS256 dev JWT (`DEV_JWT_SECRET`); true = Keycloak OIDC token introspection cached in Redis until token exp. Bearer required on all routes except `/health`, `/api/v1/metadata`, `/api/v1/auth/*`.

## Related

- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]

%% ai-graph-start %%

**Related notes:**
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]
- [[customer360-api has two auth modes dev local-JWT vs Keycloak SSO]]
- [[Keycloak 24+ token introspection requires the client in the token audience]]
- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]
- [[Monitoring-dashboard SSO login user is c360admin, not the Keycloak master admin]]

**Relations:**
- LEO CDP tenant isolation — *fails closed* — 
- LEO CDP tenant isolation — *needs* — Keycloak tenant_id claim mapper
- LEO Customer 360's API — *enforces* — Multi-tenant isolation
- Multi-tenant isolation — *is* — fail-closed
- LEO Customer 360's API — *resolves* — app.tenant_id
- LEO Customer 360's API — *resolves* — app.user_id
- LEO Customer 360's API — *pushes* — app.tenant_id
- LEO Customer 360's API — *pushes* — app.user_id
- app.tenant_id — *from* — Bearer token
- app.user_id — *from* — Bearer token
- app.tenant_id — *into* — Postgres session vars
- app.user_id — *into* — Postgres session vars
- Postgres session vars — *drive* — Row-Level Security (RLS)
- Row-Level Security (RLS) — *uses* — tenant_policy
- tenant_policy — *on* — customer360 schema
- customer360 schema — *hardened by* — migrations/001_harden_tenant_rls_policies.sql
- migrations/001_harden_tenant_rls_policies.sql — *prevents* — blank app.tenant_id matching everything
- Keycloak access token — *must carry* — tenant_id custom claim
- tenant_id custom claim — *provided via* — Protocol mapper
- Protocol mapper — *on* — leocdp client
- SSO_LOGIN=true — *requires* — Keycloak access token
- SSO_LOGIN=false — *uses* — HS256 dev JWT
- HS256 dev JWT — *uses* — DEV_JWT_SECRET
- SSO_LOGIN=true — *uses* — OIDC token introspection
- OIDC token introspection — *cached in* — Redis
- Bearer token — *required on* — all routes
- Bearer token — *not required on* — /health endpoint
- Bearer token — *not required on* — /api/v1/metadata endpoint
- Bearer token — *not required on* — /api/v1/auth/* endpoints
- LEO CDP — *related to* — LEO CDP schema migrations
- LEO CDP schema migrations — *are* — ordered plain SQL
- LEO CDP schema migrations — *not* — dbmate
- LEO CDP schema migrations — *not* — alembic
- Keycloak — *provides* — Keycloak access token
- Keycloak — *provides* — OIDC token introspection
- Keycloak — *has* — leocdp client
- Keycloak tenant_id claim mapper — *is a type of* — Protocol mapper
- Keycloak tenant_id claim mapper — *provides* — tenant_id custom claim
- Keycloak — *is a* — SSO
- LEO CDP — *uses* — Keycloak
- LEO CDP — *uses* — Postgres
- LEO CDP — *uses* — Redis
- LEO CDP — *uses* — Row-Level Security (RLS)
- LEO CDP — *uses* — LEO Customer 360's API
- LEO CDP — *has* — tenant isolation
- LEO CDP — *has* — schema migrations
- LEO Customer 360's API — *is part of* — LEO CDP
- Keycloak tenant_id claim mapper — *is a* — claim mapper

%% ai-graph-end %%