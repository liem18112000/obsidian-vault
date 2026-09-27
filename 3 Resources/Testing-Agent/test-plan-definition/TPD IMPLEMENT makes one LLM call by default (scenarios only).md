---
title: "TPD IMPLEMENT makes one LLM call by default (scenarios only)"
created: 2026-09-07
type: lesson
status: seedling
source: "session 2026-09-07 code trace"
tags: [testing-agent, tpd, vertex, llm, cloud-run, gotcha]
---

# TPD IMPLEMENT makes one LLM call by default (scenarios only)

In the TPD **IMPLEMENT** stage, the default path makes **exactly one LLM call — for scenarios — and only when Vertex is configured** (`VERTEX_PROJECT`+`VERTEX_LOCATION`+`VERTEX_MODEL`, checked by `vertex_config()`; `None` ⇒ heuristic).

- `generate_scenarios` calls `claude_scenarios` directly when `vertex_config()` is set (not gated by `detail`), falling back to `heuristic_scenarios` on empty/parse-miss.
- **test-data** and **steps** are heuristic unless `detail=True` **or** env `TPD_LLM_DETAIL` is set (`if detail or os.environ.get('TPD_LLM_DETAIL')` in `implement/testdata.py` and `implement/steps.py`). `detail` defaults to `False` in both the MCP tool and the executor parse.
- This one-call default was a **deliberate fix**: three serial blocking Vertex calls in the async handler once blocked the event loop past Cloud Run's liveness/request timeout. So every LLM call (`complete()`, thinking disabled) now runs **off the event loop via `asyncio.to_thread`** so `/livez` never starves.
- Same `vertex_config()` gating governs DEFINE's LLM seams: `claude_plan_questions` (question gen) and `claude_brief` (brief restatement). `TPD_LLM_DETAIL` does **not** affect DEFINE — only IMPLEMENT.

Heuristic scenario path expands a coverage matrix: happy × negative × boundary × error over ≤8 grounded notes ⇒ ≤32 scenarios (`'happy only'` metric ⇒ happy only).

Related: [[TPD agentic loop single-pass DEFINE to APPROVE to IMPLEMENT]]

## Related

- [[TPD agentic loop single-pass DEFINE to APPROVE to IMPLEMENT]]
