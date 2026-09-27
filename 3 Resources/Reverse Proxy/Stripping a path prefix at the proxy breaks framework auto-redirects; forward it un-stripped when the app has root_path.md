---
ai_hash: 12f0ccafbfb85675
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 proxy Caddyfile, 2026-08
status: seedling
tags:
- reverse-proxy
- caddy
- fastapi
- root_path
- redirect
- gotcha
title: Stripping a path prefix at the proxy breaks framework auto-redirects; forward
  it un-stripped when the app has root_path
type: gotcha
---

# Stripping a path prefix at the proxy breaks framework auto-redirects; forward it un-stripped when the app has root_path

When an app is exposed under a path prefix (e.g. `/c360api`) by a reverse proxy, do NOT strip the prefix if the app declares its mount point via a base/root path (FastAPI `root_path=/c360api`, similar in others). Forward it UN-stripped (Caddy `handle` not `handle_path`; nginx: no trailing-slash on proxy_pass).

**Why:** framework-generated URLs — especially the automatic trailing-slash **307 redirect** (`/x/metadata` -> `/x/metadata/`) — are built from the request path the app received. If the proxy stripped the prefix, that Location comes back WITHOUT it (`https://host/api/v1/metadata/`, missing `/c360api`), the browser follows it to the wrong route (often the catch-all/frontend), and the call silently fails. Concrete symptom hit: the admin SPA fetched `/c360api/api/v1/metadata`, got a prefix-less 307, the fetch failed, and the login page fell back to "Dev mode (SSO disabled)" even though the API had SSO on.

With the prefix forwarded intact + `root_path` set, Starlette strips root_path for **routing** (matches `/api/v1/...`) yet keeps it in **generated URLs** (redirect Location includes `/c360api`). No `/c360api/c360api` doubling — the redirect uses scope path only. The trailing-slash URL (`.../metadata/`) worked directly all along; only the no-slash -> 307 path was broken.

Related: [[Caddy handle_path strips the path prefix, handle keeps it]]

## Related

- [[Caddy handle_path strips the path prefix, handle keeps it]]

%% ai-graph-start %%

**Related notes:**
- [[Caddy handle_path strips the path prefix, handle keeps it]]
- [[Register one FastAPI handler under multiple path prefixes with add_api_route]]
- [[Serve a SPA under a sub-path via the app base-path option, not proxy strip]]
- [[Caddy path matcher p does not match the bare p; use a named matcher for both]]
- [[Keycloak behind a TLS-terminating proxy needs proxy headers and hostname-strict off]]

%% ai-graph-end %%