---
ai_hash: a8a7ecda8a7e27ca
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26 luz_jsonstore MongoClientFactory
status: seedling
tags:
- java
- concurrency
- cache
- locking
- gotcha
- luz-jsonstore
title: putIfAbsent(Supplier) runs the loader under a global write lock
type: lesson
---

# putIfAbsent(Supplier) runs the loader under a global write lock

A cache "compute-if-absent with a supplier" convenience can hide a concurrency trap: if it runs the value-supplier **while holding the caches global write lock**, then a *slow* value-load blocks reads and writes for **every other key**, not just the one being loaded.

Concrete case: `luz_jsonstore`s `SimpleCache.putIfAbsent(key, Supplier)` calls `supplier.get()` inside the `ReentrantReadWriteLock` write lock. If the supplier opens a Mongo connection (seconds under a bad network / `connectTimeoutMS`), all other tenants cache lookups stall behind it.

**Fix / decision:** for slow loaders that must stay concurrent across keys, dont use the supplier form. Use a plain `getIfPresent` fast path + a **per-key lock** around create-then-`put`, so different keys load in parallel while still opening exactly one value per key. Reserve `putIfAbsent(Supplier)` for cheap, non-blocking loaders.

## Related
[[LRU cache in Java via LinkedHashMap accessOrder + removeEldestEntry]]

## Related

- [[LRU cache in Java via LinkedHashMap accessOrder + removeEldestEntry]]

%% ai-graph-start %%

**Related notes:**
- [[Mongo unique-index insert as CAS when the cache has no putIfAbsent]]
- [[Cache one MongoClient per tenant and close it on eviction]]
- [[Per-pod single-flight kills cache stampede without semantic change]]
- [[Track pooled MongoClients in a shutdown registry instead of closing on cache eviction]]
- [[Two-tier cache must propagate caller TTL to every tier]]

%% ai-graph-end %%