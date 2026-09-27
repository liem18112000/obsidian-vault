---
ai_hash: c001072c0afb6ffd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Cross-cutting generation pass
- Security scenarios
- i18n scenarios
- Concurrency scenarios
- Score 0.62
- Score 0.70
- Completeness long tail
- LUZ-158390
- Live subscription
- Guidance
- Scenario suite
- Triplication
- Coverage gain
- Judges critique
- WITHIN-CELL completeness
- Residual issues
- HealthData matrix
- Size boundary issues
- companyId-wrong-type
- empty-title
- documentTypes-as-string
- Negative EP set
- Gap-1 token-mismatch outcome
- BLOCKED status
- Groundedness hit
- NFC/NFD near-dups
- 0.86 cosine dedup
- Dedup process
- Generation-completeness
- Boundary point enumeration
- Matrix cell filling
- Gap-1 cases blocking
- Tighter dedup
- Aggregate score
- Dup penalties
- Groundedness penalties
- Judge
- Cross-source semantic scenario dedup
- 0.70 architecture ceiling note
source: session 2026-09-23
status: seedling
tags:
- testing-agent
- tpd
- crosscutting
- quality
- luz-158390
- validated
title: 'Cross-cutting pass validated: security/i18n/concurrency now generated; score
  0.62, 0.70 is a completeness long tail'
type: project
---

# Cross-cutting pass validated: security/i18n/concurrency now generated; score 0.62, 0.70 is a completeness long tail

VALIDATED (LUZ-158390, live subscription): the cross-cutting generation pass WORKS — the suite now contains the distinct security (zip-slip, zip-bomb/high-compression, deeply-nested folders, symlink entries, duplicate entry names, JSON-bomb), i18n (CP437/UTF-8 decode, NFC-vs-NFD), and concurrency (pool-size-identical, concurrent-same-folder, no-lost-counters) scenarios that guidance alone could NEVER inject before. 57 clean scenarios, no triplication. BUT the assured score held at 0.62 (did not cross 0.70): the coverage gain was OFFSET because the judges critique shifted from "kinds missing" to finer WITHIN-CELL completeness + a few residual issues: 2x2 valid/invalid × healthData matrix only 2/4 cells; size boundary only at 102,399 (missing 102,400/102,401); negative EP set half-covered (missing companyId-wrong-type, empty-title, documentTypes-as-string); one scenario asserts the gap-1 token-mismatch outcome instead of marking it BLOCKED (groundedness hit); ~3 residual NFC/NFD near-dups the 0.86 cosine dedup did not merge. NET: dedup + cross-cutting are both real improvements (triplication gone, cross-cutting kinds present); 0.70 is now a LONG TAIL of generation-completeness (enumerate each boundary point rather than one; fill every matrix cell; block gap-1 cases; slightly tighter dedup for near-identical i18n variants) — each a small targeted change, diminishing returns. LESSON: adding coverage can leave the aggregate score flat when the additions also introduce new dup/groundedness penalties — the judge rebalances to the next weakest dimension. See [[Cross-source semantic scenario dedup embed+cosine-cluster, keep canonical, union citations|Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations]] and [[0.70 is an architecture ceiling not a tuning miss guidance cannot inject cross-cutting kinds|0.70 is an architecture ceiling not a tuning miss: guidance cannot inject cross-cutting kinds]].

## Related

- [[0.70 is an architecture ceiling not a tuning miss guidance cannot inject cross-cutting kinds|0.70 is an architecture ceiling not a tuning miss: guidance cannot inject cross-cutting kinds]]

%% ai-graph-start %%

**Related notes:**
- [[0.70 is an architecture ceiling not a tuning miss guidance cannot inject cross-cutting kinds]]
- [[Dedup fix result 117→54 scenarios, 0.58→0.62, triplication gone; 0.70 now gated on coverage not dup]]
- [[Dedup fix result 117 to 54 scenarios, 0.58 to 0.62, triplication gone; 0.70 now gated on coverage]]
- [[testing-agent implement_plan generates scenarios per pack-node x 4 kinds, amplifying pack noise and ignoring non-functional-kind guidance]]
- [[Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance]]

**Relations:**
- Cross-cutting generation pass — *validated* — Security scenarios
- Cross-cutting generation pass — *validated* — i18n scenarios
- Cross-cutting generation pass — *validated* — Concurrency scenarios
- Cross-cutting generation pass — *generated* — Security scenarios
- Cross-cutting generation pass — *generated* — i18n scenarios
- Cross-cutting generation pass — *generated* — Concurrency scenarios
- Score 0.62 — *is current score* — Cross-cutting generation pass
- Score 0.70 — *is target score* — Cross-cutting generation pass
- Score 0.70 — *is a* — Completeness long tail
- LUZ-158390 — *is a* — Live subscription
- Cross-cutting generation pass — *WORKS* — 
- Cross-cutting generation pass — *injects* — Security scenarios
- Cross-cutting generation pass — *injects* — i18n scenarios
- Cross-cutting generation pass — *injects* — Concurrency scenarios
- Guidance — *could not inject* — Cross-cutting generation pass
- Scenario suite — *contains* — 57 clean scenarios
- Triplication — *is* — gone
- Score 0.62 — *did not cross* — Score 0.70
- Coverage gain — *offset by* — Judges critique
- Judges critique — *shifted to* — WITHIN-CELL completeness
- Judges critique — *shifted to* — Residual issues
- Residual issues — *include* — HealthData matrix
- Residual issues — *include* — Size boundary issues
- Size boundary issues — *missing values* — 102,400/102,401
- Residual issues — *include* — Negative EP set
- Negative EP set — *missing* — companyId-wrong-type
- Negative EP set — *missing* — empty-title
- Negative EP set — *missing* — documentTypes-as-string
- One scenario — *asserts* — Gap-1 token-mismatch outcome
- Gap-1 token-mismatch outcome — *should be* — BLOCKED status
- Asserting Gap-1 token-mismatch outcome — *causes* — Groundedness hit
- 0.86 cosine dedup — *did not merge* — NFC/NFD near-dups
- Dedup process — *is a* — real improvement
- Cross-cutting generation pass — *is a* — real improvement
- Score 0.70 — *is a* — Generation-completeness long tail
- Generation-completeness long tail — *requires* — Boundary point enumeration
- Generation-completeness long tail — *requires* — Matrix cell filling
- Generation-completeness long tail — *requires* — Gap-1 cases blocking
- Generation-completeness long tail — *requires* — Tighter dedup
- Adding coverage — *can leave* — Aggregate score flat
- Adding coverage — *introduces* — Dup penalties
- Adding coverage — *introduces* — Groundedness penalties
- Judge — *rebalances to* — next weakest dimension
- Cross-source semantic scenario dedup — *is related to* — Dedup process
- 0.70 architecture ceiling note — *is related to* — Score 0.70
- 0.70 architecture ceiling note — *is related to* — Guidance
- 0.70 architecture ceiling note — *is related to* — Cross-cutting generation pass

%% ai-graph-end %%