---
ai_hash: 9adae48ab7537f3c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-22
entities:
- leo-customer360 frontend
- API
- SSO_LOGIN
- frontend-admin login panel
- frontend container
- deployments/frontend/overlays/uat.tfvars
- UI
- dev credential login
- Browser
- auth-view.js
- metadata.sso_login
- config.loadSystemMetadata()
- <apiRoot>/api/v1/metadata
- config.js
- API /metadata endpoint
- settings.sso_login
- metadata_repository.py
- API container SSO_LOGIN env
- config.py
- deployments/server/deploy-api.sh
- SSO_URL
- KC_REALM
- KC_CLIENT
- KC_SECRET
- deployments/sso/.env
- KEYCLOAK_CLIENT_SECRET
- GitHub Actions runner
- CD
- laptop deployment
- cd.yml
- deploy-all.sh
- DB_PASSWORD
- REDIS_PASSWORD
- GitHub secret
- repo-level secrets
- uat environment
- CI foot-gun
source: session 2026-08-22
status: seedling
tags:
- leo-customer360
- sso
- keycloak
- ci-cd
- github-actions
- gotcha
title: leo-customer360 frontend SSO=false because CD deploys the API with SSO_LOGIN=false
type: lesson
---

# leo-customer360 frontend SSO=false because CD deploys the API with SSO_LOGIN=false

The frontend-admin login panel is decided by the **API**, not the frontend container. Even with `sso_login = true` in `deployments/frontend/overlays/uat.tfvars` (frontend deployed `SSO_LOGIN=true`), the UI kept showing the dev credential login because the API published `sso_login: false`.

## The chain
1. Browser: `auth-view.js:139` picks the panel from `metadata.sso_login`, fetched via `config.loadSystemMetadata()` from `<apiRoot>/api/v1/metadata` (`config.js:412`) — the frontend`s own `SSO_LOGIN` env is a red herring.
2. API `/metadata` publishes `settings.sso_login` (`metadata_repository.py:145`), sourced from the API container`s `SSO_LOGIN` env (`config.py:196`).
3. `deployments/server/deploy-api.sh` (~L77-88) sets `SSO_LOGIN=true` only when `SSO_URL`, `KC_REALM`, `KC_CLIENT`, **and** `KC_SECRET` are all non-empty. `KC_SECRET` comes from `deployments/sso/.env` (`KEYCLOAK_CLIENT_SECRET`).
4. `deployments/sso/.env` is git-ignored (written locally by `bootstrap-realm.py`), so it is **absent on the GitHub Actions runner**.
5. In CD, `KC_SECRET` is empty -> config deemed incomplete -> API deploys `SSO_LOGIN=false`. Log tell: `SSO: api_sso_enabled=true but config/secret incomplete — deploying with SSO_LOGIN=false.`

## Net effect
SSO works when the API is deployed from a laptop (where `sso/.env` exists) but silently reverts to false on every CD run.

## Fix
Added a `cd.yml` step that recreates `deployments/sso/.env` from a `KEYCLOAK_CLIENT_SECRET` GitHub secret before `deploy-all.sh` (mirrors `DB_PASSWORD`/`REDIS_PASSWORD` injection). Set the GH secret repo-level (the `uat` environment has no env-scoped secrets; existing secrets are repo-level).

See also [[A git-ignored secret file that a deploy script silently degrades on is a CI foot-gun]].

## Related

- [[A git-ignored secret file that a deploy script silently degrades on is a CI foot-gun]]

%% ai-graph-start %%

**Related notes:**
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]
- [[Adding a step to always-on CD provision ALL its required env, and make it skip (not die) on missing secrets]]
- [[leo-customer360 deploy-sso.sh only restarts Keycloak; the realmrole bootstrap is the separate sso-realm step]]
- [[Creating a Keycloak realm role does not grant it — you must assign it and match the name the app authorizes on]]
- [[A git-ignored secret file that a deploy script silently degrades on is a CI foot-gun]]

**Relations:**
- leo-customer360 frontend — *has SSO status* — false
- CD — *deploys* — API
- API — *sets SSO_LOGIN to* — false
- frontend-admin login panel — *controlled by* — API
- frontend-admin login panel — *not controlled by* — frontend container
- sso_login = true — *configured in* — deployments/frontend/overlays/uat.tfvars
- frontend — *deployed with* — SSO_LOGIN=true
- UI — *displayed* — dev credential login
- API — *published* — sso_login: false
- Browser — *uses* — auth-view.js
- auth-view.js — *picks panel from* — metadata.sso_login
- metadata.sso_login — *fetched via* — config.loadSystemMetadata()
- config.loadSystemMetadata() — *calls* — <apiRoot>/api/v1/metadata
- <apiRoot>/api/v1/metadata — *defined in* — config.js
- API /metadata endpoint — *publishes* — settings.sso_login
- settings.sso_login — *from* — metadata_repository.py
- settings.sso_login — *sourced from* — API container SSO_LOGIN env
- API container SSO_LOGIN env — *from* — config.py
- deployments/server/deploy-api.sh — *sets SSO_LOGIN to* — true conditionally
- SSO_LOGIN=true — *requires* — SSO_URL
- SSO_LOGIN=true — *requires* — KC_REALM
- SSO_LOGIN=true — *requires* — KC_CLIENT
- SSO_LOGIN=true — *requires* — KC_SECRET
- KC_SECRET — *comes from* — deployments/sso/.env
- KC_SECRET — *is* — KEYCLOAK_CLIENT_SECRET
- deployments/sso/.env — *is* — git-ignored
- deployments/sso/.env — *absent on* — GitHub Actions runner
- KC_SECRET — *is empty in* — CD
- API — *deploys with* — SSO_LOGIN=false in CD
- SSO — *works with* — laptop deployment
- SSO — *fails on* — CD run
- Fix — *involves* — cd.yml
- cd.yml — *recreates* — deployments/sso/.env
- deployments/sso/.env — *from* — KEYCLOAK_CLIENT_SECRET GitHub secret
- cd.yml step — *runs before* — deploy-all.sh
- KEYCLOAK_CLIENT_SECRET GitHub secret — *mirrors* — DB_PASSWORD injection
- KEYCLOAK_CLIENT_SECRET GitHub secret — *mirrors* — REDIS_PASSWORD injection
- GitHub secret — *is* — repo-level
- uat environment — *lacks* — env-scoped secrets
- This issue — *is an example of* — CI foot-gun

%% ai-graph-end %%