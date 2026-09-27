---
title: "Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Multi-Agentic Architecture - Theory (2026-03-23)"
tags: [multi-agent, ai-agents, architecture, orchestration, llm]
---

# Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism

A **multi-agent system** splits work across several autonomous agents, each with a narrow role and a curated set of tools, coordinated by an orchestrator or a protocol. You give up the simplicity of one agent to buy four things:

- **Specialisation** — a narrow tool set means fewer wrong tool choices. Most agent errors are selection errors, and they scale with how many tools are in scope.
- **Context isolation** — each agent's window carries only its own concern, so one agent's long tool output cannot crowd out another's instructions.
- **Parallelism** — independent sub-tasks run concurrently instead of serially filling one context.
- **Resilience** — one agent failing does not crash the pipeline.

The costs are real and usually underestimated: handoffs lose information, the orchestrator becomes the thing that needs debugging, and total token spend rises because context gets re-established per agent.

The honest test for whether you need one: **would a single agent's context window be dominated by material irrelevant to the step it is on?** If yes, split. If the task simply has many steps, a single agent with a task list is simpler and usually better.

Note it is not the same discipline as [[Agent equals model plus harness, and the harness is the engineering discipline|harness engineering]] — multi-agent is one *pattern within* a harness, not an alternative to it. You still need guardrails, feedback loops and memory; you now need them per agent.

## Related

- [[ReAct beats plan-then-execute when the environment can surprise the agent]]
- [[A shared mutable context beats a message bus for sequentially orchestrated agents]]
- [[Swarm intelligence gets global behaviour from local rules and no central controller]]

## Related

- [[ReAct beats plan-then-execute when the environment can surprise the agent]]
- [[A shared mutable context beats a message bus for sequentially orchestrated agents]]
