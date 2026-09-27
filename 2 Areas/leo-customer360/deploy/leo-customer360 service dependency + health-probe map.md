---
ai_hash: 0d1539ae770767a1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities:
- leo-customer360
- Caddy
- beta.leocdp.com
- leocdp.com
- customer360-api
- PostgreSQL
- ads-server
- Redis
- data-tracking-api
- S3
- Keycloak
- Jaeger
- frontend
- feat/health-dependency-checks
- Health checks should probe dependencies and split critical vs fail-open
- Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health
- DB SELECT 1
- S3 client.list_buckets()
- Redis client.ping()
- S3ObjectStorage.check_connection()
- TrackingRequestProtection.ping()
- FastAPI
- get_storage
- get_protection
- leo_ads
source: session 2026-09-05
status: seedling
tags:
- leo-customer360
- health-check
- ads-server
- data-tracking-api
- dependencies
- ops
title: leo-customer360 service dependency + health-probe map
type: reference
---

# leo-customer360 service dependency + health-probe map

Dependency map + `/health` semantics for the leo-customer360 services (uat behind Caddy on `beta.leocdp.com`; prod `leocdp.com`). Caddy routes: `/`->frontend, `/c360api`->api (root_path, unstripped), `/auth`->Keycloak, `/ads`->ads-server, `/data`->tracking (stripped), `/jaeger`->trace UI.

| Service | Real dependencies | `/health` behavior |
|---|---|---|
| customer360-api | PostgreSQL | probes DB (`SELECT 1`); `{status, database:reachable, sso_login}`. Public: `/c360api/health`. (Note: still raises->500 if DB down, not yet graceful.) |
| ads-server | PostgreSQL (schema `leo_ads`) only. Redis is aspirational in docstrings but NOT wired (no `core/cache.py`; redis config unused) | probes DB; 200 `database:reachable` / **503** `status:error, database:unreachable`. Also has `/health/database`. |
| data-tracking-api | **S3** object storage (critical sink) + **Redis** (rate-limit + session counters, fail-open) | probes both; 200 ok / 200 **degraded** (redis down) / **503 error** (s3 down). Reports `s3`, `redis`, `storage_mode`. |

Cheap probes used: DB `SELECT 1`; S3 `client.list_buckets()` (also validates creds); Redis `client.ping()`. The tracking probes were added as `S3ObjectStorage.check_connection()` and `TrackingRequestProtection.ping()`; the FastAPI `/health` injects the storage/protection singletons via `Depends(get_storage/get_protection)`.

Landed on branch `feat/health-dependency-checks` (2026-09-05). Concept: [[Health checks should probe dependencies and split critical vs fail-open]]. Verify uat: [[Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health]].

## Related

- [[Health checks should probe dependencies and split critical vs fail-open]]

%% ai-graph-start %%

**Related notes:**
- [[Verify uat customer360-api health publicly at beta.leocdp.comc360apihealth]]
- [[Health checks should probe dependencies and split critical vs fail-open]]
- [[leo-customer360 Redis is a fail-open cacheauth-cacherate-limiter used only by customer360-api]]
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]

**Relations:**
- leo-customer360 — *is behind* — Caddy
- leo-customer360 — *deployed on* — beta.leocdp.com
- leo-customer360 — *deployed on* — leocdp.com
- Caddy — *routes to* — frontend
- Caddy — *routes to* — customer360-api
- Caddy — *routes to* — Keycloak
- Caddy — *routes to* — ads-server
- Caddy — *routes to* — data-tracking-api
- Caddy — *routes to* — Jaeger
- customer360-api — *depends on* — PostgreSQL
- customer360-api — *probes* — PostgreSQL
- customer360-api — *has health endpoint* — /c360api/health
- customer360-api — *reports status for* — database:reachable
- customer360-api — *reports status for* — sso_login
- ads-server — *depends on* — PostgreSQL
- ads-server — *uses schema* — leo_ads
- ads-server — *probes* — PostgreSQL
- ads-server — *has aspirational dependency* — Redis
- ads-server — *has health endpoint* — /health/database
- ads-server — *reports status for* — database:reachable
- ads-server — *reports status for* — database:unreachable
- data-tracking-api — *depends on* — S3
- data-tracking-api — *depends on* — Redis
- data-tracking-api — *probes* — S3
- data-tracking-api — *probes* — Redis
- data-tracking-api — *uses* — S3ObjectStorage.check_connection()
- data-tracking-api — *uses* — TrackingRequestProtection.ping()
- data-tracking-api — *built with* — FastAPI
- data-tracking-api — *reports status for* — s3
- data-tracking-api — *reports status for* — redis
- data-tracking-api — *reports status for* — storage_mode
- FastAPI — *injects* — get_storage
- FastAPI — *injects* — get_protection
- PostgreSQL — *probed by* — DB SELECT 1
- S3 — *probed by* — S3 client.list_buckets()
- Redis — *probed by* — Redis client.ping()
- feat/health-dependency-checks — *landed on* — 2026-09-05
- feat/health-dependency-checks — *implements concept* — Health checks should probe dependencies and split critical vs fail-open
- Health checks should probe dependencies and split critical vs fail-open — *is related to* — leo-customer360
- Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health — *verifies* — customer360-api

%% ai-graph-end %%