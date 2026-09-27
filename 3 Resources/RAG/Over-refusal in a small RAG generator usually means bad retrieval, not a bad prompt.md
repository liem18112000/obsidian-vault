---
ai_hash: 9fc274e223e34c7c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: session 2026-09-10 — docs-vector-search persona bug
status: seedling
tags:
- rag
- retrieval
- prompting
- gotcha
- llm
title: Over-refusal in a small RAG generator usually means bad retrieval, not a bad
  prompt
type: lesson
---

# Over-refusal in a small RAG generator usually means bad retrieval, not a bad prompt

When a small local RAG generator (e.g. Qwen2.5-0.5B) **over-refuses** — replying *"I don't know"* to in-scope questions whose answer is in the corpus — the instinct is to loosen the refusal instruction in the system prompt. Usually that is the wrong lever. Over-refusal in a weak generator is most often a **downstream symptom of poor retrieval**: the context it was handed did not actually contain the answer, so refusing was the *correct* behaviour given bad input.

**Order of operations:** fix retrieval first (does the definitional chunk actually reach the generator? — see [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]). Only if retrieval is verified good and the model *still* refuses should you touch the refusal prompt. Loosening the prompt while retrieval is broken trades one failure (false refusal) for a worse one — the model hallucinating an answer from junk context, and out-of-scope questions no longer being declined.

**Why the prompt is tempting but risky:** a firmer refusal prompt is often added deliberately to make out-of-scope questions decline reliably. Loosening it to cure false refusals can regress that hard-won behaviour. Retrieval fixes don't have that trade-off — they raise in-scope answer rate *and* leave genuine out-of-scope refusal intact (out-of-scope queries still retrieve only irrelevant chunks, so the model still declines).

Related: [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]

## Related

- [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]

%% ai-graph-start %%

**Related notes:**
- [[Widen the reranker candidate pool (RETRIEVE_TOP_N) or short queries retrieve junk]]
- [[LLM query enrichment for a substring-OR matcher must contract, not expand, the token set]]
- [[docs-vector-search OOMs on ask on a 1vCPU2GB box (Qwen KV cache over RAM+swap)]]
- [[Two-phase RAG chatbot UX fast retrieval first, slow generation second]]
- [[Local RAG stack for a 1 vCPU 2 GB box e5 + bge-reranker + Qwen 0.5B]]

%% ai-graph-end %%