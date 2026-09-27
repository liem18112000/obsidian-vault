---
ai_hash: 5019e1eab166de0c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- TPD generation
- Parallelization
- Duplication
- Dedup
- Code fix
- Guidance
- Speed
- Quality
- Luz-158390
- SYSTEMIC TRIPLICATION
- Scenarios
- Naming schemes
- Ticket-scoped naming scheme
- Confluence-cited set
- Luzimport-01..47 set
- Direct contradiction
- 158446-06
- Luzimport-36
- Job-counter assertion
- Generator
- Merge/dedup process
- refine_scenarios(merged, valid_ids)
- implement/generate/llm.py
- Near-duplicate scenarios
- Cross-source semantic dedup
- Parallel workers
- Behaviour
- Canonical scenario
- Citations
- Parallelism
- Throughput lever
- Quality lever
- Parallel producers
- sonnet+3-worker parallelization
- Redis-worker replicas
- _BATCH_CONCURRENCY
- Independent batch
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
- TPD generation — *sped up by* — Parallelization
- Parallelization — *amplified* — Duplication
- Dedup — *is a* — Code fix
- Dedup — *is not* — Guidance
- Parallelization — *improved* — Speed
- Parallelization — *did not improve* — Quality
- Luz-158390 — *is a* — FINDING
- Luz-158390 — *diagnosed* — SYSTEMIC TRIPLICATION
- SYSTEMIC TRIPLICATION — *is a form of* — Duplication
- SYSTEMIC TRIPLICATION — *affects* — Scenarios
- Scenarios — *are covered by* — Naming schemes
- Naming schemes — *include* — Ticket-scoped naming scheme
- Naming schemes — *include* — Confluence-cited set
- Naming schemes — *include* — Luzimport-01..47 set
- Scenarios — *contain* — Direct contradiction
- 158446-06 — *asserts* — outcome
- Luzimport-36 — *marks* — behaviour BLOCKED
- Scenarios — *missing* — Job-counter assertion
- Generator — *emits* — Scenarios
- Scenarios — *are generated* — PER-SOURCE-NODE
- Merge/dedup process — *is* — refine_scenarios(merged, valid_ids)
- refine_scenarios(merged, valid_ids) — *is in* — implement/generate/llm.py
- Merge/dedup process — *removes* — Near-duplicate scenarios
- Merge/dedup process — *does not remove* — Cross-source semantic dedup
- Parallel workers — *generate* — Independent batch
- Independent batch — *merged without* — Cross-source semantic dedup
- Parallel workers — *amplify* — Duplication
- Code fix — *is* — Dedup by Behaviour
- Dedup by Behaviour — *keeps* — Canonical scenario
- Canonical scenario — *includes* — Citations
- Dedup by Behaviour — *is a type of* — Cross-source semantic dedup
- Cross-source semantic dedup — *occurs at* — merge
- Parallelism — *is a* — Throughput lever
- Parallelism — *is not a* — Quality lever
- Parallel producers — *increase* — Duplication
- sonnet+3-worker parallelization — *is a type of* — Parallelization
- Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY — *is related to* — TPD generation
- Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY — *uses* — Redis-worker replicas
- Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY — *avoids* — _BATCH_CONCURRENCY

%% ai-graph-end %%