---
ai_hash: c0ed847c98799ba5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-07
entities:
- Materialize code review report
- sprint-156
- luz_docs materialize package
- kepler/sprint-156/earchive-master
- master
- C:/Users/dvtliem/Kepler/luz_docs/docs/materialize-code-review.md
- 15 ranked findings
- 9 below-cap
- cleanup pile
- 3 refuted candidates
- folder security-class changes
- PATCH path
- updateSecurityClasses endpoint
- materialize parent-change cascade
- PUT
- _isPublic
- _effectiveSecurityClassCodes
- materialized read path
- sentinel fields
- API responses
- single-doc snapshot
- Mongo's 16MB cap
- SC_MULTI_STATUS
- retryable
- rollback
- migration executor
- infinite loop
- single-thread migration pipeline
- DistributionCacheException.isNotFound
- this
- self.getSnapshot
- copy rename path
- projection keys
- missing-4th-sentinel exposure
- empty-codes divergence
- ForkJoinPool thread-safety
- luz_docs folder security-class changes have 3 entry points but only PUT cascades
- DistributionCacheException.isNotFound is inverted in luz_docs
- Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore
  SC_MULTI_STATUS as benign
- CDI self-invocation bypasses interceptor proxy
- jsonstore projections need quoted JSON keys and Mongo 16MB doc limit caps single-doc
  snapshots
- luz_docs
- CDI self-invocation
- interceptor proxy
- jsonstore
- Mongo
source: materialize code review 2026-06-07
status: seedling
tags:
- luz-docs
- materialize
- code-review
- earchive
- sprint-156
title: Materialize code review report - sprint-156 findings index
type: observation
---

# Materialize code review report - sprint-156 findings index

Full max-effort review of the luz_docs `materialize` package (branch `kepler/sprint-156/earchive-master` vs master) lives at `C:/Users/dvtliem/Kepler/luz_docs/docs/materialize-code-review.md` — 15 ranked findings + 9 below-cap + cleanup pile + 3 refuted candidates.

Top severity (security): folder security-class changes via the PATCH path and the `updateSecurityClasses` endpoint never trigger the materialize parent-change cascade (only PUT does) → stale `_isPublic`/`_effectiveSecurityClassCodes` → wrong access on the materialized read path. Also: sentinel fields leak into API responses; single-doc snapshot breaks at Mongo's 16MB cap; SC_MULTI_STATUS wrapped as retryable → rollback reverts correct docs; migration executor infinite loop wedges the single-thread migration pipeline.

Quick wins: `!=`→`==` in DistributionCacheException.isNotFound; `this`→`self.getSnapshot`; copy rename path's SC_MULTI_STATUS catch; quote projection keys.

Check the report before re-reviewing this package — refuted candidates listed there (missing-4th-sentinel exposure, empty-codes divergence, ForkJoinPool thread-safety) should not be re-raised.

## Related

- [[luz_docs folder security-class changes have 3 entry points but only PUT cascades]]
- [[DistributionCacheException.isNotFound is inverted in luz_docs]]
- [[Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore SC_MULTI_STATUS as benign]]
- [[CDI self-invocation bypasses interceptor proxy]]
- [[jsonstore projections need quoted JSON keys and Mongo 16MB doc limit caps single-doc snapshots]]

%% ai-graph-start %%

**Related notes:**
- [[Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore SC_MULTI_STATUS as benign]]
- [[luz_docs parent-change cascade tightened with setEquals slot-differs expr to make 207 diagnostic]]
- [[securityClassCodes scalar string breaks materialize sentinels]]
- [[Materialize bulk PATCH fans out into N serial per-doc PATCH calls]]
- [[Materialize appendAsPatchOps uses RFC-6902 replace for sentinel fields]]

**Relations:**
- Materialize code review report — *covers* — sprint-156
- Materialize code review report — *reviews* — luz_docs materialize package
- luz_docs materialize package — *branch* — kepler/sprint-156/earchive-master
- kepler/sprint-156/earchive-master — *compared to* — master
- Materialize code review report — *located at* — C:/Users/dvtliem/Kepler/luz_docs/docs/materialize-code-review.md
- Materialize code review report — *includes* — 15 ranked findings
- Materialize code review report — *includes* — 9 below-cap
- Materialize code review report — *includes* — cleanup pile
- Materialize code review report — *includes* — 3 refuted candidates
- folder security-class changes — *via* — PATCH path
- folder security-class changes — *via* — updateSecurityClasses endpoint
- updateSecurityClasses endpoint — *does not trigger* — materialize parent-change cascade
- PUT — *triggers* — materialize parent-change cascade
- materialize parent-change cascade — *updates* — _isPublic
- materialize parent-change cascade — *updates* — _effectiveSecurityClassCodes
- lack of cascade — *causes stale* — _isPublic
- lack of cascade — *causes stale* — _effectiveSecurityClassCodes
- stale fields — *lead to* — wrong access
- wrong access — *on* — materialized read path
- sentinel fields — *leak into* — API responses
- single-doc snapshot — *breaks at* — Mongo's 16MB cap
- SC_MULTI_STATUS — *wrapped as* — retryable
- retryable — *causes* — rollback
- rollback — *reverts* — correct docs
- migration executor — *causes* — infinite loop
- infinite loop — *wedges* — single-thread migration pipeline
- Quick wins — *address* — DistributionCacheException.isNotFound
- Quick wins — *address* — this
- Quick wins — *address* — self.getSnapshot
- Quick wins — *address* — copy rename path
- Quick wins — *address* — projection keys
- 3 refuted candidates — *include* — missing-4th-sentinel exposure
- 3 refuted candidates — *include* — empty-codes divergence
- 3 refuted candidates — *include* — ForkJoinPool thread-safety
- luz_docs folder security-class changes have 3 entry points but only PUT cascades — *is related to* — folder security-class changes
- DistributionCacheException.isNotFound is inverted in luz_docs — *is related to* — DistributionCacheException.isNotFound
- Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore SC_MULTI_STATUS as benign — *is related to* — SC_MULTI_STATUS
- Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore SC_MULTI_STATUS as benign — *mentions* — jsonstore
- CDI self-invocation bypasses interceptor proxy — *is related to* — CDI self-invocation
- CDI self-invocation bypasses interceptor proxy — *mentions* — interceptor proxy
- jsonstore projections need quoted JSON keys and Mongo 16MB doc limit caps single-doc snapshots — *is related to* — jsonstore
- jsonstore projections need quoted JSON keys and Mongo 16MB doc limit caps single-doc snapshots — *mentions* — projection keys
- jsonstore projections need quoted JSON keys and Mongo 16MB doc limit caps single-doc snapshots — *mentions* — Mongo's 16MB cap
- jsonstore projections need quoted JSON keys and Mongo 16MB doc limit caps single-doc snapshots — *mentions* — single-doc snapshot
- luz_docs — *contains* — materialize package
- luz_docs — *contains* — folder security-class changes
- luz_docs — *contains* — DistributionCacheException.isNotFound

%% ai-graph-end %%