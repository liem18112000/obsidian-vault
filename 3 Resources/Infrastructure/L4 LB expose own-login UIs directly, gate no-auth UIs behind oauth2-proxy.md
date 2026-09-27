---
ai_hash: 10850ebbddbcfe27
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-25
entities: []
source: session 2026-08-25 (leo-customer360)
status: seedling
tags:
- load-balancer
- oauth2-proxy
- security
- health-check
- architecture
title: 'L4 LB: expose own-login UIs directly, gate no-auth UIs behind oauth2-proxy'
type: lesson
---

# L4 LB: expose own-login UIs directly, gate no-auth UIs behind oauth2-proxy

An L4 (TCP) network load balancer cannot do OIDC/auth itself, so how you expose an admin UI behind it depends on whether the UI has its OWN login:

- **Has its own login** (Portainer, pgAdmin, redis-commander with HTTP basic auth) -> expose it **directly**: LB TCP listener -> the UI's own port. Simpler, and it avoids reverse-proxy pitfalls (e.g. Portainer's CSRF/Origin check rejects mutating requests behind a proxy).
- **No auth of its own** (Netdata, Jaeger UI) -> put **oauth2-proxy** on the box in front of it (LB -> oauth2-proxy -> Keycloak -> UI), or front it with Caddy on a TLS path.

**Two gotchas for the direct case:**
- **Health check:** a basic-auth UI returns HTTP 401 on `/`, so an HTTP 200 health check fails — use a plain **TCP** health check instead (same as an HTTPS/self-signed tool).
- **Cleartext caveat:** an L4 LB does no TLS, so if the UI serves plain HTTP the login crosses the wire in cleartext. Acceptable as a deliberate non-prod tradeoff for a single login; harden with Caddy TLS or the SSO gate.

Also: to let the LB reach a container UI, bind it to `0.0.0.0` (or the box's private ip), not `127.0.0.1` (which is tunnel-only). Surfaced exposing redis-commander for the CDP tracking broker.

## Related

- [[Loopback-bind a bridge container that must reach a host-network service]]

%% ai-graph-start %%

**Related notes:**
- [[Gating a dashboard behind Keycloak when the LB is L4 - use oauth2-proxy]]
- [[Exposure model for ops dashboards behind an L4 (OIDC-incapable) load balancer]]
- [[Serve a no-auth UI over TLS+SSO behind Caddy at a subpath (oauth2-proxy proxy-prefix)]]
- [[Loopback-bind a bridge container that must reach a host-network service]]
- [[Monitoring SSO-gate adding a dashboard needs a Keycloak redirect_uri re-sync]]

%% ai-graph-end %%