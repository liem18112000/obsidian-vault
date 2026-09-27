---
title: "Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance"
created: 2026-09-23
type: project
status: seedling
source: "session 2026-09-23"
tags: [testing-agent, tpd, dedup, quality, parallelism, luz-158390]
---

# Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance

FINDING (LUZ-158390, round 2 after sonnet+3-worker parallelization): parallelizing the TPD generator improved SPEED (2 assured rounds completed in one call vs barely 1 before) but NOT quality — best score stayed 0.58 and round 2 scored 0.45 (worse). The judge diagnosed SYSTEMIC TRIPLICATION: nearly every behaviour is covered by 2-3 near-identical scenarios across THREE parallel naming schemes (ticket-scoped `158390-*/158436-*`, a confluence-cited set, and a `luzimport-01..47` set) — e.g. golden-e2e / zip-slip / zip-bomb / symlink each appear 3x — plus a direct contradiction (158446-06 asserts an outcome while luzimport-36 marks the same behaviour BLOCKED) and document scenarios missing the required job-counter assertion. ROOT CAUSE: the generator emits scenarios PER-SOURCE-NODE and the merge/dedup (`refine_scenarios(merged, valid_ids)` in implement/generate/llm.py) only removes same-id/near-dup WITHIN the merge, not the same behaviour arriving under different source citations. Parallel workers (each generating an independent batch, merged without cross-source semantic dedup) AMPLIFY it. FIX is CODE, not guidance/rounds: dedup by BEHAVIOUR (semantic key) keeping one canonical scenario per behaviour with all supporting sources in its citations — a cross-source semantic dedup pass at merge, or generate one canonical set and attach citations. LESSON: parallelism is a throughput lever, NOT a quality lever; if the merge doesnt dedup, more parallel producers = more duplication. See [[Parallelize TPD generation via 3 Redis-worker replicas, not _BATCH_CONCURRENCY]].

## Related

- [[Parallelize TPD generation via 3 Redis-worker replicas]]
- [[not _BATCH_CONCURRENCY]]
