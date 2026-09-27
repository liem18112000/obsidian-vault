---
ai_hash: 54e4000d441078d8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Dedup fix
- scenarios
- steps
- assured score
- triplication
- code lever
- coverage
- cosine threshold
- security scenarios
- zip-bomb
- symlink
- zip-slip
- TPD_DEDUP_THRESHOLD
- PLAIN re-run
- guidance
- security/i18n scenarios
- COVERAGE COMPLETENESS
- judge
- deep-nesting
- duplicate-ZIP-entry security
- CP437-vs-UTF-8 encoding
- NFC/NFD encoding
- pool-size-N concurrency comparison
- repeated runs
- size-boundary points
- field-failure scenario
- 0.70 score
- semantic dedup
- recall
- precision
- threshold
- Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union
  citations
- LUZ-158390
- run-3be0f1e0
- 'Cross-source semantic scenario dedup: embed+cosine-cluster'
- keep canonical
- union citations
- duplication
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

RESULT of the cross-source semantic dedup fix (LUZ-158390, run-3be0f1e0): WORKED. 117→54 scenarios, 435→81 steps, assured score 0.58→0.62, and the judges "systemic triplication across 3 naming schemes" complaint is GONE. So the dedup is a net win + the correct code lever. TWO caveats: (1) coverage dipped 24%→18% AC×kind — the 0.86 cosine threshold is slightly aggressive and over-merged some DISTINCT security scenarios (zip-bomb/symlink read as near-zip-slip); bump TPD_DEDUP_THRESHOLD toward 0.90 to preserve them. (2) it was a PLAIN re-run (no guidance) so it did not re-request the security/i18n scenarios. WHAT NOW BLOCKS 0.70 changed from duplication to COVERAGE COMPLETENESS — judge wants: zip bomb/deep-nesting/symlink/duplicate-ZIP-entry security, CP437-vs-UTF-8 + NFC/NFD encoding, pool-size-N concurrency comparison + repeated runs, and the 102,399/102,401 size-boundary points, plus splitting one non-atomic 5-in-1 field-failure scenario. PATH TO 0.70: raise threshold to ~0.90 AND re-run WITH the judges coverage guidance (dedup now prevents that guidance from re-triplicating). LESSON: semantic dedup trades a little recall for precision — tune the threshold; too low over-merges distinct-but-similar cases. See [[Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations|Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations]].

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
- Dedup fix — *resulted in* — scenarios
- scenarios — *changed from* — 117
- scenarios — *changed to* — 54
- Dedup fix — *resulted in* — steps
- steps — *changed from* — 435
- steps — *changed to* — 81
- Dedup fix — *improved* — assured score
- assured score — *changed from* — 0.58
- assured score — *changed to* — 0.62
- Dedup fix — *eliminated* — triplication
- Dedup fix — *is a* — code lever
- Dedup fix — *is identified by* — LUZ-158390
- Dedup fix — *is identified by* — run-3be0f1e0
- coverage — *dipped due to* — Dedup fix
- coverage — *changed from* — 24%
- coverage — *changed to* — 18%
- cosine threshold — *has value* — 0.86
- cosine threshold — *is* — aggressive
- cosine threshold — *over-merged* — security scenarios
- security scenarios — *include* — zip-bomb
- security scenarios — *include* — symlink
- zip-bomb — *is distinct from* — zip-slip
- symlink — *is distinct from* — zip-slip
- TPD_DEDUP_THRESHOLD — *should be adjusted to* — 0.90
- PLAIN re-run — *did not re-request* — security/i18n scenarios
- 0.70 score — *was blocked by* — duplication
- 0.70 score — *is now blocked by* — COVERAGE COMPLETENESS
- judge — *wants* — zip-bomb
- judge — *wants* — deep-nesting
- judge — *wants* — symlink
- judge — *wants* — duplicate-ZIP-entry security
- judge — *wants* — CP437-vs-UTF-8 encoding
- judge — *wants* — NFC/NFD encoding
- judge — *wants* — pool-size-N concurrency comparison
- judge — *wants* — repeated runs
- judge — *wants* — size-boundary points
- judge — *wants* — field-failure scenario
- 0.70 score — *path involves* — threshold
- threshold — *should be raised to* — 0.90
- 0.70 score — *path involves* — guidance
- semantic dedup — *trades* — recall
- semantic dedup — *trades for* — precision
- semantic dedup — *is related to* — Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations
- Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations — *is related to* — Cross-source semantic scenario dedup: embed+cosine-cluster
- Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations — *includes* — keep canonical
- Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations — *includes* — union citations

%% ai-graph-end %%