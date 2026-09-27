---
ai_hash: f1de83836f9daae4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26
status: seedling
tags:
- luz-jsonstore
- bson
- bug
- gotcha
title: luz_jsonstore committed V2 updateOne count delete have latent BSON serialization
  bug
type: observation
---

# luz_jsonstore committed V2 updateOne count delete have latent BSON serialization bug

The prototype V2 endpoints in `JsonStoreMongoDbResourceV2` — `updateOne` (returns `UpdateResult`), `count` (returns `long`), and `delete` (returns `DeleteResult`) — carry a **latent serialization bug**: their return types are not `org.bson.Document`, and the V2 layer produces `application/bson`, which only `BsonProvider` (Document-only) can write. So these responses will fail to serialize (no eligible writer → 500) once actually exercised.

Only `addOne` and `get` are safe today, because both return a `Document`.

Fix by having the service return a `Document` (or wrapping the driver result in one at the resource boundary) — same rule as [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]. Worth fixing in the same pass as porting V1 `updateMany` / `updateBulk` to V2.

**RESOLVED (session 2026-08-26):** fixed while porting V1 `updateMany`/`updateBulk` to V2. The resource keeps the service returning native driver types (`UpdateResult`/`DeleteResult`/`BulkWriteResult`/`long`) and folds them into a counts `Document` at the HTTP boundary via private `toDocument(...)` overloads — `{matchedCount, modifiedCount}`, `{deletedCount}`, `{count}`. `delete` also switched from `Response.noContent()` (204, must not carry a body) to `Response.ok()` (200). `BsonProvider` was type-gated to `Document` so any future non-Document return is rejected at wiring rather than blowing up in `writeTo`.

## Related

- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]

%% ai-graph-start %%

**Related notes:**
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[Use a BSON endpoint instead of JSON to store MongoDB dates as native Date]]
- [[luz-jsonstore find returns 200 empty string, not [], on zero matches]]
- [[luz_jsonstore silently drops _shard on $set updates (HTTP 200, no persist)]]
- [[A JAX-RS body param typed org.bson.Document is JSON-deserialized, not a BSON wire format]]

%% ai-graph-end %%