---
ai_hash: 16b676b9deb33184
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- test-agent-v2
- test_evaluation
- test_evaluation restructure
- engine/
- config/
- merged golden set
- feature/test-agent/v2-adk branch
- engine.py
- plan_engine.py
- loaders.py
- pack_view()
- plan_artifacts()
- pack.py
- evaluate_pack
- KGA
- PQS
- plan.py
- evaluate_plan
- TPD
- TPS
- __init__.py (engine)
- golden/
- golden_plans/
- golden/<seed>.json
- pack view
- plan view
- expected_trajectory
- gather/refine/approve view
- define/approve/implement view
- golden/canary/
- golden.py
- _project(d, view)
- load_golden()
- load_golden_plans()
- load_canaries()
- load_canary_plans()
- _view(dir, view)
- _case_for(ctx, cases, model_cls)
- eval constants
- adk.py
- NATIVE_TRAJECTORY_METRIC
- JUDGED_METRICS
- JUDGED_THRESHOLD
- TEST_CONFIG
- adk_metrics.py
- eval/adk_metrics.py
- partitions.py
- FULL_MATRIX
- views.py
- GOLDEN_VIEWS
- layers.py
- PQS_LAYERS
- TPS_LAYERS
- eval/config.py
- eval/evalset.py
- config.JUDGED_METRICS
- evalset.TEST_CONFIG
- _FULL_MATRIX
- _PQS_LAYERS
- _TPS_LAYERS
- ruff F821
- RESEARCH eval docs
- structure update banner
- 512 tests
- ruff
- test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in
  gate
source: test-agent-v2 refactor 2026-09-15
status: seedling
tags:
- testing-agent
- tev
- eval
- refactor
- solid
- golden
- config
- v2
title: 'test-agent-v2 test_evaluation restructure: engine/ + config/ packages + merged
  golden set'
type: lesson
---

# test-agent-v2 test_evaluation restructure: engine/ + config/ packages + merged golden set

Restructured test-agent-v2 `test_evaluation` (branch feature/test-agent/v2-adk, 2026-09-15), per user request.

## engine/ package (SOLID split)
`engine.py` + `plan_engine.py` (two flat modules) -> a package `test_evaluation/engine/`:
- `loaders.py` — `pack_view()` + `plan_artifacts()` (data access / I/O, SRP).
- `pack.py` — `evaluate_pack` (KGA -> PQS).
- `plan.py` — `evaluate_plan` (TPD -> TPS).
- `__init__.py` — facade re-exporting both (so `from test_evaluation.engine import evaluate_pack` still works). `plan_engine` importers repointed to the facade.

## Golden merge (unified per-seed, nested)
`golden/` + `golden_plans/` merged into ONE `golden/<seed>.json` per seed with nested `pack` / `plan` view sub-objects (nesting is required — `expected_trajectory` differs per view: gather/refine/approve vs define/approve/implement, so a flat union collides). `golden_plans/` removed; canaries stay under `golden/canary/` (also nested). `golden.py` loaders PROJECT each view: `_project(d, view)` = shared top-level keys + `d[view]`; `load_golden()`/`load_golden_plans()`/`load_canaries()`/`load_canary_plans()` keep their original per-view shape, so every caller + eval test is unchanged. DRY: the 4 loaders collapse to a `_view(dir, view)` helper; `golden_for`/`golden_plan_for` collapse to a `_case_for(ctx, cases, model_cls)` helper (both from_dict ignore unknown keys).

## config/ package (constants by type)
Extracted scattered eval constants into `test_evaluation/config/` — one file per type: `adk.py` (NATIVE_TRAJECTORY_METRIC, JUDGED_METRICS, JUDGED_THRESHOLD, TEST_CONFIG — named `adk.py` NOT adk_metrics.py to avoid colliding with the existing `eval/adk_metrics.py` custom-metric functions), `partitions.py` (FULL_MATRIX), `views.py` (GOLDEN_VIEWS), `layers.py` (PQS_LAYERS/TPS_LAYERS). `eval/config.py` + `eval/evalset.py` re-import (re-export) so `config.JUDGED_METRICS` / `evalset.TEST_CONFIG` stay importable for tests. GOTCHA: renaming a module constant to a public config name means you must update every USAGE too — I removed defs + added imports but first missed `_FULL_MATRIX`/`_PQS_LAYERS`/`_TPS_LAYERS` usages -> NameError; ruff F821 catches these.

