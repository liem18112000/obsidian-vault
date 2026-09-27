---
ai_hash: fe8d92c8be63c082
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23
status: seedling
tags:
- testing-agent
- tpd
- dedup
- embeddings
- cosine
- quality
title: 'Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical,
  union citations'
type: howto
---

# Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations

FIX (cross-source scenario dedup, test-agent-v2): added `dedup_by_behaviour()` to implement/generate/scenarios.py and folded it into `refine_scenarios` (so both merge sites — llm.py sync path + workers.py distributed path — get it free). The generator emits scenarios PER source-node, so the same behaviour arrives 2-3x worded differently; the old dedup keyed on exact (kind, folded-title) and missed them. New pass: embed each scenario as `f"{kind}: {title}"` (batch, via the local Ollama embedder from `common.memory.pg.embed._get_embedder`), greedily cluster SAME-KIND scenarios whose cosine (reuse `common.memory.vector_memory._cosine`) >= TPD_DEDUP_THRESHOLD (default 0.86), keep ONE canonical per cluster, and UNION every duplicates source_refs + data_refs onto it so all supporting citations survive. GRACEFUL: embedder unconfigured/unreachable/error → returns input unchanged (offline + Vertex-less byte-identical; 52 offline tests pass incl. 2 new ones with a fake embedder). REUSE WINS (reuse-gate caught both): _cosine already existed in vector_memory.py; _get_embedder factory already in memory/pg/embed.py — do not re-implement. Deployed via thin-overlay rebuild (Dockerfile.patch: FROM image, COPY src, pip install --no-deps --force-reinstall .). This is the code lever the judges non-duplication ceiling needed — parallelism/guidance could not fix it. See [[Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance]].

## Related

- [[Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance]]

%% ai-graph-start %%

**Related notes:**
- [[Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance]]
- [[Dedup fix result 117 to 54 scenarios, 0.58 to 0.62, triplication gone; 0.70 now gated on coverage]]
- [[Dedup fix result 117→54 scenarios, 0.58→0.62, triplication gone; 0.70 now gated on coverage not dup]]
- [[0.70 is an architecture ceiling not a tuning miss guidance cannot inject cross-cutting kinds]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]

%% ai-graph-end %%