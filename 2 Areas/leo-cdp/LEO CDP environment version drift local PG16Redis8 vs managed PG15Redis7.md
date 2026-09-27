---
ai_hash: f2d7c5868d3eda1a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities:
- LEO CDP environment
- local environment
- managed environment
- PostgreSQL
- PG 16
- PG 15
- Redis
- Redis 8
- Redis 7
- UAT
- PROD
- postgis/postgis:16-3.5
- redis:8-alpine
- Redis port
- '6580'
- '6379'
- LEO Customer 360
- SQL
- LEO CDP schema migrations
source: leo-customer360 release-doc work, session 2026-08-26
status: seedling
tags:
- leo-cdp
- postgres
- redis
- gotcha
- environments
title: 'LEO CDP environment version drift: local PG16/Redis8 vs managed PG15/Redis7'
type: gotcha
---

# LEO CDP environment version drift: local PG16/Redis8 vs managed PG15/Redis7

In LEO Customer 360 the **local/Compose data tier versions differ from the managed UAT/PROD ones**, so version-sensitive SQL/behaviour must be validated against the *managed* versions before shipping:

- PostgreSQL: local image `postgis/postgis:16-3.5` = **PG 16**, but managed UAT/PROD = **PG 15**.
- Redis: local image `redis:8-alpine` = **8**, but managed = **Redis 7** (UAT container / PROD MemStore).

Second gotcha: **Redis listens on the non-standard port `6580`** (not 6379) throughout the stack — `REDIS_PORT=6580`, `REDIS_HOST_PORT=6580`. Point local tooling at 6580.

Why it matters: something that works on PG 16 locally can fail on PG 15 in prod; assuming 6379 makes local Redis connections silently fail.

## Related

- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]

%% ai-graph-start %%

**Related notes:**
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]
- [[Customer360 redis.conf omits port so redis listens on 6379 not 6580]]
- [[leo-customer360 Redis is a fail-open cacheauth-cacherate-limiter used only by customer360-api]]
- [[Running the LEO CDP GHCR image needs mounted configs (image ships JARs only)]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]

**Relations:**
- LEO CDP environment — *has* — version drift
- local environment — *uses* — PG 16
- local environment — *uses* — Redis 8
- managed environment — *uses* — PG 15
- managed environment — *uses* — Redis 7
- PG 16 — *is_version_of* — PostgreSQL
- PG 15 — *is_version_of* — PostgreSQL
- Redis 8 — *is_version_of* — Redis
- Redis 7 — *is_version_of* — Redis
- local environment — *uses_image* — postgis/postgis:16-3.5
- local environment — *uses_image* — redis:8-alpine
- managed environment — *includes* — UAT
- managed environment — *includes* — PROD
- Redis — *listens_on_port* — 6580
- 6580 — *is_a* — non-standard port
- 6379 — *is_a* — standard port
- LEO Customer 360 — *contains* — local environment
- LEO Customer 360 — *contains* — managed environment
- SQL — *is* — version-sensitive
- LEO CDP schema migrations — *is_related_to* — LEO CDP environment

%% ai-graph-end %%