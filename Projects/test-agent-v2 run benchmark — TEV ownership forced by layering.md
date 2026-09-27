---
ai_hash: c464aae0384a9da2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
status: planned
tags:
- test-agent-v2
- architecture
- decision
- evaluation
---

# Run benchmark must live in TEV (layering constraint)

**Feature:** benchmark a run = freeze the Test-Evaluation scores (PQS from `evaluate_pack`,
TPS from `evaluate_plan`) into a cached JSON blob; plus `compare_benchmarks` and
`summarize_benchmarks` (K latest, K<10). Plan doc: `test-agent-v2/docs/PLAN-run-benchmark.md`.

## The decision-and-why
All three benchmark tools own by **TEV**, not the admin agent — even though admin is the natural
"operator surface" that already enumerates runs (`list_runs`/`get_run`/`compare_runs`).

**Why:** the requirement "if benchmark data not found, calculate then save" means the aggregators
must be able to *compute*, and compute calls the scoring engines (`evaluate_pack`/`evaluate_plan`).
Those live in the `test_evaluation` package, and the repo rule is **agents must never import each
other, and `common` must not import an agent package**. So admin can't reach the engine, and the
engine can't move to `common`. Ergo compute — and therefore all three tools — sit in TEV. TEV can
still enumerate runs because that lives in `common.admin` (allowed).

## Load-bearing facts I had to dig for
- Eval scores are **computed on-demand, never persisted** (`common/admin/runs.py:108` says so
  verbatim). Benchmark is the first persisted per-run metric.
- `compare_runs` is a **text set-diff** (consensus %), pairwise only — NOT a numeric comparator.
  Reuse its `_pct`/`_bucket` formatters, not its logic.
- There is **no unified success-OR-fail run-finish funnel**; run logs write success-only. The only
  cross-agent vantage point over the whole pipeline is the **gateway** → eager benchmark hook wraps
  `implement_plan` there (`try/finally`), flag `BENCHMARK_ON_FINISH`.
- Deterministic eval tiers need no LLM (`trajectory=1.0` placeholder) → benchmark hot path is cheap;
  judged/semantic tier stays opt-in to avoid request-thread Vertex calls (Cloud-Run timeout risk).

## Pattern reused
Cache-aside via one choke point `load_or_compute(bank, ctx, recompute=False)`; storage
`memory/benchmarks/<slug(ctx)>.json` via `bank.put_json/get_json`; MCP tools are thin
`session.ask("<verb> <args>")` forwarders merged by the gateway.

Related: [[test-agent common shared engine]] · [[implement serial Vertex calls Cloud Run timeout]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 gateway tool interception — hook at registration not the _tools dict]]
- [[test-agent-v2 test_evaluation restructure engine + config packages + merged golden set]]
- [[test-agent-v2 Redis cache port + Memorystore needs a VPC connector]]
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[test-agent-v2 executor tests share memory-bank state and fail by test order]]

%% ai-graph-end %%