---
title: "Swarm intelligence gets global behaviour from local rules and no central controller"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Swarm Intelligence - Theories (2026-03-23)"
tags: [swarm-intelligence, multi-agent, emergence, ai-agents, optimization]
---

# Swarm intelligence gets global behaviour from local rules and no central controller

**Swarm intelligence** (Wang & Beni, 1989) is collective behaviour in decentralised, self-organised systems. Its claim is counter-intuitive: agents following **simple local rules, with no central controller and no global view**, produce global behaviour that no individual agent possesses or understands.

Three principles hold it up:

1. **Decentralisation** — no leader. Each agent decides from local information. *The intelligence lives in the interactions, not in any node.*
2. **Self-organisation** — each agent senses its environment and adjusts; coordination emerges rather than being imposed.
3. **Emergence** — the global result is often surprising. An ant colony finds the shortest path to food although no ant knows the path; each only follows a [[Stigmergy coordinates through traces left in the environment, not messages between agents|pheromone trail]].

The classic algorithms are direct transcriptions: **Ant Colony Optimization** (pheromone-weighted path search — routing, scheduling), **Particle Swarm Optimization** (flocking — continuous optimisation), **Bee Algorithm** (foraging — combinatorial problems).

Why it matters for agent systems: swarms are **resilient** (individual failures are absorbed, because no agent is load-bearing), **adaptive** (local rules respond to change without a replan), and **scale by addition** — more agents make the system stronger, which is the opposite of an orchestrated pipeline where each added stage is another coordination cost.

The honest limitation: emergence is not steerable. You get global behaviour you did not specify, which is the point *and* the risk. Use it where a good-enough answer found robustly beats an exact answer found fragilely.

## Related

- [[Stigmergy coordinates through traces left in the environment, not messages between agents]]
- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]

## Related

- [[Stigmergy coordinates through traces left in the environment, not messages between agents]]
