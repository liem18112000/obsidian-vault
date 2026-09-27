---
ai_hash: d20f2387e382cc15
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: test-agent-v2 implement, 2026-09-22
status: seedling
tags:
- adk
- concurrency
- gotcha
- llm
- test-agent
title: Concurrent in-process ADK Runners return simultaneously-empty output
type: lesson
---

# Concurrent in-process ADK Runners return simultaneously-empty output

Running Google ADK `output_schema` `LlmAgent`s **concurrently in one process** can make **all** of them return **empty structured output at once** — no exception, no timeout, just silent degradation to the callers heuristic fallback. Critically, this happened **even with isolation**: each call built its own throwaway `InMemorySessionService` + `Runner` (the `run_json_agent` pattern), yet concurrent runs still returned simultaneously-empty. So separate Runners are **not** sufficient to make concurrent in-process ADK generation safe.

**Why it matters:** the failure is invisible — the schema parse yields a default-constructed empty object, the caller logs "batch empty/invalid" and falls back, and you only notice via a tanked quality score (~0.1) in prod that offline fakes (which return clean JSON ADK parses fine) never reproduce.

**Consequence:** test_plan_definition implement is pinned to `Semaphore(1)` (`_BATCH_CONCURRENCY = 1`) for this reason. Prefer serial in-process ADK runs; for real generation throughput use an out-of-process worker pool or the providers batch API, not in-process concurrency.

Suspected root cause: concurrent in-process ADK Runners sharing some global/model-client state (never fully confirmed — "prime suspect" per the code comment).

Related: [[Shared model quota makes LLM fan-out worthless]]

## Related

- [[Shared model quota makes LLM fan-out worthless]]

%% ai-graph-start %%

**Related notes:**
- [[Shared model quota makes LLM fan-out worthless]]
- [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]]
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[Test an ADK LlmAgent(output_schema=) offline with a BaseLlm fake yielding canned JSON]]
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]

%% ai-graph-end %%