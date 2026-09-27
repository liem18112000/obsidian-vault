---
title: "Sub Agentic Loop 3.2 - Implement"
created: 2026-09-10
updated: 2026-09-18
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49741299899/Sub+Agentic+Loop+3.2+-+Implement
confluence_id: "49741299899"
confluence_path: "Team Kepler > AI-First Framework — Mission Team: Receive > Testing Agents > Agent Loop 3 - Test-Plan Definition > Evaluating the Test-Plan-Definition Agent"
tags: [confluence, ai-agents]
---

# Sub Agentic Loop 3.2 - Implement

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 3 - Test-Plan Definition › Evaluating the Test-Plan-Definition Agent · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49741299899/Sub+Agentic+Loop+3.2+-+Implement) · updated 2026-09-18*

### IMPLEMENT — the orchestrator (interrogate → generate)

Interactive `implement` is an `ImplementOrchestrator` (`implement/agent.py`) with `sub_agents = [interrogate, generate]`. (The **autonomous** `SequentialAgent` pipeline can't do HITL, so it keeps the one-shot `ImplementAgent` directly.)

**① interrogate** — an `InterrogationAgent(kind="implement")` over an `ImplementSession` (`implement/loop.py`, mirrors `PlanSession`) with 3 rounds:

- **case-design** — *which KINDS to cover?* Defaults (happy/negative/boundary/error) are a seed; the round **asks the human to add more**. `kinds_from_answer` distils the answer into `plan.test_kinds` — an **open, uncapped** taxonomy.

- **data-design** — valid/invalid partitions + BVA, stub-vs-real deps, fixtures.

- **step-oracle** — the oracle each case asserts (end-state / side-effect, not `assert 200`) + teardown.

State + decisions persist to `implement-decisions.json` / `implement-brief.md` / `implement-state.json {done}` (resumable). Once confirmed, the orchestrator chains into **② generate**.

**② generate** — `implement_plan()` (`implement/generate.py`):

`generate_test_data` → `generate_scenarios` (`claude_scenarios`, the single **I3** LLM call by default; `[+ assured loop]`) → `generate_all_steps` → `export_features` (`.feature`) → `build_coverage_matrix`.

Interrogation is heuristic (no model); `detail` / `assured` opt into more.

![[3 Resources/Confluence/Team Kepler/AI-First Framework — Mission Team - Receive/Testing Agents/attachments/sub-agentic-loop-3-2-implement/image-20260910-064800.png]]
