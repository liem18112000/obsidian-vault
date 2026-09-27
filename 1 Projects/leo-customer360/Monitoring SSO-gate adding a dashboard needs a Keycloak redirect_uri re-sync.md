---
ai_hash: 5cd254d9be6f60cb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- SSO-gate
- dashboard
- Keycloak
- redirect_uri
- re-sync
- leo-customer360
- no-auth web UI
- Netdata
- Jaeger
- SSO
- L4 NLB
- OIDC
- oauth2-proxy
- c360-oauth2-proxy
- customer360 realm
- UI
- loopback
- backend port
- Portainer
- login
- reverse-proxy CSRF check
- LB backend
- public port
- box:proxy_port
- health `/ping`
- '`_sso=true`'
- '`oauth2_enabled=true`'
- '`_GATED`'
- callback URL
- redirect URIs
- gated UI
- Netdata vars
- J_SSO
- J_GATED
- J_PROXY
- J_REDIRECT
- '`run_proxy` helper'
- '`jaeger` LB backend'
- proxy port 4686
- Jaeger's 16686 UI
- '`deploy-monitoring.sh`'
- Keycloak client bootstrap
- '`OAUTH2_PROXY_CLIENT_SECRET`'
- '`.env`'
- KC admin password
- oauth2 login
- '"Invalid redirect_uri" error'
- '`bootstrap-oauth2-client.py`'
- PUT (HTTP method)
- '`redirectUris` (Keycloak field)'
- deploy wrapper
- skip (process)
- '*Valid redirect URIs* (Keycloak UI field)'
- Docker containers
- VNG vServer VMs
- SSH
- tracing
- OTel
- UAT
- PROD
source: session 2026-08-21
status: seedling
tags:
- leo-customer360
- oauth2-proxy
- keycloak
- sso
- jaeger
- gotcha
title: 'Monitoring SSO-gate: adding a dashboard needs a Keycloak redirect_uri re-sync'
type: lesson
---

# Monitoring SSO-gate: adding a dashboard needs a Keycloak redirect_uri re-sync

**leo-customer360** pattern + gotcha for exposing a no-auth web UI (Netdata, now Jaeger) publicly behind SSO.

