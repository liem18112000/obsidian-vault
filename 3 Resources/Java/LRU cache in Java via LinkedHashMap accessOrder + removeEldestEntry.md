---
ai_hash: e4b2ead4d197a58b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26 luz_jsonstore MongoClientFactory
status: seedling
tags:
- java
- cache
- lru
- linkedhashmap
- pattern
title: LRU cache in Java via LinkedHashMap accessOrder + removeEldestEntry
type: howto
---

# LRU cache in Java via LinkedHashMap accessOrder + removeEldestEntry

A fixed-capacity **LRU cache** falls out of `LinkedHashMap` almost for free:

```java
new LinkedHashMap<K,V>(capacity, 0.75f, /*accessOrder=*/true) {
    @Override protected boolean removeEldestEntry(Map.Entry<K,V> e) {
        return size() > MAX;   // evict least-recently-used once over cap
    }
};
```

- The third constructor arg `accessOrder=true` flips insertion-order to **access-order**: every `get`/`put` moves that entry to the tail (most-recently-used), so the *head* is the least-recently-used.
- `removeEldestEntry` is a **template-method hook** that `LinkedHashMap` calls after every insertion; return `true` and it drops the eldest (= LRU) entry.
- **Gotcha:** with `accessOrder=true` even a `get()` structurally mutates the map, so it is *not* thread-safe on its own — wrap it in `Collections.synchronizedMap(...)`.

Ref: baeldung.com/java-linked-hashmap.

## Related
[[Cache one MongoClient per tenant and close it on eviction]]

## Related

- [[Cache one MongoClient per tenant and close it on eviction]]

%% ai-graph-start %%

**Related notes:**
- [[Cache one MongoClient per tenant and close it on eviction]]
- [[Track pooled MongoClients in a shutdown registry instead of closing on cache eviction]]
- [[putIfAbsent(Supplier) runs the loader under a global write lock]]
- [[Don't liveness-ping a cached DB client on every call]]
- [[Per-pod single-flight kills cache stampede without semantic change]]

%% ai-graph-end %%