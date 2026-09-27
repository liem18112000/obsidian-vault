---
title: "test-agent-v2 fixes: implement_plan predictive budget guard + gather explore opt-in gate"
created: 2026-09-15
type: lesson
status: seedling
source: "test-agent-v2 code changes 2026-09-15"
tags: [testing-agent, tpd, kga, implement_plan, gather, timeout, precision, v2]
---

# test-agent-v2 fixes: implement_plan predictive budget guard + gather explore opt-in gate

Two fixes to test-agent-v2 (branch feature/test-agent/v2-adk), 2026-09-15, from the LUZ-158230 run.

## 1. implement_plan 900s timeout → PREDICTIVE budget guard
`test_plan_definition/implement/assured/loop.py`. The P4 assured loop (generate→judge→gate→reflect→regenerate) runs up to `TPD_ASSURED_MAX_ITERS` (default 2) rounds; each round = a generate + a judge Vertex call (each already bounded to `TPD_GEN_TIMEOUT_S`, default 180s). The **bug**: the wall-clock budget (`TPD_ASSURED_BUDGET_S`, 540s) was checked POST-HOC at the top of the loop (`if history and elapsed > budget`). A ~500s round-1 is < 540 so round 2 still STARTED and pushed the total past the 900s MCP ceiling → the client killed the call with nothing persisted.
**Fix:** make the guard PREDICTIVE — measure each completed rounds duration (`round_durations`) and, before starting the next round, break if `elapsed + max(round_durations) > budget_s`. The first round in a run is always allowed (its own per-call timeout bounds it). Net: the loop returns comfortably under 900s and always persists a best-effort set. Regression test: `test_assured_predictive_budget_stops_before_overrunning` (1s budget + ~1.2s rounds → stops after round 1).

## 2. gather noise → opt-in `explore` gate
The gather pulled in three noisy tiers by default (drove retrieval precision to 0.00): **cloud/system-service discovery** (`cloud_configured()`), **external web-follow** (`Scope(follow_web=True)`), and the **LLM planners** (hypothesize + leads). Now all three are gated behind a single **`explore` opt-in (default False)** — a client-owned Yes gate (MCP elicitation is broken over HTTP, so the client asks the user then passes the flag). Plumbing: `gather_knowledge(explore=False)` MCP param → JSON `{"explore": true}` (or an `explore`/`deep` token in the seed text) → `parse_input` returns a 5-tuple `(seed, depth, repo, exclude, explore)` → `GatherAgent` sets `cloud_on = explore and cloud_configured()`, skips `_plan` (LLM planners) unless explore, and sets `Scope(follow_web=explore)`. A quiet gather (core Jira/Confluence/codegraph + memory/atlassian-search self-seeding) is the default; the reply hints "re-run with explore=true". Tests: `test_parse_input_explore_flag_opts_into_noisy_tiers`; the D15 `drive_gather_agent` helper now drives explore-on.

All 496 tests pass, ruff clean. UNCOMMITTED. Supersedes the "just author client-side" workaround in [[Testing-Agent implement_plan assured loop times out at 900s MCP ceiling]].

## Related

- [[Testing-Agent implement_plan assured loop times out at 900s MCP ceiling]]
