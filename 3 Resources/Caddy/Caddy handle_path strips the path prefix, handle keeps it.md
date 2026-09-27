---
ai_hash: ee88e55880e98496
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 deployments/proxy, 2026-08
status: seedling
tags:
- caddy
- reverse-proxy
- routing
- fastapi
title: Caddy handle_path strips the path prefix, handle keeps it
type: howto
---

# Caddy handle_path strips the path prefix, handle keeps it

In a Caddyfile, `handle_path /p/*` **strips** the matched prefix before proxying, while `handle /p/*` forwards the path **as-is**. Choose based on what the upstream expects at its own root.

**How to apply (single public host, path-routed):**
- FastAPI app with `root_path=/c360api` → `handle_path /c360api/*` so the app receives its real `/api/v1/...` routes (root_path still fixes generated/doc URLs).
- Keycloak served under `/auth` (via `KC_HTTP_RELATIVE_PATH=/auth`) → `handle` (NOT handle_path): Keycloak already serves under `/auth`, so forward the prefix.
- The bare `handle { ... }` catch-all (e.g. the frontend) must be the LAST block; specific handle/handle_path blocks match first.

Apps that emit absolute URLs or fixed callback paths (Dagster, oauth2-proxy, Portainer) do NOT sub-path cleanly without app-side prefix config — a subdomain is usually cleaner than a path for those.

Related: [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]

%% ai-graph-start %%

**Related notes:**
- [[Stripping a path prefix at the proxy breaks framework auto-redirects; forward it un-stripped when the app has root_path]]
- [[Caddy path matcher p does not match the bare p; use a named matcher for both]]
- [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]
- [[Register one FastAPI handler under multiple path prefixes with add_api_route]]
- [[Serve a no-auth UI over TLS+SSO behind Caddy at a subpath (oauth2-proxy proxy-prefix)]]

%% ai-graph-end %%