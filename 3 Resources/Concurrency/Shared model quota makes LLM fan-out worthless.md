---
title: "Shared model quota makes LLM fan-out worthless"
created: 2026-09-22
type: lesson
status: seedling
source: "test-agent-v2 parallel-gather, 2026-09-22"
tags: [concurrency, parallelism, llm, throughput, test-agent]
---

# Shared model quota makes LLM fan-out worthless

Parallelizing N calls that all hit the **same model quota** buys ~nothing: the work is **throughput-bound**, so `asyncio.gather` over the calls just queues them against one rate limit and wall-clock stays ≈ serial. Fan-out only pays when the parallel tasks land on **distinct backends** (different I/O endpoints), where the bottleneck is latency, not a shared token/req budget — then wall-clock drops from Σ-of-hops to MAX-of-hops.

**The tell:** before parallelizing, ask "do these tasks compete for one saturated resource?" If yes (one LLM quota, one DB connection cap, one disk), parallelism is a null result. If they hit different resources, it is a real win.

**Measured evidence (test-agent-v2):** implements batch generation was pinned to `Semaphore(1)` partly because concurrent batches all shared one Vertex Claude quota → the validated concurrency>1 version bought **zero speedup** and was reverted; the real lever was the batch API. Conversely the KGA gather **seed producers** hit Atlassian REST / GCP / GCS+pgvector / web — distinct backends — so the *same* `asyncio.gather` shape gave a real Σ→MAX drop. Same structure, different bottleneck.

**Corollary:** do NOT parallelize the 3 KGA LLM planners (hypothesize/leads/cloud) — they share the one Vertex quota, so they inherit the null result; only the distinct-backend seed producers were parallelized.

Related: [[Cap-before-exclude parallelism recall trap]], [[Concurrent in-process ADK Runners return simultaneously-empty output]]

## Related

- [[Cap-before-exclude parallelism recall trap]]
- [[Concurrent in-process ADK Runners return simultaneously-empty output]]
