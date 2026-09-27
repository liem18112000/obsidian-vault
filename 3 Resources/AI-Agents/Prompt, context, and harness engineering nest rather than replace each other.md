---
title: "Prompt, context, and harness engineering nest rather than replace each other"
created: 2026-09-27
type: model
status: seedling
source: "Confluence: Harness Engineering - Designing Reliable AI Systems (2026-04-06)"
tags: [ai-agents, harness-engineering, prompt-engineering, context-engineering, llm]
---

# Prompt, context, and harness engineering nest rather than replace each other

These three are usually presented as a progression where each replaces the last. They **nest** instead — each is a strictly larger scope containing the previous one.

- **Prompt engineering** — what you say in one turn. Scope: a single message.
- **Context engineering** — what the model can see at that turn: retrieval, history compaction, file selection, tool results. Scope: the context window.
- **Harness engineering** — everything around the model across turns and sessions: which tools exist at all, what is forbidden, what feedback returns, what persists, who orchestrates. Scope: the running system.

Why the distinction earns its keep: **it tells you where to fix a failure.** An agent that misreads an instruction is a prompt problem. An agent that had the right instruction but never saw the relevant file is a context problem. An agent that did exactly what it was told and destroyed something is a harness problem — no prompt wording fixes a missing permission gate.

The common mistake is treating a harness failure as a prompt failure, which produces increasingly baroque system prompts that beg the model to be careful. The structural version — remove the tool, add the validation layer, put a linter in the loop — is smaller and does not regress when the model changes.

## Related

- [[Agent equals model plus harness, and the harness is the engineering discipline]]

## Related

- [[Agent equals model plus harness, and the harness is the engineering discipline]]
