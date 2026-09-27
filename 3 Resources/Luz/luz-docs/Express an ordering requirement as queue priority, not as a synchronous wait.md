---
ai_hash: 306cf38d2fb324b7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: High-Level Design - ONE API Enricher-First Integration (2025-12-29)'
status: seedling
tags:
- luz-docs
- enricher
- one-api
- async
- api-design
- architecture
- kepler
title: Express an ordering requirement as queue priority, not as a synchronous wait
type: lesson
---

# Express an ordering requirement as queue priority, not as a synchronous wait

ONE API sends marketing letters whose documents must be **enriched before delivery**, but document creation must stay fast. The design resolves that with a flag rather than a synchronous wait:

```
POST /documents?isEnricherFirst=true  →  luz-docs
   save metadata to Mongo → upload file to GCS → return HTTP 201
                                                      ↓
                                      trigger enrichment event (async)
```

Two decisions carry the design:

- **`isEnricherFirst: bool`** — optional, defaults to `false`, so every existing caller is unaffected. A new behaviour gated behind an opt-in parameter needs no migration and no coordinated release.
- **The flag selects a priority, not a code path.** When `true`, enrichment is queued as `URGENT`; when `false`, priority is derived from the document's origin as before. The pipeline is identical — only its position in the queue changes.

That second point is the transferable one. The naive reading of "enricher-first" is *block the response until enrichment finishes*, which would couple creation latency to a downstream service and hand callers a timeout risk they did not have. Instead the ordering requirement is expressed as **queue priority**, so the caller still gets its `201` immediately and the guarantee is maintained by the scheduler.

The trade being accepted: priority is not a barrier. `URGENT` makes enrichment-before-delivery overwhelmingly likely, not certain — delivery still has to check enrichment status rather than assume it. Worth being explicit about, because "we made it urgent" quietly reads as "we made it ordered".

## Related

- [[Silently-ignored input needs a visible reason field, or it looks like data loss]]

%% ai-graph-start %%

**Related notes:**
- [[High-Level Design - ONE API Enricher-First Integration]]
- [[If upstream holds memory until you ack, your write latency is their OOM risk]]
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]
- [[Invoice Run, ePost backend storage]]
- [[Adapt to support ONE API - Enricher first delivery]]

%% ai-graph-end %%