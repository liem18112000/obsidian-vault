---
ai_hash: 066b655b8ab7b303
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities: []
source: session 2026-09-06 docs-chatbot frontend-admin
status: seedling
tags:
- rag
- llm
- ux
- frontend
- chatbot
title: 'Two-phase RAG chatbot UX: fast retrieval first, slow generation second'
type: lesson
---

# Two-phase RAG chatbot UX: fast retrieval first, slow generation second

A RAG chatbot backed by a **slow local LLM** (small GGUF model on 1 vCPU: multi-second answers, no streaming) feels broken if the UI just spins until the generated answer arrives. Split the interaction into two calls the backend already exposes:

1. **`/search`** (semantic retrieve + rerank, **no generation**) returns in sub-second -> render the matched **source docs immediately** as clickable citations. The user gets value and something to read right away.
2. **`/ask`** (full RAG generation) runs in the background -> when it returns, drop in the grounded **answer** and reconcile the (authoritative) sources.

Show an explicit status transition: "Searching the docs…" -> "Generating answer…". If phase 1 fails it is **non-fatal** -- still attempt phase 2, which returns its own sources. Only an **abort** (a newer question superseding this one) should cancel the whole flow.

This is a general pattern for any two-tier backend where a cheap query and an expensive synthesis are separately callable: paint the cheap result first, then upgrade in place. Pair it with single-inflight cancellation (AbortController) so rapid re-asks don't stack. Relates to [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]].

## Related

- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]

%% ai-graph-start %%

**Related notes:**
- [[Public unauthenticated CPU-bound LLM endpoint is a DoS foot-gun]]
- [[docs-vector-search OOMs on ask on a 1vCPU2GB box (Qwen KV cache over RAM+swap)]]
- [[Honest progress UI for un-streamable long LLM runs - elapsed time plus stage hints]]
- [[Static vs server-backed host decides proxy-vs-direct-CORS for browser widgets]]
- [[Client-side generation queue lets independent items run without blocking each other]]

%% ai-graph-end %%