---
ai_hash: 936505a6caa06ee1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26 luz_jsonstore MongoClientFactory
status: seedling
tags:
- mongodb
- performance
- cache
- connection-pool
- luz-jsonstore
title: Don't liveness-ping a cached DB client on every call
type: lesson
---

# Don't liveness-ping a cached DB client on every call

Adding a per-call liveness probe to a cached DB client — e.g. `mongoClient.getDatabase(t).listCollectionNames().first()` on every `getClient()` — puts a **full network round-trip on the hot path of every request**. It defeats most of the caching benefit.

It is usually unnecessary: the MongoDB drivers connection pool already runs background server monitoring and **reconnects on its own**. Prefer to **return the cached client directly**, and handle the rare genuinely-dead client at the call site — catch the error, `invalidate(tenantId)` to evict + close it, and retry (re-discovery is cheap).

Tradeoff to accept knowingly: without the probe, a client whose backend *relocated* (e.g. tenant moved to a different replica set) wont self-heal until something calls `invalidate`.

## Related
[[Cache one MongoClient per tenant and close it on eviction]]

## Related

- [[Cache one MongoClient per tenant and close it on eviction]]

%% ai-graph-start %%

**Related notes:**
- [[Cache one MongoClient per tenant and close it on eviction]]
- [[Track pooled MongoClients in a shutdown registry instead of closing on cache eviction]]
- [[LRU cache in Java via LinkedHashMap accessOrder + removeEldestEntry]]
- [[Truncating DB collections between benchmark runs resets data but not service warmth]]
- [[Luz services access MongoDB only through the luz_jsonstore REST API]]

%% ai-graph-end %%