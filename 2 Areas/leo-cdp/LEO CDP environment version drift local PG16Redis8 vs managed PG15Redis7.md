---
ai_hash: dc24aeabf267dc87
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities:
- LEO CDP environment
- version drift
- local environment
- managed environment
- LEO Customer 360
- data tier
- PostgreSQL
- Redis
- PG 16
- PG 15
- Redis 8
- Redis 7
- postgis/postgis:16-3.5
- redis:8-alpine
- UAT environment
- PROD environment
- Redis port 6580
- Redis port 6379
- REDIS_PORT
- REDIS_HOST_PORT
- SQL
- LEO CDP schema migrations
- dbmate
- alembic
- plain SQL
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
- LEO Customer 360 — *has* — data tier
- data tier — *in* — local environment
- data tier — *in* — managed environment
- local environment — *uses* — PG 16
- local environment — *uses* — Redis 8
- managed environment — *uses* — PG 15
- managed environment — *uses* — Redis 7
- PG 16 — *is version of* — PostgreSQL
- PG 15 — *is version of* — PostgreSQL
- Redis 8 — *is version of* — Redis
- Redis 7 — *is version of* — Redis
- local environment — *uses image* — postgis/postgis:16-3.5
- postgis/postgis:16-3.5 — *provides* — PG 16
- local environment — *uses image* — redis:8-alpine
- redis:8-alpine — *provides* — Redis 8
- managed environment — *includes* — UAT environment
- managed environment — *includes* — PROD environment
- Redis — *listens on* — Redis port 6580
- Redis port 6580 — *is* — non-standard
- Redis port 6379 — *is* — standard
- REDIS_PORT — *is set to* — 6580
- REDIS_HOST_PORT — *is set to* — 6580
- SQL — *is* — version-sensitive
- LEO CDP schema migrations — *are* — ordered plain SQL
- LEO CDP schema migrations — *are not* — dbmate
- LEO CDP schema migrations — *are not* — alembic

%% ai-graph-end %%