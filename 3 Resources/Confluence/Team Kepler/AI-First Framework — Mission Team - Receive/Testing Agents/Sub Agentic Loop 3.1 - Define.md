---
title: "Sub Agentic Loop 3.1 - Define"
created: 2026-09-10
updated: 2026-09-10
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49741692945/Sub+Agentic+Loop+3.1+-+Define
confluence_id: "49741692945"
confluence_path: "Team Kepler > AI-First Framework — Mission Team: Receive > Testing Agents > Agent Loop 3 - Test-Plan Definition > Evaluating the Test-Plan-Definition Agent"
tags: [confluence, ai-agents]
---

# Sub Agentic Loop 3.1 - Define

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 3 - Test-Plan Definition › Evaluating the Test-Plan-Definition Agent · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49741692945/Sub+Agentic+Loop+3.1+-+Define) · updated 2026-09-10*

### DEFINE — the interrogation state machine

`define` is an `InterrogationAgent(kind="plan")` driving a `PlanSession` (`define/loop.py`).

One round per human turn:

1.  `next_questions()` pops the next round and calls `generate_round(pack, round)` — the LLM path (`claude_plan_questions`, when `VERTEX_*` is set) or the heuristic `RoundQuestions` strategy for that round.

2.  The agent **pauses** (`requires_input`); the human answers via `define_plan(answer=…)`.

3.  `submit()` → `ingest` → `decision_from_answer` distils a `PlanDecision` — a **human answer is a high-confidence** `DECISION`, an **agent self-answer a low-confidence** `ASSUMPTION`.

4.  When the 4 rounds are exhausted, `finalize()` → `assemble_plan` writes the `TestPlan` (`{confirmed | draft}`) + `plan-brief.md`. **The stop condition is structural** (rounds exhausted), not confidence-gated.

`ROUNDS = (methodology, scope, metrics, test-design)` (`common/testplan/models/plan.py`).

The 4th, `test-design`, is agent-**suggested**:

`TestDesignRound.recommend_methods(pack)` (`common/interrogate/round/test_design.py`) maps signals in the pack to methods — *lifecycle/state → state-transition, eligibility/matrix → decision-table, many params → pairwise, dependencies → error-path, money/auth/risk → risk-based depth,* always **EP+BVA** as the base — combined with rounds 1–3's decisions.

It surfaces one open question with the recommendation pre-filled; the human accepts / changes / **adds**. The choice lands on `TestPlan.test_design`.

![[3 Resources/Confluence/Team Kepler/AI-First Framework — Mission Team - Receive/Testing Agents/attachments/sub-agentic-loop-3-1-define/image-20260910-064629.png]]
