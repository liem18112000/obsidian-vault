---
ai_hash: d49e69406587d8bf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-22
entities: []
source: leo-customer360 deployments/monitoring, session 2026-08-22
status: seedling
tags:
- keycloak
- oauth2-proxy
- oidc
- gotcha
- leo-customer360
title: Shared OIDC client skip-if-secret-exists guard drops new redirect URIs (Invalid
  redirect_uri)
type: gotcha
---

# Shared OIDC client skip-if-secret-exists guard drops new redirect URIs (Invalid redirect_uri)

When you add a NEW oauth2-proxy-gated dashboard (e.g. pgAdmin) to a leo-customer360 environment that ALREADY has other gated dashboards, the new dashboard's Keycloak login fails with **"Invalid redirect_uri"** even though the deploy succeeded.

**Root cause:** deploy-monitoring.sh SKIPS the Keycloak client bootstrap (bootstrap-oauth2-client.py) whenever OAUTH2_PROXY_CLIENT_SECRET is already present in .env. All gated dashboards share ONE confidential client (c360-oauth2-proxy), and each needs its own callback URI (e.g. http://<host>:5050/oauth2/callback) in the client's **Valid redirect URIs**. Because the bootstrap is skipped, the new dashboard's callback never gets registered.

**Fix (either):**
1. Manually add the new callback URI under the client's *Valid redirect URIs* in the Keycloak admin console, OR
2. Comment out OAUTH2_PROXY_CLIENT_SECRET in .env and re-run ./deploy-monitoring.sh <env> — the bootstrap then upserts ALL current redirect URIs and rewrites the same secret.

**General lesson:** any 'provision once, skip if secret exists' idempotency guard becomes a trap when the provisioned resource is a SHARED object (one OIDC client for N dashboards) that must be *amended* — not just created — each time you add a consumer. The skip-if-exists shortcut silently drops the amend.

Source: leo-customer360 deployments/monitoring (adding pgAdmin behind Keycloak SSO, 2026-08). Same trap the README documents for Jaeger.

See also [[Exposure model for ops dashboards behind an L4 (OIDC-incapable) load balancer]].

## Related

- [[Exposure model for ops dashboards behind an L4 (OIDC-incapable) load balancer]]

%% ai-graph-start %%

**Related notes:**
- [[Monitoring SSO-gate adding a dashboard needs a Keycloak redirect_uri re-sync]]
- [[Monitoring-dashboard SSO login user is c360admin, not the Keycloak master admin]]
- [[Keep Keycloak realm roles in sync with app authz constants, and ensure the bootstrap step is in the CD services list]]
- [[Serve a no-auth UI over TLS+SSO behind Caddy at a subpath (oauth2-proxy proxy-prefix)]]
- [[Exposure model for ops dashboards behind an L4 (OIDC-incapable) load balancer]]

%% ai-graph-end %%