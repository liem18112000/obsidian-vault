---
ai_hash: c58ad53ad668a930
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
source: session 2026-09-14 run-cd156028
status: seedling
tags:
- testing-agent
- implement-plan
- p4-assured-loop
- timeout
- vertex
- gotcha
- luz-158230
title: Deployed implement_plan P4 assured loop times out at 900s for broad features
type: lesson
---

# Deployed implement_plan P4 assured loop times out at 900s for broad features

On the deployed Testing-Agent, `implement_plan` (scenario generation) runs the **P4 assured loop** — generate → judge → gate → reflect → regenerate — which fires several serial Vertex calls per round. For a broad feature (LUZ-158230 ZIP import: 7 test kinds — happy/negative/boundary/error/security/concurrency/performance — with uncapped cases) this **exceeds the MCP tool's 900s ceiling** and the whole call fails.

Observed on run-cd156028: the three implement design rounds (case-design, data-design, step-oracle) each advanced fine on repeated `implement_plan` calls (the tool has no `answer` param — each call accepts the recommended default and advances one round). But the final generation step hung: two client-side 120s timeouts, then a backgrounded run that **failed at 900s with nothing checkpointed** — `get_scenarios` stayed empty, so there was no partial resume to salvage.

The P4 loop is **always-on for scenarios** (only `detail=True` adds the heavier test-data+steps LLM pass, which you do NOT want here), and there is **no cap/among knob exposed via MCP** to narrow it. So there is no cheap deployed fallback.

**Workaround:** author the scenarios client-side from the confirmed plan (same pattern as authoring the plan client-side). Consequence: with no stored scenarios, `evaluate_plan` (the Test-Plan Score) has nothing to score, so that deployed metric is unobtainable when generation times out — the PQS from `evaluate_pack` is still available since it runs on the pack, not the scenarios.

Related: [[Deployed Testing-Agent refine loop freezes after completion and drops corrections]]

## Related

- [[Deployed Testing-Agent refine loop freezes after completion and drops corrections]]

%% ai-graph-start %%

**Related notes:**
- [[Testing-Agent implement_plan assured loop times out at 900s MCP ceiling]]
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]]
- [[implement_plan heuristic-fallback emits one performance stub per node]]
- [[Testing-Agent implement_plan silent heuristic fallback = per-node x kind empty-step scenarios]]

%% ai-graph-end %%