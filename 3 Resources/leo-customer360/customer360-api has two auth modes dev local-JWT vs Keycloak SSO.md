---
ai_hash: 126ac80be492d8aa
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 docker-compose review 2026-08-19
status: seedling
tags:
- customer360
- auth
- keycloak
- sso
- jwt
- fastapi
title: 'customer360-api has two auth modes: dev local-JWT vs Keycloak SSO'
type: concept
---

# customer360-api has two auth modes: dev local-JWT vs Keycloak SSO

customer360-api (FastAPI) selects its auth backend from the SSO_LOGIN flag (core/config.py), which defaults to **False**:

- SSO_LOGIN=false (default, dev): POST /auth/login accepts ONE root/super-admin credential pair (DEFAULT_ROOT_USERNAME / DEFAULT_ROOT_PASSWORD, default admin / empty) that is NOT backed by a sys_user row, and returns a JWT signed LOCALLY with HS256 (DEV_JWT_SECRET). Same `Authorization: Bearer <token>` contract as prod, just self-signed. No Keycloak needed.
- SSO_LOGIN=true (prod): tokens are validated against Keycloak (OIDC) at SSO_LOGIN_URL; needs keycloak_realm / keycloak_client_id / keycloak_client_secret.

Practical consequence: a freshly deployed API returns 401 on protected routes simply because you have not logged in yet -- NOT because Keycloak is down. In the default dev mode you obtain a token via POST /auth/login with the root credential; Keycloak is only required once you flip SSO_LOGIN=true.

Stack context (docker-compose.yml, 6 services): postgres, redis, keycloak-db-init (creates the db_keycloak database), keycloak (SSO), api, cir-demo-seed (dev-only one-shot demo seeder). The backend-system/Dagster worker is NOT in this compose -- it is a separate stack. Related: [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]

## Related

- [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP tenant isolation fails closed and needs a Keycloak tenant_id claim mapper]]
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]
- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]
- [[leo-customer360 frontend SSO=false because CD deploys the API with SSO_LOGIN=false]]
- [[Deploying Keycloak 26 as a container health port 9000, bootstrap admin, start vs start-dev]]

%% ai-graph-end %%