---
ai_hash: 2f7623b1da165740
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Prompt engineering
- Context engineering
- Harness engineering
- Single message
- Context window
- Running system
- Retrieval
- History compaction
- File selection
- Tool results
- Tool
- Feedback
- Orchestration
- Agent
- Model
- Instruction
- Prompt problem
- Context problem
- Harness problem
- System prompts
- Validation layer
- Linter
- Permission gate
source: 'Confluence: Harness Engineering - Designing Reliable AI Systems (2026-04-06)'
status: seedling
tags:
- ai-agents
- harness-engineering
- prompt-engineering
- context-engineering
- llm
title: Prompt, context, and harness engineering nest rather than replace each other
type: model
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

%% ai-graph-start %%

**Related notes:**
- [[Harness Engineering - Designing Reliable AI Systems]]
- [[Agent equals model plus harness, and the harness is the engineering discipline]]
- [[A prompt is a temporary instruction, a skill is an encapsulated capability]]
- [[A complete skill has five layers - intent, knowledge, execution, verification, evolution]]
- [[Multi-agent systems trade a single capable agent for specialisation, isolation and parallelism]]

**Relations:**
- Prompt engineering — *is nested within* — Context engineering
- Context engineering — *is nested within* — Harness engineering
- Prompt engineering — *has scope* — Single message
- Context engineering — *has scope* — Context window
- Harness engineering — *has scope* — Running system
- Context engineering — *includes* — Retrieval
- Context engineering — *includes* — History compaction
- Context engineering — *includes* — File selection
- Context engineering — *includes* — Tool results
- Harness engineering — *defines* — Tool
- Harness engineering — *defines* — Feedback
- Harness engineering — *defines* — Orchestration
- Agent — *misreads* — Instruction
- Agent misreads Instruction — *is a* — Prompt problem
- Agent — *misses* — Relevant file
- Agent misses Relevant file — *is a* — Context problem
- Agent — *causes destruction* — Harness problem
- Harness problem — *is mistaken for* — Prompt problem
- Prompt problem — *produces* — System prompts
- Harness problem — *can be fixed by* — Validation layer
- Harness problem — *can be fixed by* — Linter
- Harness engineering — *manages* — Permission gate
- Agent — *is composed of* — Model
- Agent — *is composed of* — Harness engineering

%% ai-graph-end %%