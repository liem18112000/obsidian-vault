---
ai_hash: d285798ec972b42c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Dedup fix result
- scenarios
- steps
- assured score
- triplication
- coverage
- cross-source semantic dedup fix
- LUZ-158390
- run-3be0f1e0
- cosine threshold
- TPD_DEDUP_THRESHOLD
- distinct security scenarios
- zip-bomb
- symlink read
- zip-slip
- PLAIN re-run
- guidance
- security/i18n scenarios
- 0.70 score
- COVERAGE COMPLETENESS
- zip bomb security
- deep-nesting security
- symlink security
- duplicate-ZIP-entry security
- CP437-vs-UTF-8 encoding
- NFC/NFD encoding
- pool-size-N concurrency comparison
- repeated runs
- size-boundary points
- field-failure scenario
- semantic dedup
- recall
- precision
- 'Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union
  citations'
- 'Cross-source semantic scenario dedup: embed+cosine-cluster'
- keep canonical
- union citations
- threshold
- distinct-but-similar cases
- judges coverage guidance
source: session 2026-09-23
status: seedling
tags:
- testing-agent
- tpd
- dedup
- quality
- luz-158390
- result
title: 'Dedup fix result: 117→54 scenarios, 0.58→0.62, triplication gone; 0.70 now
  gated on coverage not dup'
type: project
---

# Dedup fix result: 117→54 scenarios, 0.58→0.62, triplication gone; 0.70 now gated on coverage not dup

RESULT of the cross-source semantic dedup fix (LUZ-158390, run-3be0f1e0): WORKED. 117→54 scenarios, 435→81 steps, assured score 0.58→0.62, and the judges "systemic triplication across 3 naming schemes" complaint is GONE. So the dedup is a net win + the correct code lever. TWO caveats: (1) coverage dipped 24%→18% AC×kind — the 0.86 cosine threshold is slightly aggressive and over-merged some DISTINCT security scenarios (zip-bomb/symlink read as near-zip-slip); bump TPD_DEDUP_THRESHOLD toward 0.90 to preserve them. (2) it was a PLAIN re-run (no guidance) so it did not re-request the security/i18n scenarios. WHAT NOW BLOCKS 0.70 changed from duplication to COVERAGE COMPLETENESS — judge wants: zip bomb/deep-nesting/symlink/duplicate-ZIP-entry security, CP437-vs-UTF-8 + NFC/NFD encoding, pool-size-N concurrency comparison + repeated runs, and the 102,399/102,401 size-boundary points, plus splitting one non-atomic 5-in-1 field-failure scenario. PATH TO 0.70: raise threshold to ~0.90 AND re-run WITH the judges coverage guidance (dedup now prevents that guidance from re-triplicating). LESSON: semantic dedup trades a little recall for precision — tune the threshold; too low over-merges distinct-but-similar cases. See [[Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations]].

## Related

- [[Cross-source semantic scenario dedup: embed+cosine-cluster]]
- [[keep canonical]]
- [[union citations]]

%% ai-graph-start %%

**Related notes:**
- [[Dedup fix result 117 to 54 scenarios, 0.58 to 0.62, triplication gone; 0.70 now gated on coverage]]
- [[0.70 is an architecture ceiling not a tuning miss guidance cannot inject cross-cutting kinds]]
- [[Cross-cutting pass validated securityi18nconcurrency now generated; score 0.62, 0.70 is a completeness long tail]]
- [[Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance]]
- [[Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations]]

**Relations:**
- Dedup fix result — *is result of* — cross-source semantic dedup fix
- cross-source semantic dedup fix — *has ID* — LUZ-158390
- cross-source semantic dedup fix — *has run ID* — run-3be0f1e0
- Dedup fix result — *reduced scenarios from 117 to* — 54
- Dedup fix result — *reduced steps from 435 to* — 81
- Dedup fix result — *increased assured score from 0.58 to* — 0.62
- Dedup fix result — *eliminated* — triplication
- Dedup fix result — *is a* — net win
- Dedup fix result — *is a* — correct code lever
- coverage — *dipped from 24% to* — 18%
- cosine threshold — *is* — 0.86
- 0.86 — *is* — aggressive
- 0.86 — *over-merged* — distinct security scenarios
- distinct security scenarios — *include* — zip-bomb
- distinct security scenarios — *include* — symlink read
- distinct security scenarios — *include* — zip-slip
- zip-bomb — *read as near* — zip-slip
- symlink read — *read as near* — zip-slip
- TPD_DEDUP_THRESHOLD — *should be bumped toward* — 0.90
- TPD_DEDUP_THRESHOLD — *preserves* — distinct security scenarios
- PLAIN re-run — *did not re-request* — security/i18n scenarios
- 0.70 score — *is now gated on* — COVERAGE COMPLETENESS
- COVERAGE COMPLETENESS — *requires* — zip bomb security
- COVERAGE COMPLETENESS — *requires* — deep-nesting security
- COVERAGE COMPLETENESS — *requires* — symlink security
- COVERAGE COMPLETENESS — *requires* — duplicate-ZIP-entry security
- COVERAGE COMPLETENESS — *requires* — CP437-vs-UTF-8 encoding
- COVERAGE COMPLETENESS — *requires* — NFC/NFD encoding
- COVERAGE COMPLETENESS — *requires* — pool-size-N concurrency comparison
- COVERAGE COMPLETENESS — *requires* — repeated runs
- COVERAGE COMPLETENESS — *requires* — size-boundary points
- COVERAGE COMPLETENESS — *requires* — splitting one non-atomic 5-in-1 field-failure scenario
- 0.70 score — *requires raising* — threshold to ~0.90
- 0.70 score — *requires* — re-run WITH judges coverage guidance
- semantic dedup — *prevents* — judges coverage guidance from re-triplicating
- semantic dedup — *trades* — recall
- semantic dedup — *trades for* — precision
- semantic dedup — *requires* — tune the threshold
- threshold — *when too low* — over-merges distinct-but-similar cases
- Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations — *is related to* — semantic dedup
- Cross-source semantic scenario dedup: embed+cosine-cluster — *is related to* — semantic dedup
- keep canonical — *is related to* — semantic dedup
- union citations — *is related to* — semantic dedup

%% ai-graph-end %%