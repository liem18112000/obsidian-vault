---
ai_hash: f2c9575fb6f8e2ac
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49255219207'
confluence_path: Team Kepler > Developer note > AI Research > Agentic and LLM
created: 2026-03-20
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
- search
title: 'Swarm Intelligence: Theories'
type: source
updated: 2026-03-23
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49255219207/Swarm+Intelligence+Theories
---

# Swarm Intelligence: Theories

*Confluence source · Team Kepler › Developer note › AI Research › Agentic and LLM · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49255219207/Swarm+Intelligence+Theories) · updated 2026-03-23*

## The Concept

### Definition

Swarm intelligence (SI) is the collective behavior of decentralized, self-organized systems, natural or artificial. The expression was introduced by Jing Wang and Gerardo Beni in 1989, in the context of cellular robotic systems

The core idea is deceptively simple: the agents follow very simple rules, and although there is no centralized control structure dictating how individual agents should behave, local, and to a certain degree random, interactions between such agents lead to the emergence of "intelligent" global behavior, unknown to the individual agents.

![[02_swarm_intelligence_concept.png]]

### Three Foundational Principles

1.  **Decentralization** — There is no central leader, no master controller. Each agent makes its own local decisions based on local information. The intelligence lives in the *interactions*, not in any single node.

2.  **Self-Organization** — Coordination emerges naturally through interactions. Each agent in the swarm can sense the changes in its environment and adjust its behavior accordingly. Order arises spontaneously from disorder.

3.  **Emergence** — Emergent behavior is often surprising and counter-intuitive. For example, a swarm of ants can find the shortest path to a food source, even though no single ant knows the entire path. Each ant follows simple, local rules, such as following the scent trail left by other ants. These simple rules lead to the emergence of complex, global behavior, such as path optimization.

### Stigmergy — The Communication Mechanism

- Stigmergy refers to the indirect communication between agents through changes in their environment.

- For example, ants communicate with each other by leaving a pheromone trail that other ants can follow.

- This form of communication is indirect, as the ants do not interact directly, but through the changes they make in their environment.

- This is how swarms coordinate without any centralized messaging system.

### Natural Examples

Examples of swarm intelligence in natural systems include ant colonies, bee colonies, bird flocking, hawks hunting, animal herding, bacterial growth, fish schooling and microbial intelligence.

A particularly elegant example: a colony of ants collectively achieves complex tasks such as constructing nests, taking care of their young, building bridges and foraging for food — yet no individual ant understands the global plan.

### Classic Algorithms Inspired by SI

**Ant Colony Optimization (ACO)** — Models how ants find shortest paths using pheromone trails. Used for routing, scheduling, and logistics problems.

![[01_ant_colony_optimization_ACO-20260320-021958.png]]

**Particle Swarm Optimization (PSO)** — Models how bird flocks or fish schools search for food.

![[02_particle_swarm_optimization_PSO-20260320-021958.png]]

**Bee Algorithm** — Models how honey bees select food sources. Used for combinatorial optimization.

![[03_bee_algorithm_BA-20260320-021958.png]]

### Why It Matters for AI

The appeal of swarm intelligence lies in its resilience and flexibility. These systems can continue functioning even when individual agents fail, and they can adapt quickly to new information. They also scale naturally: adding more agents strengthens the system.

## Attachments

*Attached to the Confluence page but not embedded in its body.*

- [[3 Resources/Confluence/Team Kepler/Developer note/AI Research/Agentic and LLM/attachments/swarm-intelligence-theories/mirofish_five_stage_pipeline.svg|mirofish_five_stage_pipeline.svg]]
- [[3 Resources/Confluence/Team Kepler/Developer note/AI Research/Agentic and LLM/attachments/swarm-intelligence-theories/swarm_intelligence_emergence_concept.svg|swarm_intelligence_emergence_concept.svg]]

%% ai-graph-start %%

**Related notes:**
- [[Swarm intelligence gets global behaviour from local rules and no central controller]]
- [[Stigmergy coordinates through traces left in the environment, not messages between agents]]
- [[Swarm Intelligence - Miro Fish - Prediction Engine]]

%% ai-graph-end %%