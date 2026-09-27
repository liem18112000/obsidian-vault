---
ai_hash: 7f44c61ba0752aa1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities:
- leo-customer360
- Caddy
- beta.leocdp.com
- leocdp.com
- customer360-api
- frontend
- Keycloak
- ads-server
- data-tracking-api
- trace UI
- PostgreSQL
- Redis
- S3
- FastAPI
- S3ObjectStorage.check_connection()
- TrackingRequestProtection.ping()
- get_storage
- get_protection
- DB SELECT 1
- S3 client.list_buckets()
- Redis client.ping()
- feat/health-dependency-checks
- Health checks should probe dependencies and split critical vs fail-open
- Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health
- core/cache.py
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

Landed on branch `feat/health-dependency-checks` (2026-09-05). Concept: [[Health checks should probe dependencies and split critical vs fail-open]]. Verify uat: [[Verify uat customer360-api health publicly at beta.leocdp.comc360apihealth|Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health]].

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
- leo-customer360 — *uses* — Caddy
- Caddy — *routes_to* — frontend
- Caddy — *routes_to* — customer360-api
- Caddy — *routes_to* — Keycloak
- Caddy — *routes_to* — ads-server
- Caddy — *routes_to* — data-tracking-api
- Caddy — *routes_to* — trace UI
- beta.leocdp.com — *is_uat_environment_for* — leo-customer360
- leocdp.com — *is_prod_environment_for* — leo-customer360
- customer360-api — *depends_on* — PostgreSQL
- customer360-api — *has_health_endpoint* — /c360api/health
- customer360-api — *health_probe_checks* — PostgreSQL
- customer360-api — *health_report_field* — status
- customer360-api — *health_report_field* — database:reachable
- customer360-api — *health_report_field* — sso_login
- ads-server — *depends_on* — PostgreSQL
- ads-server — *uses_postgresql_schema* — leo_ads
- ads-server — *health_probe_checks* — PostgreSQL
- ads-server — *has_health_endpoint* — /health/database
- ads-server — *health_report_field* — database:reachable
- ads-server — *health_report_field* — status:error
- ads-server — *health_report_field* — database:unreachable
- ads-server — *does_not_use* — Redis
- ads-server — *does_not_use* — core/cache.py
- data-tracking-api — *depends_on* — S3
- data-tracking-api — *depends_on* — Redis
- data-tracking-api — *health_probe_checks* — S3
- data-tracking-api — *health_probe_checks* — Redis
- data-tracking-api — *health_report_field* — s3
- data-tracking-api — *health_report_field* — redis
- data-tracking-api — *health_report_field* — storage_mode
- S3 — *is_critical_dependency_for* — data-tracking-api
- Redis — *is_fail_open_dependency_for* — data-tracking-api
- DB SELECT 1 — *is_probe_method_for* — PostgreSQL
- S3 client.list_buckets() — *is_probe_method_for* — S3
- Redis client.ping() — *is_probe_method_for* — Redis
- S3ObjectStorage.check_connection() — *is_used_by* — data-tracking-api
- TrackingRequestProtection.ping() — *is_used_by* — data-tracking-api
- FastAPI — *injects_dependency* — get_storage
- FastAPI — *injects_dependency* — get_protection
- feat/health-dependency-checks — *is_branch_for_health_probes* — leo-customer360
- feat/health-dependency-checks — *landed_on_date* — 2026-09-05
- Health checks should probe dependencies and split critical vs fail-open — *is_concept* — leo-customer360
- Health checks should probe dependencies and split critical vs fail-open — *is_related_to* — Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health
- Verify uat customer360-api health publicly at beta.leocdp.com/c360api/health — *is_verification_task* — customer360-api

%% ai-graph-end %%