---
ai_hash: a885ce3f98d24c08
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: session 2026-08-19
status: seedling
tags:
- portainer
- oauth2-proxy
- csrf
- reverse-proxy
- keycloak
- gotcha
title: Portainer CSRF origin-invalid behind a reverse proxy - expose it directly
type: howto
---

# Portainer CSRF "origin invalid" behind a reverse proxy — expose it directly

**Symptom:** in Portainer, a mutating action (e.g. "Unable to create tag") fails with
**"Forbidden - origin invalid"** when Portainer is reached through a reverse proxy
(oauth2-proxy, nginx, an L7 LB).

**Cause:** Portainer enforces a CSRF check that compares the request's `Origin` header host
to the host Portainer itself sees. A proxy hop changes `Host`/`Origin` (proxy talks to the
upstream as `127.0.0.1:9443` while the browser's Origin is the public URL) → mismatch →
Portainer rejects all POST/PUT.

**Fix / decision:** don't put Portainer CE behind an auth proxy. It already has its OWN
login, so expose it **directly** (e.g. LB → Portainer `:9443`, L4 TLS passthrough). Reserve
the SSO gate (oauth2-proxy → Keycloak) for tools that have NO auth of their own, like the
Netdata agent. Gating Portainer would require Portainer **EE** (native OIDC) or a proxy
carefully rewriting `Origin` to match — rarely worth it.

Applied in `leo-customer360` `deployments/monitoring` as a per-dashboard flag
`portainer_sso=false` / `netdata_sso=true`. Related:
[[Gating a dashboard behind Keycloak when the LB is L4 - use oauth2-proxy]]

%% ai-graph-start %%

**Related notes:**
- [[Gating a dashboard behind Keycloak when the LB is L4 - use oauth2-proxy]]
- [[Exposure model for ops dashboards behind an L4 (OIDC-incapable) load balancer]]
- [[Monitoring SSO-gate adding a dashboard needs a Keycloak redirect_uri re-sync]]
- [[L4 LB expose own-login UIs directly, gate no-auth UIs behind oauth2-proxy]]
- [[Serve a no-auth UI over TLS+SSO behind Caddy at a subpath (oauth2-proxy proxy-prefix)]]

%% ai-graph-end %%