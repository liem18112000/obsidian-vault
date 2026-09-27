---
ai_hash: 47b2322a4c81921f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Agent
- Model
- Harness
- Harness engineering
- Mitchell Hashimoto
- HashiCorp
- Terraform
- CPU
- RAM
- Operating System
- Application
- Prompt
- Context Window
- Tools
- Guardrails
- Feedback Loops
- Memory
- State
- Context Management
- Security
- Orchestration
- Prompt text
- Validation Layer
- Linter
- Structural fix
- Prompt, context, and harness engineering
- ReAct
- Engineering discipline
- Thinking
- Model's focus
- Agent reliability
- CPU upgrade
- Smarter Model
- Agent misbehavior
- Missing guardrail
- Absent feedback loop
- Unparseable tool output
- Failure
source: 'Confluence: Harness Engineering - Designing Reliable AI Systems (2026-04-06)'
status: seedling
tags:
- ai-agents
- harness-engineering
- architecture
- reliability
- llm
title: Agent equals model plus harness, and the harness is the engineering discipline
type: term
---

# Agent equals model plus harness, and the harness is the engineering discipline

**Harness engineering** is designing everything *around* the model — tools, guardrails, feedback loops, memory, state, context management, security, orchestration — so an agent is reliable in production. The term was coined by **Mitchell Hashimoto** (HashiCorp/Terraform) in February 2026.

```
Agent = Model + Harness
```

The model is what **thinks**. The harness decides **what it thinks about**. The analogy that makes it click: the model is the CPU, the context window is RAM, the harness is the operating system, and the agent is the application. Swapping in a smarter model is a CPU upgrade; it does not give you an OS.

This reframes where agent reliability comes from. When an agent misbehaves, the reflex is to rewrite the prompt — but prompt text is only one component of the harness, and usually not the one that failed. More often the fix is a missing guardrail, an absent feedback loop, or a tool that returned something unparseable.

The operating principle is what makes it a *discipline* rather than a description:

> Whenever an agent makes a mistake, engineer a solution ensuring it never repeats that mistake.

That is a ratchet. Each failure becomes a permanent structural fix — a validation layer, a linter in the loop, a narrowed tool — rather than a prompt tweak that regresses on the next model change.

## Related

- [[Prompt, context, and harness engineering nest rather than replace each other]]
- [[ReAct beats plan-then-execute when the environment can surprise the agent]]

## Related

- [[Prompt]]
- [[context]]
- [[and harness engineering nest rather than replace each other]]

%% ai-graph-start %%

**Related notes:**
- [[Harness Engineering - Designing Reliable AI Systems]]
- [[Prompt, context, and harness engineering nest rather than replace each other]]
- [[A complete skill has five layers - intent, knowledge, execution, verification, evolution]]
- [[A prompt is a temporary instruction, a skill is an encapsulated capability]]
- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]

**Relations:**
- Agent — *IS_COMPOSED_OF* — Model
- Agent — *IS_COMPOSED_OF* — Harness
- Harness — *IS_A* — Engineering discipline
- Harness engineering — *DEFINED_AS* — designing everything around the model
- Harness engineering — *INCLUDES* — Tools
- Harness engineering — *INCLUDES* — Guardrails
- Harness engineering — *INCLUDES* — Feedback Loops
- Harness engineering — *INCLUDES* — Memory
- Harness engineering — *INCLUDES* — State
- Harness engineering — *INCLUDES* — Context Management
- Harness engineering — *INCLUDES* — Security
- Harness engineering — *INCLUDES* — Orchestration
- Harness engineering — *ENSURES* — Agent reliability
- Harness engineering — *COINED_BY* — Mitchell Hashimoto
- Mitchell Hashimoto — *AFFILIATED_WITH* — HashiCorp
- Mitchell Hashimoto — *AFFILIATED_WITH* — Terraform
- Model — *PERFORMS* — Thinking
- Harness — *CONTROLS* — Model's focus
- Model — *IS_ANALOGOUS_TO* — CPU
- Context Window — *IS_ANALOGOUS_TO* — RAM
- Harness — *IS_ANALOGOUS_TO* — Operating System
- Agent — *IS_ANALOGOUS_TO* — Application
- Smarter Model — *IS_ANALOGOUS_TO* — CPU upgrade
- Prompt text — *IS_COMPONENT_OF* — Harness
- Missing guardrail — *CAUSES* — Agent misbehavior
- Absent feedback loop — *CAUSES* — Agent misbehavior
- Unparseable tool output — *CAUSES* — Agent misbehavior
- Failure — *LEADS_TO* — Structural fix
- Structural fix — *INCLUDES* — Validation Layer
- Structural fix — *INCLUDES* — Linter
- Structural fix — *INCLUDES* — Tools
- Harness engineering — *RELATED_TO* — Prompt, context, and harness engineering
- Agent — *RELATED_TO* — ReAct

%% ai-graph-end %%