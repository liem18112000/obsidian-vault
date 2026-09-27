---
title: "Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk"
created: 2026-09-10
type: lesson
status: seedling
source: "session 2026-09-10 — docs-vector-search persona bug"
tags: [rag, retrieval, reranking, pgvector, gotcha]
---

# Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk

In a two-stage **retrieve → rerank** RAG pipeline, the first-stage candidate pool (`RETRIEVE_TOP_N` — the pgvector top-N handed to the cross-encoder reranker) must be wide enough that the correct chunk is *inside* it. The reranker can only reorder what it is given; it can never rescue a relevant chunk that first-stage vector search ranked at position 21–50.

**Symptom that points here:** the chatbot answers *"I don't know — that isn't in the documentation."* while the UI still shows a correct-looking source, and a near-identical query answers fine. That gap ("query A works, near-identical query B refuses") is a **retrieval-recall tell**, not a generation-prompt problem.

**Concrete case (LEO Customer 360 `docs-vector-search`: pgvector + `bge-reranker-base` + Qwen2.5-0.5B):** for short / low-signal queries — especially bare colloquial Vietnamese like *"persona là gì vậy?"* — the definitional chunk sat at vector-rank 21–50. With `RETRIEVE_TOP_N=20` it never reached the reranker; the 5 survivors all reranked around −10 (junk) so the tiny generator refused. Widening the pool to 50 made the reranker surface the right chunks at the top (rerank score +1.34) and the model answered — while out-of-scope refusal was preserved. Fix was **config-only** (`RETRIEVE_TOP_N` 20→50); the generator `top_k` (chunks it actually reads) was unchanged, so cost/memory barely moved.

**Diagnostic technique:** call `/search` with a *larger* `top_n` and watch where the correct chunk lands and what rerank score it gets. If it appears with a high score only once the pool is widened, the pool was the bottleneck.

Related: [[Over-refusal in a small RAG generator usually means bad retrieval, not a bad prompt]]

## Related

- [[Over-refusal in a small RAG generator usually means bad retrieval]]
- [[not a bad prompt]]
