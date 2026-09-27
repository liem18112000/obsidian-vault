---
ai_hash: ac34fa9d62f0951e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-15
entities: []
source: session 2026-09-15
status: seedling
tags:
- testing-agent
- interrogation
- refine
- knowledge-gathering
- test-agent-v2
title: Refine interrogation caps open questions at 7 per round (max_questions), overflow
  deferred
type: reference
---

# Refine interrogation caps open questions at 7 per round (max_questions), overflow deferred

In test-agent-v2, the knowledge-gather **refine** interrogation (and the shared InterrogationAgent that also backs TPD **define**) caps how many questions a single round presents:

- `RefineSession(..., max_questions: int = 7, max_rounds: int = 4)` — `common/interrogate/loop.py:24`. The refine spec constructs it WITHOUT overriding, so the default **7** applies.
- Each round: `generate_round(pack, round, max_questions=self.max_questions)` → `_defer_excess_open(questions, cap)` (`common/interrogate/questions.py:49`): keeps the first `cap` **open** questions open and flags the rest `status='deferred'` — **deferred, NOT dropped**, so overflow can resurface in a later round.
- Only `status=='open'` counts toward the cap; `self-answered` questions don't.
- Tunable: pass `max_questions=` to the session (persisted in refine state, rehydrated with default 7). Companion `max_rounds` default 4.

Empirical confirmation (LUZ-158230 / run-cd156028): the *scope* round emitted exactly 7 questions (hit the cap); business=2, technical=5, metrics=3 were under it.

Related: [[Deployed Testing-Agent refine loop freezes after completion and drops corrections]]

## Related

- [[Deployed Testing-Agent refine loop freezes after completion and drops corrections]]

%% ai-graph-start %%

**Related notes:**
- [[Deployed Testing-Agent refine loop freezes after completion and drops corrections]]
- [[test-agent-v2 fixes implement_plan predictive budget guard + gather explore opt-in gate]]
- [[Deployed Testing-Agent refine recommendations are speculative until validated]]
- [[TPD agentic loop single-pass DEFINE to APPROVE to IMPLEMENT]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]

%% ai-graph-end %%