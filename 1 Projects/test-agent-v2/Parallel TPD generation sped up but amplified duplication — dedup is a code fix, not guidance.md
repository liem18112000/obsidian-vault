---
ai_hash: 99df177d87338451
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Parallel TPD generation
- TPD generator
- Duplication
- Dedup
- Code fix
- Guidance
- Speed
- Quality
- Score
- SYSTEMIC TRIPLICATION
- Behaviour
- Scenario
- Naming scheme
- Ticket-scoped naming scheme
- Confluence-cited naming scheme
- Luzimport naming scheme
- golden-e2e
- zip-slip
- zip-bomb
- symlink
- Contradiction
- 158446-06
- luzimport-36
- job-counter assertion
- Root cause
- Generator
- Source node
- Merge/dedup process
- '`refine_scenarios` function'
- '`implement/generate/llm.py`'
- Parallel worker
- Semantic dedup
- FIX
- CODE
- Semantic key
- Canonical scenario
- Citation
- Throughput lever
- Quality lever
- Parallelism
- Producer
- LUZ-158390
- Finding
- Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY
- Parallelize TPD generation via 3 Redis-worker replicas
- _BATCH_CONCURRENCY
source: session 2026-09-23
status: seedling
tags:
- testing-agent
- tpd
- dedup
- quality
- parallelism
- luz-158390
title: Parallel TPD generation sped up but amplified duplication — dedup is a code
  fix, not guidance
type: project
---

# Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance

FINDING (LUZ-158390, round 2 after sonnet+3-worker parallelization): parallelizing the TPD generator improved SPEED (2 assured rounds completed in one call vs barely 1 before) but NOT quality — best score stayed 0.58 and round 2 scored 0.45 (worse). The judge diagnosed SYSTEMIC TRIPLICATION: nearly every behaviour is covered by 2-3 near-identical scenarios across THREE parallel naming schemes (ticket-scoped `158390-*/158436-*`, a confluence-cited set, and a `luzimport-01..47` set) — e.g. golden-e2e / zip-slip / zip-bomb / symlink each appear 3x — plus a direct contradiction (158446-06 asserts an outcome while luzimport-36 marks the same behaviour BLOCKED) and document scenarios missing the required job-counter assertion. ROOT CAUSE: the generator emits scenarios PER-SOURCE-NODE and the merge/dedup (`refine_scenarios(merged, valid_ids)` in implement/generate/llm.py) only removes same-id/near-dup WITHIN the merge, not the same behaviour arriving under different source citations. Parallel workers (each generating an independent batch, merged without cross-source semantic dedup) AMPLIFY it. FIX is CODE, not guidance/rounds: dedup by BEHAVIOUR (semantic key) keeping one canonical scenario per behaviour with all supporting sources in its citations — a cross-source semantic dedup pass at merge, or generate one canonical set and attach citations. LESSON: parallelism is a throughput lever, NOT a quality lever; if the merge doesnt dedup, more parallel producers = more duplication. See [[Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY]].

## Related

- [[Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY]]

%% ai-graph-start %%

**Related notes:**
- [[Dedup fix result 117 to 54 scenarios, 0.58 to 0.62, triplication gone; 0.70 now gated on coverage]]
- [[Dedup fix result 117→54 scenarios, 0.58→0.62, triplication gone; 0.70 now gated on coverage not dup]]
- [[0.70 is an architecture ceiling not a tuning miss guidance cannot inject cross-cutting kinds]]
- [[Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations]]
- [[TPD test_kinds must be additive over the base four, not replace them]]

**Relations:**
- Parallel TPD generation — *sped up* — TPD generator
- Parallel TPD generation — *amplified* — Duplication
- Dedup — *is a* — Code fix
- Dedup — *is not* — Guidance
- TPD generator — *parallelization improved* — Speed
- TPD generator — *parallelization did not improve* — Quality
- Speed — *enabled* — 2 assured rounds completed
- Quality — *score stayed at* — 0.58
- Round 2 — *scored* — 0.45
- Judge — *diagnosed* — SYSTEMIC TRIPLICATION
- SYSTEMIC TRIPLICATION — *describes* — Behaviour covered by multiple near-identical Scenario
- Scenario — *uses* — Naming scheme
- Naming scheme — *includes* — Ticket-scoped naming scheme
- Naming scheme — *includes* — Confluence-cited naming scheme
- Naming scheme — *includes* — Luzimport naming scheme
- golden-e2e — *is an example of* — Behaviour
- zip-slip — *is an example of* — Behaviour
- zip-bomb — *is an example of* — Behaviour
- symlink — *is an example of* — Behaviour
- Text — *mentions* — Contradiction
- 158446-06 — *asserts* — outcome
- luzimport-36 — *marks* — Behaviour BLOCKED
- Scenario — *missing* — job-counter assertion
- Root cause — *is* — Generator emits Scenario per Source node
- Root cause — *is* — Merge/dedup process only removes same-id/near-dup
- Merge/dedup process — *is implemented by* — `refine_scenarios` function
- `refine_scenarios` function — *located in* — `implement/generate/llm.py`
- Parallel worker — *amplifies* — Duplication
- FIX — *is* — CODE
- FIX — *is not* — Guidance
- FIX — *involves* — Semantic dedup by Behaviour
- Semantic dedup by Behaviour — *keeps* — Canonical scenario
- Canonical scenario — *includes* — Citation
- FIX — *involves* — Cross-source semantic dedup pass at merge
- FIX — *involves* — Generate one canonical set and attach Citation
- Parallelism — *is a* — Throughput lever
- Parallelism — *is not a* — Quality lever
- More Parallel producers — *leads to* — More Duplication
- LUZ-158390 — *is a* — Finding
- Parallel TPD generation — *is related to* — Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY
- Parallel TPD generation — *is related to* — Parallelize TPD generation via 3 Redis-worker replicas
- Parallel TPD generation — *is related to* — _BATCH_CONCURRENCY

%% ai-graph-end %%