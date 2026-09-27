---
ai_hash: 572f5a4473398f7d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities: []
source: Testing-Agent run-8eafe7ea implement, 2026-09-18
status: seedling
tags:
- testing-agent
- tpd
- implement_plan
- assured-loop
- gotcha
title: TPD assured-loop judge penalizes cross-run scenario duplication
type: lesson
---

# TPD assured-loop judge penalizes cross-run scenario duplication

When you re-run the Testing-Agent on a ticket that has **prior runs**, `implement_plan`s P4 assured-loop judge tends to score **BELOW BAR** for **duplication**: scenarios accumulated in the GCS memory bank from earlier runs (e.g. a `:001-:012` family + a `codegraph-*` family) get re-surfaced alongside the new `:1-:10` family, so the same behaviour appears 2-3× with only cosmetic wording differences. This inflates suite size without adding coverage and tanks the score.

Two other recurring judge penalties on the same round:
- **Methodology untagged** — if the plan specifies a method sequence (e.g. `api, e2e, api, ui, api`), scenarios not tagged/grouped by slot make the mix unverifiable (esp. whether a *true* UI-driven scenario exists).
- **Misleading titles** — a title whose verb ("Reject...") contradicts the asserted Then (fallback + success) is flagged as inconsistent with the confirmed model.

Fix by re-invoking `implement_plan(guidance=...)` with an explicit steer: "each distinct behaviour EXACTLY ONCE; keep the best-traced variant, delete near-duplicates; tag each scenario by methodology slot; cite the most specific source node (confluence/attachment), not the umbrella jira; audit every title verb against its outcome." The loop is chunked/multi-turn and runs the retry seeded by that guidance.

## Related
[[LUZ-158230 test approach full-chain real-deps integration with fully-materialized done]]

%% ai-graph-start %%

**Related notes:**
- [[Testing-Agent implement_plan assured loop times out at 900s MCP ceiling]]
- [[Deployed implement_plan P4 assured loop times out at 900s for broad features]]
- [[TPD test_kinds must be additive over the base four, not replace them]]
- [[Parallel TPD generation sped up but amplified duplication — dedup is a code fix, not guidance]]
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]

%% ai-graph-end %%