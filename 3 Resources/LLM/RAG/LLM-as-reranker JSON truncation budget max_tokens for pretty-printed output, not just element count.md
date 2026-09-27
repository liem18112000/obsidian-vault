---
title: "LLM-as-reranker JSON truncation: budget max_tokens for pretty-printed output, not just element count"
created: 2026-09-13
type: lesson
status: seedling
source: "session 2026-09-13"
tags: [llm, rag, reranker, openai, json, gotcha]
---

# LLM-as-reranker JSON truncation: budget max_tokens for pretty-printed output, not just element count

When you use an LLM as a reranker (or any "return a JSON array of N numbers" call), sizing the output budget as `max_tokens = N * small_k` (e.g. N*6) silently breaks at scale: the model **pretty-prints** the array (`100,\n    100,\n` = number + comma + newline + indent ≈ ~6 tokens *per element just for formatting*, before the digits and the `{"scores":[ ]}` wrapper). So the budget is exhausted, the response hits `finish_reason=length`, the JSON is cut off mid-array, `json.loads` fails, and — if the code has a fallback — it degrades **silently**.

**Real case (leo-customer360 docs-vector-search, UAT):** `gpt-4o-mini` reranker, `max_tokens=max(128, n*6)`, `RETRIEVE_TOP_N=50` → 50 candidates → `max_tokens=300`, truncated on essentially every query → fell back to `DOCS_RERANK_OPENAI_FALLBACK=vector` which returns `[0.0]*n`, so every hit scored 0.0 and rerank was effectively OFF. A 2-candidate test returned clean `{"scores":[90,0]}` (finish_reason=stop) — the failure is **input-size dependent**, which is the tell.

**Fixes / rules of thumb:**
- Budget for formatted output, not element count: `n*12 + wrapper`, or force compact JSON.
- Enforce shape with `response_format={"type":"json_object"}` (or a json_schema) + "no whitespace" — smaller and more reliable than free-form.
- Cap the pool the LLM ranker sees (rerank top ~20, not all 50) to bound tokens AND latency.
- A `finish_reason=length` on a "return JSON" call is the smoking gun for truncation.
- Never let a ranker fallback be invisible — surface "fallback active" in health/metrics, else it silently regresses relevance.

## Related
[[docs-search UAT latency root cause: unapplied 8001 secgroup ingress (api to docs box)]]

## Related

- [[docs-search UAT latency root cause: unapplied 8001 secgroup ingress (api to docs box)]]
