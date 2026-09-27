---
ai_hash: 07713538c49921dd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Dedup fix
- 117 scenarios
- 54 scenarios
- 435 steps
- 81 steps
- 0.58 assured score
- 0.62 assured score
- triplication
- dedup
- code lever
- 0.70 score
- coverage
- 24% coverage
- 18% coverage
- AC×kind
- 0.86 cosine threshold
- distinct security scenarios
- zip-bomb
- symlink
- zip-slip
- TPD_DEDUP_THRESHOLD
- 0.90 threshold
- PLAIN re-run
- security/i18n scenarios
- duplication
- COVERAGE COMPLETENESS
- judge
- deep-nesting
- duplicate-ZIP-entry security
- CP437-vs-UTF-8 encoding
- NFC/NFD encoding
- pool-size-N concurrency comparison
- repeated runs
- size-boundary points
- non-atomic 5-in-1 field-failure scenario
- PATH TO 0.70
- judge coverage guidance
- semantic dedup
- recall
- precision
- threshold
- distinct-but-similar cases
- 'Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union
  citations'
- 'Cross-source semantic scenario dedup: embed+cosine-cluster'
- keep canonical
- union citations
- LUZ-158390
- run-3be0f1e0
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
- Dedup fix — *reduced_scenarios_from* — 117 scenarios
- Dedup fix — *reduced_scenarios_to* — 54 scenarios
- Dedup fix — *reduced_steps_from* — 435 steps
- Dedup fix — *reduced_steps_to* — 81 steps
- Dedup fix — *increased_assured_score_from* — 0.58 assured score
- Dedup fix — *increased_assured_score_to* — 0.62 assured score
- Dedup fix — *eliminated* — triplication
- Dedup fix — *is_a* — net win
- Dedup fix — *is_a* — correct code lever
- 0.70 score — *was_gated_on* — coverage
- 0.70 score — *is_now_gated_on* — COVERAGE COMPLETENESS
- coverage — *dipped_from* — 24% coverage
- coverage — *dipped_to* — 18% coverage
- 18% coverage — *measured_by* — AC×kind
- 0.86 cosine threshold — *is* — slightly aggressive
- 0.86 cosine threshold — *over_merged* — distinct security scenarios
- distinct security scenarios — *include* — zip-bomb
- distinct security scenarios — *include* — symlink
- zip-slip — *is_similar_to* — zip-bomb
- zip-slip — *is_similar_to* — symlink
- TPD_DEDUP_THRESHOLD — *should_be_bumped_toward* — 0.90 threshold
- 0.90 threshold — *preserves* — distinct security scenarios
- PLAIN re-run — *did_not_re_request* — security/i18n scenarios
- 0.70 score — *was_blocked_by* — duplication
- 0.70 score — *is_blocked_by* — COVERAGE COMPLETENESS
- judge — *wants* — zip bomb
- judge — *wants* — deep-nesting
- judge — *wants* — symlink
- judge — *wants* — duplicate-ZIP-entry security
- judge — *wants* — CP437-vs-UTF-8 encoding
- judge — *wants* — NFC/NFD encoding
- judge — *wants* — pool-size-N concurrency comparison
- judge — *wants* — repeated runs
- judge — *wants* — size-boundary points
- judge — *wants* — non-atomic 5-in-1 field-failure scenario
- PATH TO 0.70 — *requires* — raise threshold to ~0.90 threshold
- PATH TO 0.70 — *requires* — re-run WITH judge coverage guidance
- dedup — *prevents* — re-triplication
- semantic dedup — *trades* — recall
- semantic dedup — *trades_for* — precision
- threshold — *should_be_tuned* — threshold
- too low threshold — *over_merges* — distinct-but-similar cases
- Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations — *is_related_to* — Cross-source semantic scenario dedup: embed+cosine-cluster
- Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations — *is_related_to* — keep canonical
- Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations — *is_related_to* — union citations
- Dedup fix — *identified_by_ticket* — LUZ-158390
- Dedup fix — *identified_by_run_id* — run-3be0f1e0
- semantic dedup — *is_a_type_of* — dedup

%% ai-graph-end %%