---
ai_hash: d173e6c68b24556d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
tags:
- test-agent-v2
- cache
- redis
- gcp
- terraform
- architecture
---

# Redis benchmark cache — pluggable port + the Memorystore/VPC gotcha

Added a cache layer in front of the benchmark GCS blob store, built as a **ports-and-adapters port**
mirroring the existing `common/store` ObjectStore pattern (so infra is swappable + local-friendly).

## The port
`common/cache/`: `Cache` Protocol (`get/set/delete`, string values) + three adapters —
`NullCache` (default no-op), `InMemoryCache` (per-process, TTL via `time.monotonic`), `RedisCache`
(GCP Memorystore; `redis` client lazy-imported so local/test never needs it). `build_cache()` selects
on `CACHE_BACKEND` (`none`|`memory`|`redis`); `get_cache()` is an `@lru_cache` singleton.

**Design choices:**
- Default `none` → NullCache → **zero behavior change** until enabled, and offline tests stay
  cache-free. Prod sets `CACHE_BACKEND=redis`; local dev can set `memory`.
- Benchmark store = **read-through / write-through**: read = cache → GCS (populate on miss, GCS is the
  source of truth); write = GCS then cache. Shared Redis keeps all Cloud Run instances coherent
  (an InMemoryCache would be per-instance, so only useful for a single local process).
- Every Redis op **degrades to a miss/no-op on error** (cache is an optimization; a Redis outage must
  never break a benchmark read). `RedisCache(client=...)` injection makes it testable with a fake.

## The deployment gotcha (why it's not just "add a Redis instance")
**Memorystore Redis has a private IP inside the VPC — Cloud Run cannot reach it directly.** You must
add a **Serverless VPC Access connector** and attach it to the service
(`template { vpc_access { connector, egress = PRIVATE_RANGES_ONLY } }`). So the Terraform is three
resources, not one: `google_redis_instance` + `google_vpc_access_connector` + the APIs
(`redis.googleapis.com`, `vpcaccess.googleapis.com`). The connector needs an **unused /28** CIDR.

All gated by `var.deploy_redis` (default false) so existing deploys are untouched. Only the **TEV**
service gets the connector + `CACHE_BACKEND/REDIS_HOST/REDIS_PORT` env — TEV is the only process that
reads/writes benchmarks. Enable with `deploy_redis = true` in tfvars.

## Test hygiene
Clear `CACHE_BACKEND` in the session conftest fixture (same dotenv-leak risk as VERTEX_*) and call
`get_cache.cache_clear()` — else a local `.env` `CACHE_BACKEND=redis` makes offline tests dial Redis.

Related: [[test-agent-v2 run benchmark — TEV ownership forced by layering]] · [[dotenv leaks VERTEX into offline tests]]

%% ai-graph-start %%

**Related notes:**
- [[redis_proxy.sh needs compute firewall + VM + IAP permissions]]
- [[test-agent-v2 Redis deploy blocked by vpcaccess.connectors.create IAM denial]]
- [[test-agent-v2 Redis VPC connector stuck in ERROR (CIDRnetwork misconfig)]]
- [[Memorystore Redis has no auth-proxy — local access needs an IAP jump VM]]
- [[test-agent-v2 run benchmark — TEV ownership forced by layering]]

%% ai-graph-end %%