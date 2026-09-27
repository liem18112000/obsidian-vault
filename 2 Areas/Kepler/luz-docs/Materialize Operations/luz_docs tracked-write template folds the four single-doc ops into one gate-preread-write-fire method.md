---
ai_hash: e86ec90cb8359fe9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-05
entities:
- luz_docs tracked-write template
- TrackingJsonStoreClient
- gate-preread-write-fire method
- tracked single-doc writes
- insert
- replace
- update
- delete
- template method
- tracked(String collection, Set<String> touchedFields, Supplier<JsonObject> beforeReader,
  Supplier<Response> write, BiConsumer<JsonObject, Response> onSuccess)
- gate
- optional pre-read
- write
- fire-and-forget onSuccess
- track() wrapper
- touchedFields
- beforeReader
- write Supplier
- raw client call
- bug surface
- JDK Supplier
- JDK BiConsumer
- luz_docs DocumentChangeObserver base owns the reload-recompute-restamp template
- Intercept an MP REST client by implementing its interface - unqualified inject resolves
  the wrapper, RestClient qualifier is the bypass
source: TrackingJsonStoreClient template extraction, session 2026-06-05
status: budding
tags:
- luz-docs
- change-tracking
- template-method
- refactoring
title: luz_docs tracked-write template folds the four single-doc ops into one gate-preread-write-fire
  method
type: model
---

# luz_docs tracked-write template folds the four single-doc ops into one gate-preread-write-fire method

luz_docs `TrackingJsonStoreClient` folds its four tracked single-doc writes (insert/replace/update/delete) into one template method instead of four copies of the same flow:

```java
private Response tracked(String collection, Set<String> touchedFields, Supplier<JsonObject> beforeReader,
                         Supplier<Response> write, BiConsumer<JsonObject, Response> onSuccess)
```

Flow: gate (untracked collection / suppression / untouched fields) -> optional pre-read -> write -> fire-and-forget `onSuccess(before, response)` inside the never-throw `track()` wrapper. Null is the skip signal: `touchedFields == null` means "whole document may change, collection membership alone gates" (replace, delete); `beforeReader == null` means "no before state" (insert). The write `Supplier` is the single source for both the bypass and tracked branches — previously every method spelled the raw client call twice, which is the bug surface this removes (branches drifting apart). Plain JDK `Supplier`/`BiConsumer`, no custom functional interface.

## Related

- [[luz_docs DocumentChangeObserver base owns the reload-recompute-restamp template]]
- [[Intercept an MP REST client by implementing its interface - unqualified inject resolves the wrapper, RestClient qualifier is the bypass]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs JsonStore change tracking via client-layer wrapper and CDI async events]]
- [[luz_docs change tracking covers updateMany-deleteMany via projected before-after snapshots keyed by id]]
- [[luz_docs DocumentChangeObserver base owns the reload-recompute-restamp template]]
- [[Diff-based write tracking dies silently if the write runs before the pre-read]]
- [[Intercept an MP REST client by implementing its interface - unqualified inject resolves the wrapper, RestClient qualifier is the bypass]]

**Relations:**
- luz_docs tracked-write template — *folds* — tracked single-doc writes
- luz_docs tracked-write template — *uses* — gate-preread-write-fire method
- TrackingJsonStoreClient — *uses* — luz_docs tracked-write template
- TrackingJsonStoreClient — *folds* — tracked single-doc writes
- tracked single-doc writes — *include* — insert
- tracked single-doc writes — *include* — replace
- tracked single-doc writes — *include* — update
- tracked single-doc writes — *include* — delete
- template method — *is defined as* — tracked(String collection, Set<String> touchedFields, Supplier<JsonObject> beforeReader, Supplier<Response> write, BiConsumer<JsonObject, Response> onSuccess)
- gate-preread-write-fire method — *consists of* — gate
- gate-preread-write-fire method — *consists of* — optional pre-read
- gate-preread-write-fire method — *consists of* — write
- gate-preread-write-fire method — *consists of* — fire-and-forget onSuccess
- fire-and-forget onSuccess — *is wrapped by* — track() wrapper
- touchedFields — *signals* — whole document may change
- whole document may change — *applies to* — replace
- whole document may change — *applies to* — delete
- beforeReader — *signals* — no before state
- no before state — *applies to* — insert
- write Supplier — *is source for* — bypass
- write Supplier — *is source for* — tracked branches
- write Supplier — *removes* — bug surface
- bug surface — *caused by* — raw client call
- template method — *uses* — JDK Supplier
- template method — *uses* — JDK BiConsumer
- luz_docs tracked-write template — *is related to* — luz_docs DocumentChangeObserver base owns the reload-recompute-restamp template
- luz_docs tracked-write template — *is related to* — Intercept an MP REST client by implementing its interface - unqualified inject resolves the wrapper, RestClient qualifier is the bypass

%% ai-graph-end %%