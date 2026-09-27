---
title: "A shared mutable context beats a message bus for sequentially orchestrated agents"
created: 2026-09-27
type: argument
status: seedling
source: "Confluence: Multi-Agentic Architecture - Theory (2026-03-23)"
tags: [multi-agent, ai-agents, architecture, orchestration, design-tradeoff]
---

# A shared mutable context beats a message bus for sequentially orchestrated agents

For agents that run **sequentially under an orchestrator**, a plain mutable dictionary passed down the chain outperforms a message queue or event bus — and the Kepler multi-agent design picked it deliberately.

The reasoning: a bus buys you **decoupling, ordering guarantees, and fan-out**. Sequential phases under one orchestrator need none of those — the order is the call order, there is exactly one consumer per message, and the producer and consumer are in the same process. What you pay for the bus you do not need is serialization on every hop, broker operation, and a failure mode (message lost, consumer lagging) that a function call cannot have.

The shared dict also gives something a bus makes awkward: **accumulation**. Each agent adds its findings and the next sees everything discovered so far, not just its predecessor's output. That is the natural shape for "gather → analyse → decide" pipelines.

Where it stops working — and these are the signals to switch:

- agents need to run **concurrently** (a mutable dict shared across threads is a race waiting to happen);
- agents live in **separate processes or services**;
- one agent's output must reach **several** consumers;
- you need the exchange to be **durable** across a crash.

Design rule: **start with the dict, move to a bus when concurrency or a process boundary forces it** — not because a bus feels more architectural.

## Related

- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]
- [[Stigmergy coordinates through traces left in the environment, not messages between agents]]

## Related

- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]
