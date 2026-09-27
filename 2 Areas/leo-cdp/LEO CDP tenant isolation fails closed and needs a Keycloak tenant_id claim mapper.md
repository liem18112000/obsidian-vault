---
ai_hash: b6e8a493175fc0f9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities:
- LEO CDP
- Keycloak
- tenant_id claim mapper
- LEO Customer 360 API
- auth middleware
- Postgres
- app.tenant_id (session var)
- app.user_id (session var)
- Row-Level Security (RLS)
- tenant_policy
- customer360 schema
- migrations/001_harden_tenant_rls_policies.sql
- SSO_LOGIN
- Keycloak access token
- tenant_id custom claim
- protocol mapper
- leocdp client
- sys_user
- sys_userinfo
- HS256 dev JWT
- DEV_JWT_SECRET
- OIDC token introspection
- Redis
- /health route
- /api/v1/metadata route
- /api/v1/auth/* routes
- schema migrations
- dbmate
- alembic
- bearer token
- 401 'Tenant context could not be resolved'
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

- [[LEO CDP schema migrations are ordered plain SQL]]
- [[not dbmate or alembic]]

%% ai-graph-start %%

**Related notes:**
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]
- [[customer360-api has two auth modes dev local-JWT vs Keycloak SSO]]
- [[Keycloak 24+ token introspection requires the client in the token audience]]
- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]
- [[Monitoring-dashboard SSO login user is c360admin, not the Keycloak master admin]]

**Relations:**
- LEO CDP — *has property* — tenant isolation fails closed
- LEO CDP — *needs* — Keycloak tenant_id claim mapper
- LEO Customer 360 API — *enforces* — fail-closed multi-tenant isolation
- auth middleware — *resolves* — app.tenant_id (session var)
- auth middleware — *resolves* — app.user_id (session var)
- auth middleware — *pushes to* — Postgres
- app.tenant_id (session var) — *drives* — Row-Level Security (RLS)
- app.user_id (session var) — *drives* — Row-Level Security (RLS)
- Row-Level Security (RLS) — *applies to* — customer360 schema
- tenant_policy — *hardened by* — migrations/001_harden_tenant_rls_policies.sql
- app.tenant_id (session var) — *failure to resolve leads to* — 401 'Tenant context could not be resolved'
- Keycloak access token — *must carry* — tenant_id custom claim
- tenant_id custom claim — *configured via* — protocol mapper
- protocol mapper — *on* — leocdp client
- tenant_id claim mapper — *absence leads to* — empty data
- tenant_id claim mapper — *absence leads to* — provisioning refusal
- provisioning refusal — *affects* — sys_user
- provisioning refusal — *affects* — sys_userinfo
- SSO_LOGIN — *determines* — Auth modes
- SSO_LOGIN — *false uses* — HS256 dev JWT
- HS256 dev JWT — *uses* — DEV_JWT_SECRET
- SSO_LOGIN — *true uses* — Keycloak OIDC token introspection
- OIDC token introspection — *cached in* — Redis
- bearer token — *required for* — most routes
- bearer token — *not required for* — /health route
- bearer token — *not required for* — /api/v1/metadata route
- bearer token — *not required for* — /api/v1/auth/* routes
- LEO CDP — *uses* — schema migrations
- schema migrations — *are* — ordered plain SQL
- schema migrations — *are not* — dbmate
- schema migrations — *are not* — alembic

%% ai-graph-end %%