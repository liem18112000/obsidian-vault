---
ai_hash: d9c56263a612b764
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities:
- test-agent-v2
- HTML report generator
- Testing-Agent
- deterministic enriched HTML report
- client-authored plain scenario list
- src/common/testplan/report/html.py
- build_report_html
- bank
- context_id
- self-contained page
- Scenarios
- Feature files
- inline Gherkin
- Test data
- Benchmark
- assured score
- read_assured_state
- TEV scorecard
- read_benchmark
- Test-case Workflow
- per-case step flow
- explanation of what each case verifies
- common.testplan.memory
- test_plan_definition
- render_feature
- tools/build_report.py
- MemoryBank
- build_object_store()
- STORE_BACKEND
- GCS_BUCKET
- agents
- src/common/bridge/prompts.py
- step 7
- enriched report
- tool
- self-render fallback
- vague "render HTML artifact"
- plain default
- persisted run
- LLM re-authoring
- always enriched
- byte-stably
- Ticket-specific fixture .zip
- LUZ-158230
- test_data node specs
- bridge/prompts instructions
- MCP init
- mandate
- mcp-gateway-v2
- client
- /mcp-reconnects
- MCP instructions load at init — reconnect to refresh
- 3a39108
- origin/feature/test-agent/v2-adk
- e39a398f
- 6e94595-redis
- Deploying the test-agent-v2 Cloud Run stack (names
- tags
- plan
source: Testing-Agent run-188f96b8 follow-up
status: seedling
tags:
- testing-agent
- report
- html
- bridge-prompts
- feature
title: test-agent-v2 always-enriched HTML report generator
type: howto
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

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 retired agent-side HTML renderers — agent is data-only, client renders]]
- [[test-agent-v2 persists diagram-as-code + get_deliverables tool]]
- [[test-agent-v2 deploy + get_deliverables E2E verification]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[HTML report artifacts should be more concise]]

**Relations:**
- test-agent-v2 — *has component* — HTML report generator
- Testing-Agent — *is alias for* — test-agent-v2
- HTML report generator — *produces* — deterministic enriched HTML report
- deterministic enriched HTML report — *replaces* — client-authored plain scenario list
- src/common/testplan/report/html.py — *is renderer for* — deterministic enriched HTML report
- src/common/testplan/report/html.py — *defines function* — build_report_html
- build_report_html — *takes parameter* — bank
- build_report_html — *takes parameter* — context_id
- deterministic enriched HTML report — *is a* — self-contained page
- deterministic enriched HTML report — *has tab* — Scenarios
- deterministic enriched HTML report — *has tab* — Feature files
- deterministic enriched HTML report — *has tab* — Test data
- deterministic enriched HTML report — *has tab* — Benchmark
- deterministic enriched HTML report — *has tab* — Test-case Workflow
- Feature files — *contains* — inline Gherkin
- Benchmark — *includes* — assured score
- Benchmark — *includes* — TEV scorecard
- assured score — *derived from* — read_assured_state
- TEV scorecard — *derived from* — read_benchmark
- Test-case Workflow — *shows* — per-case step flow
- Test-case Workflow — *shows* — explanation of what each case verifies
- src/common/testplan/report/html.py — *reads via* — common.testplan.memory
- src/common/testplan/report/html.py — *does not import* — test_plan_definition
- test_plan_definition — *contains function* — render_feature
- tools/build_report.py — *is CLI for* — HTML report generator
- tools/build_report.py — *takes parameter* — context_id
- tools/build_report.py — *takes parameter* — out.html
- tools/build_report.py — *builds* — MemoryBank
- MemoryBank — *uses* — build_object_store()
- MemoryBank — *configured by env var* — STORE_BACKEND
- MemoryBank — *configured by env var* — GCS_BUCKET
- MemoryBank — *is used by* — agents
- src/common/bridge/prompts.py — *contains* — mandate
- mandate — *is in* — step 7
- mandate — *requires* — enriched report
- enriched report — *generated via* — tool
- tool — *replaces* — self-render fallback
- tool — *replaces* — vague "render HTML artifact"
- vague "render HTML artifact" — *caused* — plain default
- Design decision — *is to render from* — persisted run
- persisted run — *ensures* — always enriched
- persisted run — *ensures* — byte-stably
- LLM re-authoring — *is not used for* — rendering
- Ticket-specific fixture .zip — *are not* — auto-synthesized
- LUZ-158230 — *had* — hand-built .zip
- Test data — *lists* — test_data node specs
- bridge/prompts instructions — *load at* — MCP init
- mandate — *takes effect after* — mcp-gateway-v2 redeploys
- mandate — *takes effect after* — client /mcp-reconnects
- mandate — *related to* — MCP instructions load at init — reconnect to refresh
- 3a39108 — *is commit for* — origin/feature/test-agent/v2-adk
- 3a39108 — *is not yet deployed* — true
- deploy — *after session* — e39a398f
- e39a398f — *finishes* — 6e94595-redis deploy
- test-agent-v2 — *related to* — Deploying the test-agent-v2 Cloud Run stack (names
- test-agent-v2 — *related to* — tags
- test-agent-v2 — *related to* — plan

%% ai-graph-end %%