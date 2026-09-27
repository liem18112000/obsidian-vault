---
ai_hash: 2ea69173d5fe9250
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- jsonstore version-number CAS
created: 2026-08-10
entities: []
source: LUZ-158230 session 2026-08-10
status: seedling
tags:
- luz
- jsonstore
- mongodb
- optimistic-concurrency
- cas
title: luz-jsonstore optimistic-concurrency PATCH version-number is an equality CAS
type: concept
---

# luz-jsonstore optimistic-concurrency PATCH version-number is an equality CAS

luz-jsonstore supports optimistic concurrency on the generic Mongo document API:

`PATCH mdb/{tenant}/{collection}/{id}?version-number=N` performs an **equality compare-and-set** on the stored `_versionNumber` field before applying the update, and returns the raw Mongo `UpdateResult`. A **`matchedCount == 0`** means the guard rejected the write — the stored revision was not `N` (stale / out-of-order / duplicate). `matchedCount == 1` means it applied.

**Critical caveat:** jsonstore does **not** auto-increment `_versionNumber`. The update body must `$set` the new value itself (e.g. `{$set: {..., _versionNumber: N+1}}`). Forgetting this leaves every future CAS comparing against the same old N.

Use it to make redelivered / retried writes idempotent-safe: publish with `rev = current+1`, apply with `version-number = current`, and drop the message on `matchedCount == 0`. Behind a per-entity ordering key the rev guard is defense-in-depth.

**Open question:** behaviour when the stored `_versionNumber` is **absent** (first versioned write) vs `version-number=0` — verify against jsonstore before relying on it.

Surfaced on LUZ-158230 (import-job Tier 4) in `luz_docs_import`; the client method is `JsonStoreClient.updateByIdVersioned(...)`.

## Related

- [[Hosting a PubSub consumer in a luz service with luz_message_receiver|Hosting a Pub/Sub consumer in a luz service with luz_message_receiver]]

%% ai-graph-start %%

**Related notes:**
- [[Shared aggregate write targets need CAS, not plain $set]]
- [[Kepler PubSub topology HTTP-to-luz_message_broker publisher, luz_message_receiver pull-consumers, jsonstore version-number CAS]]
- [[luz_docs change tracking covers updateMany-deleteMany via projected before-after snapshots keyed by id]]
- [[Mongo unique-index insert as CAS when the cache has no putIfAbsent]]
- [[luz-docs updateManyByFilter requires every targeted document to actually change]]

%% ai-graph-end %%