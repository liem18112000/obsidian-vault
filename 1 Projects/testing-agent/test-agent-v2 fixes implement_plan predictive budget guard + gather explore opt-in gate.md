---
ai_hash: 6e614b90269bcbc7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities:
- test-agent-v2
- implement_plan
- predictive budget guard
- gather explore opt-in gate
- feature/test-agent/v2-adk
- LUZ-158230
- implement_plan 900s timeout
- TPD_ASSURED_MAX_ITERS
- TPD_GEN_TIMEOUT_S
- TPD_ASSURED_BUDGET_S
- MCP ceiling
- test_assured_predictive_budget_stops_before_overrunning
- gather
- gather noise
- cloud/system-service discovery
- external web-follow
- LLM planners
- explore
- gather_knowledge
- parse_input
- GatherAgent
- cloud_configured()
- Scope(follow_web=True)
- Jira
- Confluence
- codegraph
- memory/atlassian-search
- test_parse_input_explore_flag_opts_into_noisy_tiers
- drive_gather_agent
- Testing-Agent implement_plan assured loop times out at 900s MCP ceiling
source: test-agent-v2 code changes 2026-09-15
status: seedling
tags:
- testing-agent
- tpd
- kga
- implement_plan
- gather
- timeout
- precision
- v2
title: 'test-agent-v2 fixes: implement_plan predictive budget guard + gather explore
  opt-in gate'
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[Testing-Agent implement_plan assured loop times out at 900s MCP ceiling]]
- [[Deployed implement_plan P4 assured loop times out at 900s for broad features]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]
- [[test-agent-v2 ran fully local end-to-end (LUZ-158390) — pipeline + quality gates proven]]

**Relations:**
- test-agent-v2 — *fixes* — predictive budget guard
- test-agent-v2 — *fixes* — gather explore opt-in gate
- test-agent-v2 — *is_on_branch* — feature/test-agent/v2-adk
- test-agent-v2 — *addresses_run* — LUZ-158230
- implement_plan — *had_bug* — implement_plan 900s timeout
- predictive budget guard — *replaces* — implement_plan 900s timeout
- implement_plan — *uses_parameter* — TPD_ASSURED_MAX_ITERS
- implement_plan — *uses_parameter* — TPD_GEN_TIMEOUT_S
- implement_plan — *uses_parameter* — TPD_ASSURED_BUDGET_S
- MCP ceiling — *is_value* — 900s
- predictive budget guard — *tested_by* — test_assured_predictive_budget_stops_before_overrunning
- gather — *had_problem* — gather noise
- gather noise — *caused_by* — cloud/system-service discovery
- gather noise — *caused_by* — external web-follow
- gather noise — *caused_by* — LLM planners
- gather explore opt-in gate — *solves* — gather noise
- explore — *is_opt-in_flag_for* — gather explore opt-in gate
- explore — *controls* — cloud/system-service discovery
- explore — *controls* — external web-follow
- explore — *controls* — LLM planners
- gather_knowledge — *accepts_parameter* — explore
- parse_input — *extracts_flag* — explore
- GatherAgent — *uses_flag* — explore
- GatherAgent — *configures_setting* — cloud_on
- GatherAgent — *configures_setting* — Scope(follow_web=True)
- GatherAgent — *conditionally_skips* — _plan
- gather — *uses_source* — Jira
- gather — *uses_source* — Confluence
- gather — *uses_source* — codegraph
- gather — *uses_source* — memory/atlassian-search
- gather explore opt-in gate — *tested_by* — test_parse_input_explore_flag_opts_into_noisy_tiers
- drive_gather_agent — *enables* — explore
- test-agent-v2 — *supersedes_workaround_in* — Testing-Agent implement_plan assured loop times out at 900s MCP ceiling
- implement_plan 900s timeout — *is_related_to* — Testing-Agent implement_plan assured loop times out at 900s MCP ceiling

%% ai-graph-end %%