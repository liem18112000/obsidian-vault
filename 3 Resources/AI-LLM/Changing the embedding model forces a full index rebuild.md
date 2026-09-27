---
ai_hash: 90cecd886831b401
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Embedding model
- Index rebuild
- v1 vectors
- v2 vectors
- Similarity computation
- Cosine distance
- v1 document vector
- v2 query vector
- Migration
- Downtime
- Blue/green deployment
- v2 index
- v1 index
- Query
- Dual-write
- Transition
- Documents
- Tenant's pointer
- Backfill
- Serving layer
- Routing
- Index version
- Blue/green transition
- Step 2 (of migration)
- Double computation
- Double writes
- Transition window
- Re-embedding
- Existing document
- Mitigation
- Migration cost
- Chunking output
- OCR text
- Extracted metadata
- Cache
- Content hash
- Embedding step
- Per-tenant indices
- Tenant A
- Tenant B
- All-at-once cutover
- Gradual rollout
- Retrieval quality
- Raw text
- Chunk boundaries
- Re-OCR
- Re-chunk
- Retrieval pipeline
- Vector
- Version the whole retrieval pipeline, not just the model
- IR - System Design
- AI
- Confluence
source: 'Confluence: IR - System Design (AI)'
status: seedling
tags:
- embeddings
- vector-search
- migration
- blue-green
- rag
- confluence-distilled
title: Changing the embedding model forces a full index rebuild
type: lesson
---

# Changing the embedding model forces a full index rebuild

You cannot upgrade an embedding model in place. **v1 and v2 vectors live in different spaces and cannot coexist in one similarity computation** — a cosine distance between a v1 document vector and a v2 query vector is a meaningless number, not a slightly worse one. Changing the model means rebuilding the entire index.

The migration that avoids downtime is blue/green at the index level:

1. **Build the v2 index in the background** while v1 keeps serving every query.
2. **Dual-write during the transition** — new and updated documents are written to *both* indices, so v2 does not fall behind while it backfills.
3. **Flip the tenant's pointer atomically** to v2 when the backfill completes.
4. **Delete v1.**

This is why the serving layer needs **routing** as a first-class responsibility — directing each query to the appropriate index *version* is what makes the blue→green transition smooth, and it must exist before you ever need to migrate.

**The expensive part is step 2.** Double computation and double writes for the whole transition window, on top of re-embedding every existing document. The design names the mitigation plainly: *"the ability to reuse previous results and to recompute only the needed elements can cut migration cost enormously."* Chunking output, OCR text, and extracted metadata often do **not** change when only the embedding model changes — so cache them, keyed by content hash, and re-run only the embedding step.

> [!tip] Per-tenant indices let you migrate incrementally
> With one index per tenant you can run **tenant A on v1 and tenant B on v2** simultaneously. That converts a terrifying all-at-once cutover into a gradual rollout with real traffic on both versions — and gives you a population to compare retrieval quality against before committing.

> [!warning] Budget the rebuild before you choose the model
> "We'll upgrade the embedding model later" is a decision with a known, large cost attached: re-embedding every document you have ever indexed, plus a dual-write window. Estimate it at design time. It is also a strong argument for keeping the raw text and chunk boundaries durable — if you have to re-OCR and re-chunk as well, the rebuild is several times more expensive.

Related: [[Version the whole retrieval pipeline, not just the model]] — the discipline that makes "which version produced this vector?" answerable at all.

Source: [[IR - System Design]] (AI, Confluence).

## Related

- [[Version the whole retrieval pipeline, not just the model]]

%% ai-graph-start %%

**Related notes:**
- [[Version the whole retrieval pipeline, not just the model]]
- [[IR - System Design]]
- [[Embedding a search library means building its control plane yourself]]
- [[Lucene's write-once segments turn replication into a filename diff]]
- [[Agent Memory]]

**Relations:**
- Embedding model — *forces* — Index rebuild
- v1 vectors — *cannot coexist with* — v2 vectors
- v1 vectors — *cannot coexist in* — Similarity computation
- v2 vectors — *cannot coexist in* — Similarity computation
- Cosine distance — *between* — v1 document vector
- Cosine distance — *and* — v2 query vector
- Cosine distance — *is* — meaningless number
- Migration — *avoids* — Downtime
- Migration — *is a form of* — Blue/green deployment
- Blue/green deployment — *involves building* — v2 index
- v2 index — *is built in* — background
- v1 index — *keeps serving* — Query
- Dual-write — *occurs during* — Transition
- Documents — *are written to* — v1 index
- Documents — *are written to* — v2 index
- v2 index — *does not fall behind during* — Backfill
- Tenant's pointer — *is flipped to* — v2 index
- v1 index — *is* — deleted
- Serving layer — *needs* — Routing
- Routing — *is a* — first-class responsibility
- Routing — *directs* — Query
- Routing — *directs to* — Index version
- Routing — *enables smooth* — Blue/green transition
- Routing — *must exist before* — Migration
- Step 2 (of migration) — *is the* — expensive part
- Step 2 (of migration) — *involves* — Double computation
- Step 2 (of migration) — *involves* — Double writes
- Step 2 (of migration) — *involves* — Re-embedding
- Re-embedding — *applies to* — Existing document
- Mitigation — *reduces* — Migration cost
- Chunking output — *do not change with* — Embedding model change
- OCR text — *do not change with* — Embedding model change
- Extracted metadata — *do not change with* — Embedding model change
- Cache — *stores* — Chunking output
- Cache — *stores* — OCR text
- Cache — *stores* — Extracted metadata
- Cache — *is keyed by* — Content hash
- Embedding step — *is* — re-run
- Per-tenant indices — *enable* — incremental migration
- Tenant A — *runs on* — v1 index
- Tenant B — *runs on* — v2 index
- Per-tenant indices — *converts* — All-at-once cutover
- All-at-once cutover — *into* — Gradual rollout
- Gradual rollout — *allows comparison of* — Retrieval quality
- Index rebuild — *has* — large cost
- large cost — *includes* — Re-embedding
- Re-embedding — *applies to* — Document
- large cost — *includes* — Dual-write window
- Raw text — *should be kept* — durable
- Chunk boundaries — *should be kept* — durable
- Re-OCR — *increases* — rebuild expense
- Re-chunk — *increases* — rebuild expense
- Version the whole retrieval pipeline, not just the model — *is related to* — Changing the embedding model forces a full index rebuild
- Changing the embedding model forces a full index rebuild — *has source* — IR - System Design
- IR - System Design — *is tagged with* — AI
- IR - System Design — *is tagged with* — Confluence

%% ai-graph-end %%