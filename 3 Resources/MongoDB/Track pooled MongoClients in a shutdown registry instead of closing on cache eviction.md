---
ai_hash: e718b962ea5f1fb4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26
status: seedling
tags:
- mongodb
- mongoclient
- lifecycle
- luz-jsonstore
- cdi
- concurrency
- gotcha
title: Track pooled MongoClients in a shutdown registry instead of closing on cache
  eviction
type: lesson
---

# Track pooled MongoClients in a shutdown registry instead of closing on cache eviction

A `MongoClient` is a heavyweight, thread-safe connection pool meant to be long-lived and shared. If you cache clients in a **size-bounded LRU** and close each one in the cache eviction listener, you can close a client that another thread is **still using** (it fetched the reference just before the 101st tenant triggered eviction) -> `IllegalStateException: state should be: open`.

Safer pattern: do NOT close on eviction. Instead keep a separate registry of every client the factory has opened, and close them all in one `@PreDestroy` sweep at bean/app shutdown. The size-bounded cache cannot be the source of truth for "what needs closing" because it forgets evicted-but-still-live clients.

Wiring (in an `@ApplicationScoped` factory): `openClients.add(client)` on successful open, `openClients.remove(client)` in the close helper, and `@PreDestroy void shutdown(){ openClients.forEach(this::close); }`. The registry MUST be a thread-safe `ConcurrentHashMap.newKeySet()` because the `@ApplicationScoped` bean is a single instance shared across all concurrent request threads.

**Trade-off / gotcha:** closing only at `@PreDestroy` means evicted-but-referenced clients accumulate for the whole app lifetime -> fine if distinct-tenant count is bounded, a slow leak under high tenant churn (>cache size). If that matters, close on eviction WITH a grace delay or ref-count rather than never-until-shutdown.

**Concrete gotcha seen in luz_jsonstore `MongoClientFactory`:** a WIP edit removed the `evictionListener(close)` and added the `openClients` set + `PreDestroy` import but never wired `add`/`remove`/`@PreDestroy` — so the set was dead code AND nothing closed clients on eviction anymore (a leak). Half-done refactors like this are worse than either endpoint.

## Related

- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]

%% ai-graph-start %%

**Related notes:**
- [[Cache one MongoClient per tenant and close it on eviction]]
- [[Don't liveness-ping a cached DB client on every call]]
- [[putIfAbsent(Supplier) runs the loader under a global write lock]]
- [[Semaphore acquire before try leaks permits on static semaphores]]
- [[Luz services access MongoDB only through the luz_jsonstore REST API]]

%% ai-graph-end %%