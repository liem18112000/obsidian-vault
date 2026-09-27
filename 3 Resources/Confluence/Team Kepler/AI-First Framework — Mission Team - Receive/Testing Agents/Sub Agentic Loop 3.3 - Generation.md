---
ai_hash: a1b14e7b81e9a499
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49741299914'
confluence_path: 'Team Kepler > AI-First Framework — Mission Team: Receive > Testing
  Agents > Agent Loop 3 - Test-Plan Definition > Evaluating the Test-Plan-Definition
  Agent'
created: 2026-09-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
title: Sub Agentic Loop 3.3 - Generation
type: source
updated: 2026-09-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49741299914/Sub+Agentic+Loop+3.3+-+Generation
---

# Sub Agentic Loop 3.3 - Generation

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 3 - Test-Plan Definition › Evaluating the Test-Plan-Definition Agent · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49741299914/Sub+Agentic+Loop+3.3+-+Generation) · updated 2026-09-10*

### The Assured Generation Loop

`run_assured_scenarios` (`implement/assured.py`) replaces the single blind scenario call — **opt-in** via `TPD_ASSURED` / `implement_plan(assured=True)`; default OFF keeps the I3 single call.

Bounded by `TPD_ASSURED_MAX_ITERS` (default 2):

1.  **GENERATE** `claude_scenarios(reflections)` → candidates.

2.  **JUDGE** `claude_judge_scenarios` → `JudgeVerdict` — a 7-dimension rubric (`ac_coverage`, `atomicity`, `testability`, `traceability`, `faithfulness`, `negative_edge_coverage`, `non_duplication`); `score()` = the model's `overall` or the mean of the seven.

3.  **GATE** keep iff `score() ≥ threshold` (`TPD_ASSURED_THRESHOLD`, 0.7) → **accepted** (best scenarios + `AssuredReport`).

4.  **REFLECT** below the bar with iterations left → carry `verdict.reflections` (dedup) into the next GENERATE.

Each round checkpoints to `assured.json` so a Cloud-Run kill **resumes, not restarts** (`_resumable`).

![[image-20260910-064932.png]]

%% ai-graph-start %%

**Related notes:**
- [[Sub Agentic Loop 3.2 - Implement]]
- [[Agent Loop 3 - Test-Plan Definition]]
- [[Sub Agentic Loop 3.1 - Define]]
- [[Test-Plan Definition Agent]]
- [[Evaluating the Test-Plan-Definition Agent]]

%% ai-graph-end %%