---
ai_hash: 620acc65a6b9a479
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26 luz_jsonstore MongoClientFactory
status: seedling
tags:
- mongodb
- connection-pool
- cache
- gotcha
- luz-jsonstore
title: Cache one MongoClient per tenant and close it on eviction
type: lesson
---

# Cache one MongoClient per tenant and close it on eviction

A `MongoClient` owns a **connection pool plus background server-monitor threads** and is thread-safe — the driver expects you to create **one and share it**, not build one per request. Creating a client per call silently leaks pools and monitor threads until GC (which is unreliable for this).

So cache one client per tenant/key. **Gotcha:** when that cache evicts an entry (LRU size cap), a plain `LinkedHashMap` / `SimpleCache` eviction just drops the reference — it does **not** `close()` the value. You must add a **close-on-evict hook** or every eviction leaks a pool. In `luz_jsonstore` this meant adding an optional `evictionListener(BiConsumer)` to `SimpleCache` so `MongoClientFactory` can close the evicted `MongoClient`.

## Related
[[LRU cache in Java via LinkedHashMap accessOrder + removeEldestEntry]]
[[Don't liveness-ping a cached DB client on every call|Dont liveness-ping a cached DB client on every call]]

## Related

- [[LRU cache in Java via LinkedHashMap accessOrder + removeEldestEntry]]
- [[Don't liveness-ping a cached DB client on every call]]

%% ai-graph-start %%

**Related notes:**
- [[Track pooled MongoClients in a shutdown registry instead of closing on cache eviction]]
- [[Don't liveness-ping a cached DB client on every call]]
- [[LRU cache in Java via LinkedHashMap accessOrder + removeEldestEntry]]
- [[putIfAbsent(Supplier) runs the loader under a global write lock]]
- [[Luz services access MongoDB only through the luz_jsonstore REST API]]

%% ai-graph-end %%