---
title: "test-agent-v2 test_evaluation restructure: engine/ + config/ packages + merged golden set"
created: 2026-09-15
type: lesson
status: seedling
source: "test-agent-v2 refactor 2026-09-15"
tags: [testing-agent, tev, eval, refactor, solid, golden, config, v2]
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
