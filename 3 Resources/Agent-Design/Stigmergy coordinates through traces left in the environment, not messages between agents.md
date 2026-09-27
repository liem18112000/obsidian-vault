---
title: "Stigmergy coordinates through traces left in the environment, not messages between agents"
created: 2026-09-27
type: term
status: seedling
source: "Confluence: Swarm Intelligence - Theories (2026-03-23)"
tags: [swarm-intelligence, multi-agent, coordination, ai-agents, distributed-systems]
---

# Stigmergy coordinates through traces left in the environment, not messages between agents

**Stigmergy** is coordination through **changes agents make to a shared environment**, rather than through messages sent to each other. Ants do not tell each other where the food is; they deposit pheromone, and the trail itself carries the information. The signal outlives the sender and is readable by anyone who passes.

Three properties fall out of it, and they are exactly the ones that matter for multi-agent software:

- **No addressing.** A sender needs no recipient list. Agents can be added or removed without rewiring anything.
- **No liveness coupling.** The trace persists after the writer is gone, so an agent that crashes still contributes what it learned.
- **Natural decay.** Pheromone evaporates, which is how ant colonies forget stale paths. A shared store without an expiry policy keeps acting on information that is no longer true.

This is why a **shared scratchpad, task board, or context dict** is not a poor man's message bus — it is a different coordination model with different guarantees. Message passing gives you ordering and delivery semantics; stigmergy gives you decoupling and resilience. Choosing the bus when the problem is stigmergic adds serialization, addressing and failure handling you did not need.

The design question to carry over: **what is my pheromone, and what makes it evaporate?** Most agent systems answer the first and forget the second.

## Related

- [[Swarm intelligence gets global behaviour from local rules and no central controller]]
- [[A shared mutable context beats a message bus for sequentially orchestrated agents]]

## Related

- [[Swarm intelligence gets global behaviour from local rules and no central controller]]
