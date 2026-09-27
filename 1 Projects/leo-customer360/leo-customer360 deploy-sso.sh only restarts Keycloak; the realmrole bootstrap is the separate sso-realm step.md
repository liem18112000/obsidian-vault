---
ai_hash: c752c49fb78b8a66
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-22
entities:
- leo-customer360
- deploy-sso.sh
- Keycloak container
- realm
- role
- client
- mapper
- test user
- deploy-api.sh
- sso-realm step
- deploy-all.sh
- bootstrap-realm.py
- KC_URL
- REALM (env var)
- CLIENT_ID (env var)
- TENANT_ID (env var)
- TEST_USER (env var)
- REDIRECT_URIS (env var)
- KEYCLOAK_ADMIN_PASSWORD (env var)
- KC_TEST_USER_PASSWORD (env var)
- sso/.env
- Creating a Keycloak realm role does not grant it
source: session 2026-08-22
status: seedling
tags:
- leo-customer360
- keycloak
- sso
- deployment
- gotcha
title: 'leo-customer360: deploy-sso.sh only restarts Keycloak; the realm/role bootstrap
  is the separate sso-realm step'
type: howto
---

# leo-customer360: deploy-sso.sh only restarts Keycloak; the realm/role bootstrap is the separate sso-realm step

In leo-customer360, `deployments/sso/deploy-sso.sh` only (re)runs the **Keycloak container** (docker run + readiness probe). It does NOT provision the realm, roles, client, mappers, or test user — its final line literally says to re-run `../server/deploy-api.sh` next.

The realm provisioning is a **separate step**: `sso-realm` in `deploy-all.sh` (runs `bootstrap-realm.py`, only on the `apply` action — `deploy-all.sh:146`), or run manually `cd deployments/sso && python3 bootstrap-realm.py`.

## Practical consequence
Editing `bootstrap-realm.py` (e.g. adding a role) and then running `./deploy-sso.sh uat` changes **nothing** — the script is never invoked. To apply realm changes: `./deploy-all.sh <env> apply --only sso-realm` (or `--from sso-realm`), or run the Python directly with the right env (`KC_URL`, `REALM`, `CLIENT_ID`, `TENANT_ID`, `TEST_USER`, `REDIRECT_URIS`, plus `KEYCLOAK_ADMIN_PASSWORD` + `KC_TEST_USER_PASSWORD` from `sso/.env`).

Related: [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]].

## Related

- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]

%% ai-graph-start %%

**Related notes:**
- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]
- [[leo-customer360 frontend SSO=false because CD deploys the API with SSO_LOGIN=false]]
- [[Adding a step to always-on CD provision ALL its required env, and make it skip (not die) on missing secrets]]
- [[Monitoring-dashboard SSO login user is c360admin, not the Keycloak master admin]]

**Relations:**
- deploy-sso.sh — *is part of* — leo-customer360
- deploy-sso.sh — *restarts* — Keycloak container
- deploy-sso.sh — *does not provision* — realm
- deploy-sso.sh — *does not provision* — role
- deploy-sso.sh — *does not provision* — client
- deploy-sso.sh — *does not provision* — mapper
- deploy-sso.sh — *does not provision* — test user
- deploy-sso.sh — *suggests running next* — deploy-api.sh
- sso-realm step — *is a step in* — deploy-all.sh
- sso-realm step — *runs* — bootstrap-realm.py
- bootstrap-realm.py — *provisions* — realm
- bootstrap-realm.py — *provisions* — role
- deploy-all.sh — *can apply changes for* — sso-realm step
- bootstrap-realm.py — *requires* — KC_URL
- bootstrap-realm.py — *requires* — REALM (env var)
- bootstrap-realm.py — *requires* — CLIENT_ID (env var)
- bootstrap-realm.py — *requires* — TENANT_ID (env var)
- bootstrap-realm.py — *requires* — TEST_USER (env var)
- bootstrap-realm.py — *requires* — REDIRECT_URIS (env var)
- bootstrap-realm.py — *requires* — KEYCLOAK_ADMIN_PASSWORD (env var)
- bootstrap-realm.py — *requires* — KC_TEST_USER_PASSWORD (env var)
- KEYCLOAK_ADMIN_PASSWORD (env var) — *is sourced from* — sso/.env
- KC_TEST_USER_PASSWORD (env var) — *is sourced from* — sso/.env
- Editing bootstrap-realm.py — *has no effect on* — deploy-sso.sh
- This note — *is related to* — Creating a Keycloak realm role does not grant it

%% ai-graph-end %%