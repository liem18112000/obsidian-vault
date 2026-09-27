---
ai_hash: cc3445b8cfb2496c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Cross-cutting pass
- Security scenarios
- I18n scenarios
- Concurrency scenarios
- LUZ-158390
- Cross-cutting generation pass
- Zip-slip
- Zip-bomb/high-compression
- Deeply-nested folders
- Symlink entries
- Duplicate entry names
- JSON-bomb
- CP437/UTF-8 decode
- NFC-vs-NFD
- Pool-size-identical
- Concurrent-same-folder
- No-lost-counters
- Guidance
- Score 0.62
- Score 0.70
- Completeness long tail
- 57 clean scenarios
- Triplication
- Coverage gain
- Judges critique
- Within-cell completeness
- Residual issues
- 2x2 valid/invalid x healthData matrix
- Size boundary
- 102,399
- 102,400
- 102,401
- Negative EP set
- companyId-wrong-type
- empty-title
- documentTypes-as-string
- Gap-1 token-mismatch outcome
- BLOCKED status
- Groundedness hit
- Residual NFC/NFD near-dups
- 0.86 cosine dedup
- Dedup
- Generation-completeness
- Boundary point enumeration
- Matrix cell filling
- Gap-1 cases blocking
- Tighter dedup
- Aggregate score
- Dup/groundedness penalties
- Judge
- 'Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union
  citations'
- '0.70 is an architecture ceiling not a tuning miss: guidance cannot inject cross-cutting
  kinds'
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
- Cross-cutting pass — *validated* — Security scenarios
- Cross-cutting pass — *validated* — I18n scenarios
- Cross-cutting pass — *validated* — Concurrency scenarios
- Cross-cutting generation pass — *is* — VALIDATED
- Cross-cutting generation pass — *is associated with* — LUZ-158390
- Cross-cutting generation pass — *generates* — Security scenarios
- Cross-cutting generation pass — *generates* — I18n scenarios
- Cross-cutting generation pass — *generates* — Concurrency scenarios
- Security scenarios — *include* — Zip-slip
- Security scenarios — *include* — Zip-bomb/high-compression
- Security scenarios — *include* — Deeply-nested folders
- Security scenarios — *include* — Symlink entries
- Security scenarios — *include* — Duplicate entry names
- Security scenarios — *include* — JSON-bomb
- I18n scenarios — *include* — CP437/UTF-8 decode
- I18n scenarios — *include* — NFC-vs-NFD
- Concurrency scenarios — *include* — Pool-size-identical
- Concurrency scenarios — *include* — Concurrent-same-folder
- Concurrency scenarios — *include* — No-lost-counters
- Guidance — *could not inject* — cross-cutting kinds
- suite — *contains* — 57 clean scenarios
- Score 0.62 — *is current* — score
- Score 0.70 — *is a target for* — Completeness long tail
- Coverage gain — *was offset by* — Judges critique
- Judges critique — *shifted to* — Within-cell completeness
- Judges critique — *shifted to* — Residual issues
- Residual issues — *include* — 2x2 valid/invalid x healthData matrix
- 2x2 valid/invalid x healthData matrix — *has* — 2/4 cells covered
- Residual issues — *include* — Size boundary
- Size boundary — *is at* — 102,399
- Size boundary — *missing* — 102,400
- Size boundary — *missing* — 102,401
- Residual issues — *include* — Negative EP set
- Negative EP set — *is* — half-covered
- Negative EP set — *missing* — companyId-wrong-type
- Negative EP set — *missing* — empty-title
- Negative EP set — *missing* — documentTypes-as-string
- Residual issues — *include* — Gap-1 token-mismatch outcome
- Gap-1 token-mismatch outcome — *asserted instead of marked as* — BLOCKED status
- Groundedness hit — *is caused by* — Gap-1 token-mismatch outcome
- Residual issues — *include* — Residual NFC/NFD near-dups
- Residual NFC/NFD near-dups — *not merged by* — 0.86 cosine dedup
- Dedup — *is an* — improvement
- Cross-cutting pass — *is an* — improvement
- Triplication — *is eliminated by* — Dedup
- Cross-cutting kinds — *are present due to* — Cross-cutting pass
- Score 0.70 — *is a* — LONG TAIL of Generation-completeness
- Generation-completeness — *requires* — Boundary point enumeration
- Generation-completeness — *requires* — Matrix cell filling
- Generation-completeness — *requires* — Gap-1 cases blocking
- Generation-completeness — *requires* — Tighter dedup
- Adding coverage — *can leave* — Aggregate score flat
- Additions — *introduce* — Dup/groundedness penalties
- Judge — *rebalances to* — next weakest dimension
- Cross-source semantic scenario dedup: embed+cosine-cluster, keep canonical, union citations — *is related to* — Dedup
- 0.70 is an architecture ceiling not a tuning miss: guidance cannot inject cross-cutting kinds — *is related to* — Score 0.70

%% ai-graph-end %%