---
ai_hash: fc0228ca290e6549
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-23
entities: []
source: leo-customer360 deployments/sso + cd.yml, session 2026-08-23
status: seedling
tags:
- keycloak
- rbac
- cd
- idempotency
- leo-customer360
- gotcha
title: Keep Keycloak realm roles in sync with app authz constants, and ensure the
  bootstrap step is in the CD services list
type: lesson
---

# Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list

Two related traps found wiring Keycloak SSO for customer360-api:

**1. The IdP's roles must match the app's authorization constants.** The API authorizes on hardcoded role sets — `PLATFORM_ADMIN_ROLES = {platform_admin, super_admin, system_admin}` and `TENANT_ADMIN_ROLES = that | {tenant_admin, admin}` (customer360-api/core/routers/segment_api.py) — read from the token's `realm_access.roles`. But the realm bootstrap (bootstrap-realm.py) only created a `root` realm role the API never checks, and assigned NO role to the admin test user. Net effect: no user could ever pass the admin checks. Fix: the realm-provisioning script must create exactly the roles the app checks for, and grant one to a bootstrap admin user. Keycloak includes assigned realm roles in `realm_access.roles` by default (via the built-in 'roles' client scope) — no extra mapper needed.

**2. A provisioning step can silently never run because it is not in the CD 'services' list.** deploy-all.sh has an `sso-realm` step, but CD deployed with `--only api,backend,ads,frontend` (DEFAULT_SVC), which EXCLUDES sso-realm — so the realm bootstrap never ran in CD, only when someone remembered to run it manually. The step also needed `KEYCLOAK_ADMIN_PASSWORD`, which the CD runner didn't have. Lesson: an idempotent bootstrap script is worthless if the pipeline's step-selection filter drops it; audit what the CD 'services'/'only' list actually includes, and confirm every step it runs has the secrets it needs.

**Idempotency techniques used:** GET-before-POST per role; re-POSTing a realm role-mapping is a Keycloak no-op; realm/client/user are create-or-update. So the whole bootstrap is safe to run on every deploy.

Source: leo-customer360 deployments/sso/bootstrap-realm.py + .github/workflows/cd.yml (2026-08).

## Related

- [[Exposure model for ops dashboards behind an L4 (OIDC-incapable) load balancer]]

%% ai-graph-start %%

**Related notes:**
- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]
- [[leo-customer360 deploy-sso.sh only restarts Keycloak; the realmrole bootstrap is the separate sso-realm step]]
- [[Adding a step to always-on CD provision ALL its required env, and make it skip (not die) on missing secrets]]
- [[leo-customer360 frontend SSO=false because CD deploys the API with SSO_LOGIN=false]]
- [[LEO CDP tenant isolation fails closed and needs a Keycloak tenant_id claim mapper]]

%% ai-graph-end %%