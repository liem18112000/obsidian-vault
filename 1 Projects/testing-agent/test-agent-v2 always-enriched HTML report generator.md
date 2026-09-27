---
title: "test-agent-v2 always-enriched HTML report generator"
created: 2026-09-16
type: howto
status: seedling
source: "Testing-Agent run-188f96b8 follow-up"
tags: [testing-agent, report, html, bridge-prompts, feature]
---

# test-agent-v2 always-enriched HTML report generator

The Testing-Agent (test-agent-v2) now has a **deterministic enriched HTML report** so every run's deliverable is the same rich format (was a client-authored plain scenario list before).

- **Renderer**: `src/common/testplan/report/html.py` → `build_report_html(bank, context_id) -> str`. One self-contained page, tabs: **Scenarios · Feature files (inline Gherkin) · Test data · Benchmark (assured score from `read_assured_state` + TEV scorecard from `read_benchmark`) · Test-case Workflow** (per-case step flow + an explanation of what each case verifies — this is the "test-case workflow", NOT a system/architecture diagram). Pure render; reads via `common.testplan.memory` only; **no `test_plan_definition` import** (common must stay agent-independent), so Gherkin is rendered inline rather than reusing TPD's `render_feature`.
- **CLI**: `tools/build_report.py <context_id> [out.html]` — builds the bank (`MemoryBank(build_object_store())`, same `STORE_BACKEND`/`GCS_BUCKET` env as the agents) and writes the report.
- **Mandate**: `src/common/bridge/prompts.py` step 7 rewritten to REQUIRE the enriched report via the tool (with a self-render fallback) instead of the vague "render HTML artifact" that caused the plain default.
- **Design decision**: render from the *persisted run* (deterministic) rather than LLM re-authoring → "always enriched" is guaranteed byte-stably. Ticket-specific fixture `.zip`s are NOT auto-synthesized (that was hand-built for LUZ-158230); the generic Test-data tab lists the `test_data` node specs.
- **Deploy caveat**: bridge/prompts instructions load at MCP init, so the mandate only takes effect after **mcp-gateway-v2 redeploys AND the client `/mcp`-reconnects** (see [[MCP instructions load at init — reconnect to refresh]]).
- Committed **3a39108**, pushed to origin/feature/test-agent/v2-adk; **not yet deployed** (deploy later, after session e39a398f finishes its 6e94595-redis deploy).

## Related

- [[Deploying the test-agent-v2 Cloud Run stack (names]]
- [[tags]]
- [[plan)]]