Added a "structure update" banner to the 4 RESEARCH eval docs. 512 passed, ruff clean. UNCOMMITTED (tree also holds unrelated pre-existing WIP). Related: [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]].

## Related

- [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 run benchmark — TEV ownership forced by layering]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]

**Relations:**
- test_evaluation — *is part of* — test-agent-v2
- test_evaluation — *underwent* — test_evaluation restructure
- test_evaluation restructure — *occurred on branch* — feature/test-agent/v2-adk branch
- test_evaluation restructure — *involved* — engine/
- test_evaluation restructure — *involved* — config/
- test_evaluation restructure — *involved* — merged golden set
- engine.py — *moved to* — engine/
- plan_engine.py — *moved to* — engine/
- engine/ — *contains* — loaders.py
- loaders.py — *contains* — pack_view()
- loaders.py — *contains* — plan_artifacts()
- engine/ — *contains* — pack.py
- pack.py — *contains* — evaluate_pack
- evaluate_pack — *transforms* — KGA
- evaluate_pack — *produces* — PQS
- engine/ — *contains* — plan.py
- plan.py — *contains* — evaluate_plan
- evaluate_plan — *transforms* — TPD
- evaluate_plan — *produces* — TPS
- engine/ — *contains* — __init__.py (engine)
- __init__.py (engine) — *re-exports* — evaluate_pack
- __init__.py (engine) — *re-exports* — evaluate_plan
- plan_engine.py — *importers repointed to* — __init__.py (engine)
- golden/ — *merged into* — golden/<seed>.json
- golden_plans/ — *merged into* — golden/<seed>.json
- golden_plans/ — *was* — removed
- golden/<seed>.json — *contains nested* — pack view
- golden/<seed>.json — *contains nested* — plan view
- expected_trajectory — *differs for* — gather/refine/approve view
- expected_trajectory — *differs for* — define/approve/implement view
- canaries — *stay under* — golden/canary/
- golden.py — *contains* — _project(d, view)
- golden.py — *contains* — load_golden()
- golden.py — *contains* — load_golden_plans()
- golden.py — *contains* — load_canaries()
- golden.py — *contains* — load_canary_plans()
- _project(d, view) — *projects* — pack view
- _project(d, view) — *projects* — plan view
- load_golden() — *maintains interface* — per-view shape
- load_golden_plans() — *maintains interface* — per-view shape
- load_canaries() — *maintains interface* — per-view shape
- load_canary_plans() — *maintains interface* — per-view shape
- load_golden() — *collapses to* — _view(dir, view)
- load_golden_plans() — *collapses to* — _view(dir, view)
- load_canaries() — *collapses to* — _view(dir, view)
- load_canary_plans() — *collapses to* — _view(dir, view)
- golden_for — *collapses to* — _case_for(ctx, cases, model_cls)
- golden_plan_for — *collapses to* — _case_for(ctx, cases, model_cls)
- eval constants — *extracted into* — config/
- config/ — *contains* — adk.py
- adk.py — *contains* — NATIVE_TRAJECTORY_METRIC
- adk.py — *contains* — JUDGED_METRICS
- adk.py — *contains* — JUDGED_THRESHOLD
- adk.py — *contains* — TEST_CONFIG
- adk.py — *avoids colliding with* — adk_metrics.py
- eval/adk_metrics.py — *are* — existing custom-metric functions
- config/ — *contains* — partitions.py
- partitions.py — *contains* — FULL_MATRIX
- config/ — *contains* — views.py
- views.py — *contains* — GOLDEN_VIEWS
- config/ — *contains* — layers.py
- layers.py — *contains* — PQS_LAYERS
- layers.py — *contains* — TPS_LAYERS
- eval/config.py — *re-imports* — config.JUDGED_METRICS
- eval/evalset.py — *re-imports* — evalset.TEST_CONFIG
- ruff F821 — *catches* — NameError
- structure update banner — *added to* — RESEARCH eval docs
- 512 tests — *are* — passed
- ruff — *is* — clean
- test_evaluation restructure — *related to* — test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate

%% ai-graph-end %%