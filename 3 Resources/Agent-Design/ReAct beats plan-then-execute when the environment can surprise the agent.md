---
ai_hash: 3fbcc517b8463b9b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Multi-Agentic Architecture - Theory (2026-03-23)'
status: seedling
tags:
- ai-agents
- react
- planning
- agent-design
- llm
- design-tradeoff
title: ReAct beats plan-then-execute when the environment can surprise the agent
type: argument
---

# ReAct beats plan-then-execute when the environment can surprise the agent

**ReAct** interleaves Thought → Action → Observation, deciding the next step *after* seeing the last result. **Plan-then-execute** commits to a full plan up front and then carries it out.

The choice is not about sophistication — it is about **whether the environment can tell you something you did not know when you planned**. In test automation, code exploration, debugging, or anything touching a live system, it always can: a file is not where you expected, a service returns an unexpected shape, a query comes back empty. A pre-built plan has no branch for that, so the agent either barrels through executing wrong steps or stalls and needs a replan — and the replan discards the work already done.

ReAct absorbs the surprise as a normal observation and self-corrects mid-flight. That is why it was chosen for the Kepler testing agents over plan-execute.

The counterweight, worth stating because it is the real cost: **ReAct has no commitment**, so it can wander, revisit, and spend turns without converging. Plan-execute is better where the environment is known and stable, where you need the plan *reviewed* before anything executes, or where an audit trail of intended-vs-actual matters.

Practical middle ground used in practice: plan at the coarse level (phases that rarely need revision), ReAct within each phase.

## Related

- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]
- [[Agent equals model plus harness, and the harness is the engineering discipline]]

## Related

- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]

%% ai-graph-start %%

**Related notes:**
- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]
- [[Multi-Agentic Architecture - Apply in AI Driven Testing]]
- [[Multi-Agentic Architecture - Theory]]
- [[Sub Agentic Loop 3.2 - Implement]]
- [[Agentic browser testing discover once, compile deterministic, heal only on failure]]

%% ai-graph-end %%