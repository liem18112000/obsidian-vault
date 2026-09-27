---
ai_hash: 9037ff835560a5ce
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- cross-source semantic dedup fix
- scenarios
- steps
- assured score
- systemic triplication across 3 naming schemes
- LUZ-158390
- run-3be0f1e0
- semantic dedup
- code lever
- coverage
- 0.86 cosine threshold
- security scenarios
- zip-bomb
- symlink
- zip-slip
- TPD_DEDUP_THRESHOLD
- 0.90 threshold
- PLAIN re-run
- i18n scenarios
- 0.70 target score
- duplication
- COVERAGE COMPLETENESS
- judge
- deep-nesting
- duplicate-ZIP-entry security
- CP437-vs-UTF-8 encoding
- NFC/NFD encoding
- pool-size-N concurrency comparison
- repeated runs
- 102,399/102,401 size-boundary points
- field-failure scenario
- judge coverage guidance
- re-triplication
- recall
- precision
- distinct-but-similar cases
- 'Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union
  citations'
- 'Cross-source semantic scenario dedup: embed+cosine-cluster'
- keep canonical
- union citations
source: session 2026-09-23
status: seedling
tags:
- testing-agent
- tpd
- dedup
- quality
- luz-158390
- result
title: Dedup fix result 117 to 54 scenarios, 0.58 to 0.62, triplication gone; 0.70
  now gated on coverage
type: project
---

# Dedup fix result 117 to 54 scenarios, 0.58 to 0.62, triplication gone; 0.70 now gated on coverage

RESULT of the cross-source semantic dedup fix (LUZ-158390, run-3be0f1e0): WORKED. 117->54 scenarios, 435->81 steps, assured score 0.58->0.62, and the judge "systemic triplication across 3 naming schemes" complaint is GONE. So the dedup is a net win + the correct code lever. TWO caveats: (1) coverage dipped 24%->18% AC×kind — the 0.86 cosine threshold is slightly aggressive and over-merged some DISTINCT security scenarios (zip-bomb/symlink read as near-zip-slip); bump TPD_DEDUP_THRESHOLD toward 0.90 to preserve them. (2) it was a PLAIN re-run (no guidance) so it did not re-request the security/i18n scenarios. WHAT NOW BLOCKS 0.70 changed from duplication to COVERAGE COMPLETENESS — judge wants: zip bomb/deep-nesting/symlink/duplicate-ZIP-entry security, CP437-vs-UTF-8 + NFC/NFD encoding, pool-size-N concurrency comparison + repeated runs, and the 102,399/102,401 size-boundary points, plus splitting one non-atomic 5-in-1 field-failure scenario. PATH TO 0.70: raise threshold to ~0.90 AND re-run WITH the judge coverage guidance (dedup now prevents re-triplication). LESSON: semantic dedup trades a little recall for precision — tune the threshold; too low over-merges distinct-but-similar cases. See [[Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations|Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations]].

## Related

- [[Cross-source semantic scenario dedup: embed+cosine-cluster]]
- [[keep canonical]]
- [[union citations]]

%% ai-graph-start %%

**Related notes:**
- [[Dedup fix result 117→54 scenarios, 0.58→0.62, triplication gone; 0.70 now gated on coverage not dup]]
- [[0.70 is an architecture ceiling not a tuning miss guidance cannot inject cross-cutting kinds]]
- [[Cross-cutting pass validated securityi18nconcurrency now generated; score 0.62, 0.70 is a completeness long tail]]
- [[Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance]]
- [[Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations]]

**Relations:**
- cross-source semantic dedup fix — *reduced scenarios from 117 to 54* — scenarios
- cross-source semantic dedup fix — *reduced steps from 435 to 81* — steps
- cross-source semantic dedup fix — *improved assured score from 0.58 to 0.62* — assured score
- cross-source semantic dedup fix — *resolved* — systemic triplication across 3 naming schemes
- cross-source semantic dedup fix — *is identified by* — LUZ-158390
- cross-source semantic dedup fix — *is identified by* — run-3be0f1e0
- semantic dedup — *is a* — net win
- semantic dedup — *is the* — correct code lever
- coverage — *dipped from 24% to 18%* — coverage
- 0.86 cosine threshold — *is* — aggressive
- 0.86 cosine threshold — *over-merged* — security scenarios
- security scenarios — *include* — zip-bomb
- security scenarios — *include* — symlink
- zip-bomb — *was read as* — near-zip-slip
- symlink — *was read as* — near-zip-slip
- TPD_DEDUP_THRESHOLD — *should be bumped toward* — 0.90 threshold
- 0.90 threshold — *would preserve* — security scenarios
- PLAIN re-run — *did not re-request* — security scenarios
- PLAIN re-run — *did not re-request* — i18n scenarios
- duplication — *previously blocked* — 0.70 target score
- COVERAGE COMPLETENESS — *now blocks* — 0.70 target score
- judge — *wants* — zip bomb
- judge — *wants* — deep-nesting
- judge — *wants* — symlink
- judge — *wants* — duplicate-ZIP-entry security
- judge — *wants* — CP437-vs-UTF-8 encoding
- judge — *wants* — NFC/NFD encoding
- judge — *wants* — pool-size-N concurrency comparison
- judge — *wants* — repeated runs
- judge — *wants* — 102,399/102,401 size-boundary points
- judge — *wants* — splitting field-failure scenario
- field-failure scenario — *is* — non-atomic
- field-failure scenario — *is* — 5-in-1
- TPD_DEDUP_THRESHOLD — *raised to 0.90 is a path to* — 0.70 target score
- judge coverage guidance — *used in re-run is a path to* — 0.70 target score
- semantic dedup — *prevents* — re-triplication
- semantic dedup — *trades* — recall for precision
- TPD_DEDUP_THRESHOLD — *should be* — tuned
- too low TPD_DEDUP_THRESHOLD — *over-merges* — distinct-but-similar cases
- Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations — *describes* — semantic dedup
- Cross-source semantic scenario dedup: embed+cosine-cluster — *is related to* — Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations
- keep canonical — *is related to* — Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations
- union citations — *is related to* — Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations

%% ai-graph-end %%