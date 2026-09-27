---
ai_hash: 184db1fc300777a8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- Monitoring-dashboard SSO login
- c360admin
- Keycloak master admin
- leo-customer360 dashboards
- Netdata
- Jaeger
- oauth2-proxy
- Keycloak
- customer360 realm
- deployments/sso/bootstrap-realm.py
- TEST_USER
- KC_TEST_USER_PASSWORD
- deployments/sso/.env
- admin account
- KEYCLOAK_ADMIN_PASSWORD
- master-realm console admin
- oauth2 callback
- beta.leocdp.com
- HSTS
- 'Monitoring SSO-gate: adding a dashboard needs a Keycloak redirect_uri re-sync'
source: session 2026-08-21
status: seedling
tags:
- leo-customer360
- keycloak
- sso
- login
- oauth2-proxy
title: Monitoring-dashboard SSO login user is c360admin, not the Keycloak master admin
type: reference
---

# Monitoring-dashboard SSO login user is c360admin, not the Keycloak master admin

To log into the SSO-gated leo-customer360 dashboards (Netdata, Jaeger — via oauth2-proxy -> Keycloak), use the **`customer360` realm** user, NOT the Keycloak console admin.

- **Username: `c360admin`** — the ONLY user in the `customer360` realm (created by `deployments/sso/bootstrap-realm.py` as `TEST_USER`, default `c360admin`, email c360admin@example.com, enabled + emailVerified).
- **Password:** the value of **`KC_TEST_USER_PASSWORD`** in `deployments/sso/.env` (bootstrap sets it as a non-temporary password via the KC admin API reset-password call).
- The **`admin` / `KEYCLOAK_ADMIN_PASSWORD`** account is the Keycloak **master-realm console admin** — it is NOT a user in the `customer360` realm, so oauth2-proxy rejects it at the credential step (that's the usual 'cannot login' cause).

Realm login settings: loginWithEmailAllowed=true, registrationAllowed=false, resetPasswordAllowed=false, verifyEmail=false.

Separate gotcha to watch AFTER creds: the oauth2 callback is `http://<oauth2_public_host>:<port>/oauth2/callback` on `beta.leocdp.com`, which is HSTS-preloaded -> browser upgrades to https on a plain-HTTP port and the callback can fail. Affects Netdata and Jaeger identically.

## Related
[[Monitoring SSO-gate: adding a dashboard needs a Keycloak redirect_uri re-sync]]

## Related

- [[Monitoring SSO-gate: adding a dashboard needs a Keycloak redirect_uri re-sync]]

%% ai-graph-start %%

**Related notes:**
- [[Monitoring SSO-gate adding a dashboard needs a Keycloak redirect_uri re-sync]]
- [[Shared OIDC client skip-if-secret-exists guard drops new redirect URIs (Invalid redirect_uri)]]
- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]
- [[leo-customer360 deploy-sso.sh only restarts Keycloak; the realmrole bootstrap is the separate sso-realm step]]
- [[leo-customer360 frontend SSO=false because CD deploys the API with SSO_LOGIN=false]]

**Relations:**
- Monitoring-dashboard SSO login — *uses user* — c360admin
- Monitoring-dashboard SSO login — *does not use user* — Keycloak master admin
- leo-customer360 dashboards — *include* — Netdata
- leo-customer360 dashboards — *include* — Jaeger
- leo-customer360 dashboards — *are accessed via* — oauth2-proxy
- oauth2-proxy — *authenticates with* — Keycloak
- Monitoring-dashboard SSO login — *uses realm* — customer360 realm
- c360admin — *is user in* — customer360 realm
- c360admin — *is created by* — deployments/sso/bootstrap-realm.py
- TEST_USER — *is alias for* — c360admin
- c360admin — *password is set by* — KC_TEST_USER_PASSWORD
- KC_TEST_USER_PASSWORD — *is defined in* — deployments/sso/.env
- admin account — *is* — master-realm console admin
- admin account — *uses password* — KEYCLOAK_ADMIN_PASSWORD
- admin account — *is not in realm* — customer360 realm
- oauth2-proxy — *rejects* — admin account
- oauth2 callback — *is on host* — beta.leocdp.com
- beta.leocdp.com — *is* — HSTS-preloaded
- oauth2 callback — *can fail due to* — HSTS
- Netdata — *is affected by* — oauth2 callback failure
- Jaeger — *is affected by* — oauth2 callback failure
- Monitoring-dashboard SSO login user is c360admin, not the Keycloak master admin — *is related to* — Monitoring SSO-gate: adding a dashboard needs a Keycloak redirect_uri re-sync

%% ai-graph-end %%