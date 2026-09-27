---
ai_hash: 86e2f9e57fc6e371
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: LEO CDP UAT 2026-09-09
status: seedling
tags:
- architecture
- redis
- cache
- idempotency
- dagster
- decision
title: A dedup/idempotency cache belongs co-located with the worker, not in a shared
  cross-service cache
type: lesson
---

# A dedup/idempotency cache belongs co-located with the worker, not in a shared cross-service cache

When a worker uses Redis only as a **processing cache** — idempotency locks + "already-processed" markers + intermediate counters — and the **durable output lives elsewhere** (e.g. Postgres totals), the cache should be **co-located with the worker**, not the shared cross-service Redis.

**Why:** a local cache removes a cross-box network hop and its firewall/security-group rule, removes a shared-state coupling, and is simpler to operate. The cache is reconstructible (that is what makes it a cache); the source of truth is the DB. Give it light persistence (Redis `appendonly yes` + a volume) so the "already-processed" markers survive a restart and logs are not needlessly reprocessed.

**Concrete case (LEO CDP Dagster jobs box):** `analytics_job` wrote `analytics:data-source-lock:` / `analytics:data-source-state:` keys to Redis and its hourly totals to Postgres. Instead of opening the jobs box -> api-box Redis (`10.100.1.5:6580`) and adding a secgroup rule + terraform apply, we run a local `redis:7-alpine` container on the jobs box (`127.0.0.1:6580`, appendonly) shared by all Dagster tasks. Containers on `--network host` with the app default `REDIS_HOST=localhost` reach it with zero config.

**Contrast:** if the Redis data were READ by another service (dashboards), it would have to be the shared instance — check who consumes the keys before deciding local vs shared.

%% ai-graph-start %%

**Related notes:**
- [[Broker Redis needs opposite config from cache Redis, run it separately]]
- [[Scaling Dagster needs Postgres storage and a singleton daemon before adding replicas]]
- [[leo-customer360 Redis is a fail-open cacheauth-cacherate-limiter used only by customer360-api]]
- [[Run an event-broker Redis as a dedicated co-located instance, not the cache Redis]]
- [[Redis Streams consumer-group at-least-once Loader pattern]]

%% ai-graph-end %%