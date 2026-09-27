---
title: "Agent equals model plus harness, and the harness is the engineering discipline"
created: 2026-09-27
type: term
status: seedling
source: "Confluence: Harness Engineering - Designing Reliable AI Systems (2026-04-06)"
tags: [ai-agents, harness-engineering, architecture, reliability, llm]
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
