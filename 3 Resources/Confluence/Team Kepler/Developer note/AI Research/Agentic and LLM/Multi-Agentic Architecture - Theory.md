---
ai_hash: 3c59fc5e578e9b3b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49259216966'
confluence_path: Team Kepler > Developer note > AI Research > Agentic and LLM
created: 2026-03-23
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
- search
title: 'Multi-Agentic Architecture: Theory'
type: source
updated: 2026-03-23
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259216966/Multi-Agentic+Architecture+Theory
---

# Multi-Agentic Architecture: Theory

*Confluence source · Team Kepler › Developer note › AI Research › Agentic and LLM · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49259216966/Multi-Agentic+Architecture+Theory) · updated 2026-03-23*

## 1. What is a Multi-Agent System?

A **multi-agent system** (MAS) is an architecture where multiple autonomous AI agents collaborate to solve complex tasks that would be too large, too slow, or too error-prone for a single agent.

Each agent has a specialized role, a limited scope of tools, and communicates with other agents through shared state or message passing.

|  |  |
|----|----|
| Principle | Description |
| **Specialization** | Each agent has a narrow expertise and a curated set of tools |
| **Autonomy** | Agents reason and act independently within their scope |
| **Coordination** | An orchestrator (or protocol) sequences and routes work between agents |
| **Shared State** | Agents accumulate results in a shared context that flows downstream |
| **Resilience** | Failure in one agent doesn't crash the entire pipeline |

## 2. The ReAct Pattern (Reasoning + Acting)

Each agent follows the **ReAct** loop — a cycle of Thought, Action, and Observation

More details information: [https://www.promptingguide.ai/techniques/react](https://www.promptingguide.ai/techniques/react)

|  |  |
|----|----|
| Decision | Rationale |
| **ReAct over Plan-Execute** | Agents self-correct mid-flight; no rigid pre-plan needed |
| **Dict-based shared context over message queues** | Simpler for sequential orchestration; no serialization overhead |
| **Memory Bank over fine-tuning** | Facts are editable, scopeable, and don't require retraining |
| **Tool descriptions guide LLM** | No hardcoded decision trees; the LLM chooses which tools to use based on descriptions |

![[image-20260323-015643.png]]

## 3. Orchestrator Pattern

The **Orchestrator** is the top-level agent that sequences sub-agents through defined phases:

![[image-20260323-015740.png]]

Key characteristics:

- **Sequential phases**: Each phase depends on the output of the previous

- **Parallel prefetch**: Data gathering runs concurrently before agents start

- **Shared context**: A mutable dictionary flows through all agents, accumulating state

- **Cleanup**: Resources are released in a `finally` block regardless of success/failure

## 4. Tool System

Agents interact with the external world through **tools** — typed functions exposed to the LLM via function-calling protocol:

![[image-20260323-015844.png]]

Each tool has:

- **name**: Identifier the LLM uses to invoke it

- **description**: Natural language description (guides the LLM's choice)

- **parameters**: JSON schema defining required/optional arguments

- **handler**: The actual function that executes the action

## 5. Shared Context (State Bus)

The **shared context** is a mutable dictionary that flows through all agents as the primary state carrier

This pattern is simpler than message queues or event buses, and works well when agents run sequentially under an orchestrator.

![[image-20260323-015948.png]]

## 6. Memory Bank (Cross-Session Learning)

A **Memory Bank** enables agents to learn across sessions — facts discovered in Run N are available in Run N+1:

![[image-20260323-020125.png]]

Memory operations:

- **retrieve(query)**: Semantic search for relevant facts

- **create(fact)**: Store an explicit fact

- **generate(conversation)**: Auto-extract facts from agent conversation

## 7. Complete Architecture Overview

![[image-20260323-020406.png]]

%% ai-graph-start %%

**Related notes:**
- [[Multi-Agentic Architecture - Apply in AI Driven Testing]]
- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]
- [[A shared mutable context beats a message bus for sequentially orchestrated agents]]
- [[AI-Powered Development Environment Architecture]]
- [[ReAct beats plan-then-execute when the environment can surprise the agent]]

%% ai-graph-end %%