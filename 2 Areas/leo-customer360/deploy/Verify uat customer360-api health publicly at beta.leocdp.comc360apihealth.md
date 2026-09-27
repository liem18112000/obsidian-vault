---
ai_hash: adc5ce7fa5ed0009
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities:
- uat customer360-api
- beta.leocdp.com
- Caddy
- SSH tunnel
- VM
- FastAPI app
- Starlette
- Postgres
- Keycloak
- ads-server
- data-tracking
- Jaeger
- leocdp.com
- SHA-pinned docker pulls accumulate and fill small deploy VM disks
- frontend
source: session 2026-09-05
status: seedling
tags:
- leo-customer360
- uat
- health-check
- caddy
- fastapi
- ops
title: Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health
type: howto
---

# Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health

The uat customer360-api can be health-checked **without an SSH tunnel** via the public Caddy route:

```
curl -sS https://beta.leocdp.com/c360api/health
# -> 200 {"status":"ok","database":"reachable","sso_login":true}
```

- The container itself listens on the VM at `:8008` (`--network host`) with health path `/health`, only reachable via `ssh -L 8008:localhost:8008 leocdp360@49.213.71.76`.
- Caddy fronts `beta.leocdp.com` (uat) and routes `/c360api/*` to the api **unstripped** (`handle /c360api/*`, not `handle_path`) because the FastAPI app sets `root_path=/c360api`. Starlette strips root_path for routing, so the app's `/health` route is publicly served at `/c360api/health`.
- Interpreting results: `database:reachable` confirms Postgres connectivity; `/c360api/` and `/c360api/docs` returning **401 Authentication required** is healthy (auth enforced), not an error.
- Other services (per Caddyfile): `/` -> frontend, `/auth` -> Keycloak, `/ads` -> ads-server, `/data` -> data-tracking (stripped), `/jaeger` -> trace UI. prod domain is `leocdp.com`.

Related: [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]].

## Related

- [[SHA-pinned docker pulls accumulate and fill small deploy VM disks]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 service dependency + health-probe map]]
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]
- [[Deploying Keycloak 26 as a container health port 9000, bootstrap admin, start vs start-dev]]
- [[Verify an authed health endpoint in-container, not by curl, in CI]]

**Relations:**
- uat customer360-api — *has public health check at* — beta.leocdp.com
- uat customer360-api — *can be health-checked without* — SSH tunnel
- Caddy — *fronts* — beta.leocdp.com
- Caddy — *routes /c360api/* to* — uat customer360-api
- FastAPI app — *is component of* — uat customer360-api
- FastAPI app — *sets* — root_path=/c360api
- Starlette — *strips* — root_path
- uat customer360-api — *uses* — Postgres
- VM — *hosts* — uat customer360-api
- beta.leocdp.com — *routes / to* — frontend
- beta.leocdp.com — *routes /auth to* — Keycloak
- beta.leocdp.com — *routes /ads to* — ads-server
- beta.leocdp.com — *routes /data to* — data-tracking
- beta.leocdp.com — *routes /jaeger to* — Jaeger
- leocdp.com — *is production domain for* — beta.leocdp.com
- SHA-pinned docker pulls accumulate and fill small deploy VM disks — *is related to* — uat customer360-api

%% ai-graph-end %%