---
ai_hash: 447de0bf166e871d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: session 2026-09-07 code trace
status: seedling
tags:
- testing-agent
- tpd
- a2a
- agentic-loop
- architecture
title: 'TPD agentic loop: single-pass DEFINE to APPROVE to IMPLEMENT'
type: model
---

# TPD agentic loop: single-pass DEFINE to APPROVE to IMPLEMENT

The test-plan-definition (TPD) agent runs a three-stage agentic loop over one `context_id` (reused from knowledge-gathering): **DEFINE → APPROVE → IMPLEMENT**.

- **DEFINE** is a multi-turn interrogation but a *single fixed pass* over exactly 3 rounds in order: **methodology → scope → metrics** (`ROUNDS` in `models/plan.py`). Exactly **one round advances per human turn**; `PlanSession.next_questions` pops a round, generates its questions, and pauses at the first round with an open question. The stop condition is **purely structural** — the loop ends when the 3 rounds are exhausted. There is **no confidence/coverage gate** deciding whether to ask more; confidence is only computed at `finalize` (open questions→low, any assumption→medium, else high) and just sets `status = draft | confirmed`.
- **APPROVE** is the reconfirm gate. There is no separate lock object — `run_approve` simply **flips `TestPlan.status` to `confirmed`**. The status field *is* the lock. A `draft` plan (unresolved open gaps) is blocked from implement until approve force-confirms it.
- **IMPLEMENT** is **one-shot** (not multi-turn): it refuses unless `status == confirmed`, then generates **sequentially: test-data → scenarios → steps → Gherkin .feature** (each depends on the previous).

Client owns the Yes/No gates before refine/approve/define/implement; the agent never auto-approves.

Related: [[TPD IMPLEMENT makes one LLM call by default (scenarios only)]]

## Related

- [[TPD IMPLEMENT makes one LLM call by default (scenarios only)]]

%% ai-graph-start %%

**Related notes:**
- [[TPD IMPLEMENT makes one LLM call by default (scenarios only)]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[approve_plan is an agent-side write, unlike knowledge_gathering's read-only approve]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

%% ai-graph-end %%