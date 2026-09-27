---
ai_hash: bb62a04d7223bc37
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- Monitoring SSO-gate
- dashboard
- Keycloak
- redirect_uri re-sync
- leo-customer360
- no-auth web UI
- Netdata
- Jaeger
- SSO
- L4 NLB
- OIDC
- oauth2-proxy container
- c360-oauth2-proxy
- customer360 realm
- UI
- loopback
- Portainer
- LB backend
- public port
- proxy_port
- gated dashboard
- callback URL
- client's redirect URIs
- run_proxy helper
- jaeger LB backend
- deploy-monitoring.sh
- Keycloak client bootstrap
- OAUTH2_PROXY_CLIENT_SECRET
- .env
- oauth2 login
- Invalid redirect_uri
- bootstrap-oauth2-client.py
- Valid redirect URIs
- Docker containers
- VNG vServer VMs
- SSH
- OTel
- UAT
- PROD
- leo-customer360 tracing
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
- Monitoring SSO-gate — *adding a dashboard needs* — Keycloak redirect_uri re-sync
- leo-customer360 — *exposes* — no-auth web UI
- no-auth web UI — *is* — Netdata
- no-auth web UI — *is* — Jaeger
- no-auth web UI — *behind* — SSO
- L4 NLB — *cannot do* — OIDC
- no-auth web UI — *fronted by* — oauth2-proxy container
- oauth2-proxy container — *is a* — c360-oauth2-proxy
- c360-oauth2-proxy — *in* — customer360 realm
- UI — *binds* — loopback
- oauth2-proxy container — *reaches* — UI
- oauth2-proxy container — *listens on* — dedicated backend port
- Portainer — *has* — own login
- Portainer — *has* — reverse-proxy CSRF check
- Portainer — *exposed* — DIRECT
- LB backend — *maps* — public port
- public port — *to* — proxy_port
- gated dashboard — *adds* — callback URL
- callback URL — *to* — client's redirect URIs
- Jaeger — *uses* — proxy port 4686
- Jaeger — *has* — UI port 16686
- deploy-monitoring.sh — *skips* — Keycloak client bootstrap
- Keycloak client bootstrap — *skipped when* — OAUTH2_PROXY_CLIENT_SECRET in .env
- adding a NEW gated dashboard — *does not register* — callback URL
- oauth2 login — *fails with* — Invalid redirect_uri
- bootstrap-oauth2-client.py — *upserts* — redirect_uri
- Keycloak — *has* — Valid redirect URIs
- deploy-monitoring.sh — *re-bootstraps* — redirects
- deploy-monitoring.sh — *rewrites* — secret
- leo-customer360 — *deploys as* — Docker containers
- Docker containers — *on* — VNG vServer VMs
- VNG vServer VMs — *over* — SSH
- leo-customer360 tracing — *uses* — OTel
- OTel — *is off-by-default on* — UAT
- OTel — *is on at 10% on* — PROD

%% ai-graph-end %%