## The pattern (deployments/monitoring)
The L4 NLB can't do OIDC, so each no-native-auth dashboard is fronted by its OWN **oauth2-proxy** container (a Keycloak confidential client `c360-oauth2-proxy` in the `customer360` realm):
- UI binds **loopback** (127.0.0.1:PORT); only its oauth2-proxy reaches it.
- oauth2-proxy listens on a dedicated backend port (Portainer skips this — it has its own login + a reverse-proxy CSRF check, so it's exposed DIRECT).
- LB backend maps **public port -> box:proxy_port** (health `/ping`).
- Per-dashboard toggle: `<x>_sso=true` + `oauth2_enabled=true` => `<X>_GATED`; each gated dashboard adds its `http://<oauth2_public_host>:<public_port>/oauth2/callback` to the client's redirect URIs.
- To add another gated UI (e.g. Jaeger): mirror the Netdata vars (J_SSO/J_GATED/J_PROXY/J_REDIRECT), call the shared `run_proxy` helper, add a `jaeger` LB backend. Chose proxy port 4686 for Jaeger's 16686 UI.

## The gotcha (bit me)
`deploy-monitoring.sh` **skips the Keycloak client bootstrap entirely when `OAUTH2_PROXY_CLIENT_SECRET` is already in `.env`** (to avoid needing the KC admin password every run). So on an env that already has the client, adding a NEW gated dashboard does NOT register its callback URL -> oauth2 login fails with **"Invalid redirect_uri"**. (The `bootstrap-oauth2-client.py` script itself DOES upsert redirectUris via PUT — it's the deploy wrapper's skip that's the trap.)
Fix: add the URI under the client's *Valid redirect URIs* in Keycloak, OR comment out `OAUTH2_PROXY_CLIENT_SECRET` in `.env` and re-run `./deploy-monitoring.sh <env>` (re-bootstraps, upserts all redirects, rewrites the same secret).

## Related
[[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
[[leo-customer360 tracing OTel off-by-default on UAT, on at 10% on PROD|leo-customer360 tracing: OTel off-by-default on UAT, on at 10% on PROD]]

## Related

- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[leo-customer360 tracing OTel off-by-default on UAT, on at 10% on PROD]]

%% ai-graph-start %%

**Related notes:**
- [[Shared OIDC client skip-if-secret-exists guard drops new redirect URIs (Invalid redirect_uri)]]
- [[Monitoring-dashboard SSO login user is c360admin, not the Keycloak master admin]]
- [[Serve a no-auth UI over TLS+SSO behind Caddy at a subpath (oauth2-proxy proxy-prefix)]]
- [[Exposure model for ops dashboards behind an L4 (OIDC-incapable) load balancer]]
- [[Gating a dashboard behind Keycloak when the LB is L4 - use oauth2-proxy]]

**Relations:**
- SSO-gate — *needs* — redirect_uri re-sync
- redirect_uri re-sync — *involves* — Keycloak
- adding a dashboard — *triggers need for* — redirect_uri re-sync
- leo-customer360 — *is a pattern for* — exposing no-auth web UI
- no-auth web UI — *is behind* — SSO
- Netdata — *is an example of* — no-auth web UI
- Jaeger — *is an example of* — no-auth web UI
- L4 NLB — *cannot do* — OIDC
- no-native-auth dashboard — *is fronted by* — oauth2-proxy
- oauth2-proxy — *is a* — Keycloak confidential client
- c360-oauth2-proxy — *is an instance of* — oauth2-proxy
- c360-oauth2-proxy — *is in* — customer360 realm
- UI — *binds to* — loopback
- oauth2-proxy — *reaches* — UI
- oauth2-proxy — *listens on* — backend port
- Portainer — *skips* — oauth2-proxy
- Portainer — *has* — login
- Portainer — *has* — reverse-proxy CSRF check
- LB backend — *maps* — public port
- public port — *to* — box:proxy_port
- box:proxy_port — *provides* — health `/ping`
- `_sso=true` — *enables* — `_GATED`
- `oauth2_enabled=true` — *enables* — `_GATED`
- gated dashboard — *adds* — callback URL
- callback URL — *to* — redirect URIs
- Jaeger — *is an example of* — gated UI
- gated UI — *mirrors* — Netdata vars
- gated UI — *calls* — `run_proxy` helper
- gated UI — *adds* — `jaeger` LB backend
- proxy port 4686 — *is for* — Jaeger's 16686 UI
- `deploy-monitoring.sh` — *skips* — Keycloak client bootstrap
- Keycloak client bootstrap — *is skipped when* — `OAUTH2_PROXY_CLIENT_SECRET`
- `OAUTH2_PROXY_CLIENT_SECRET` — *is in* — `.env`
- Keycloak client bootstrap — *requires* — KC admin password
- adding a gated dashboard — *without registering callback URL causes* — oauth2 login
- oauth2 login — *fails with* — "Invalid redirect_uri" error
- `bootstrap-oauth2-client.py` — *upserts* — `redirectUris` (Keycloak field)
- `redirectUris` (Keycloak field) — *via* — PUT (HTTP method)
- deploy wrapper — *causes* — skip (process)
- adding URI to *Valid redirect URIs* in Keycloak — *fixes* — "Invalid redirect_uri" error
- commenting out `OAUTH2_PROXY_CLIENT_SECRET` in `.env` — *and re-running* — `deploy-monitoring.sh`
- `deploy-monitoring.sh` — *fixes* — "Invalid redirect_uri" error
- `deploy-monitoring.sh` — *re-bootstraps* — Keycloak client bootstrap
- `deploy-monitoring.sh` — *upserts* — redirect URIs
- `deploy-monitoring.sh` — *rewrites* — `OAUTH2_PROXY_CLIENT_SECRET`
- leo-customer360 — *deploys as* — Docker containers
- Docker containers — *run on* — VNG vServer VMs
- VNG vServer VMs — *accessed via* — SSH
- leo-customer360 — *uses* — tracing
- tracing — *uses* — OTel
- OTel — *is off on* — UAT
- OTel — *is on at 10% on* — PROD

%% ai-graph-end %%