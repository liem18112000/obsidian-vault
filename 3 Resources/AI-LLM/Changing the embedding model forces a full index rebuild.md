---
title: "Changing the embedding model forces a full index rebuild"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: IR - System Design (AI)"
tags: [embeddings, vector-search, migration, blue-green, rag, confluence-distilled]
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
