---
ai_hash: 6060ccc031a27b27
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Skill
- Intent
- Knowledge
- Execution
- Verification
- Evolution
- Prompt
- Instructions
- Problem
- Correctness criteria
- Permitted scope
- Domain rules
- API references
- Style guides
- Templates
- Internal conventions
- Scripts
- Commands
- Workflows
- Sample queries
- Scaffolds
- Checklists
- Output
- Failures
- System
- Documentation
- Harness engineering
- Agent
- Model
- Harness
- Engineering discipline
- Capability
source: 'Confluence: From Prompt-Based Usage to Skill-Based Execution (2026-03-19)'
status: seedling
tags:
- ai-agents
- skills
- claude-skills
- knowledge-management
- llm
title: A complete skill has five layers - intent, knowledge, execution, verification,
  evolution
type: model
---

# A complete skill has five layers - intent, knowledge, execution, verification, evolution

A skill that is only instructions is a long prompt. A complete one carries five layers, and skipping any of them produces a recognisable failure:

1. **Intent** — the problem, what "done" means, the correctness criteria, the permitted scope. *Skip it and the agent produces output that looks right but answers a different question.*
2. **Knowledge** — domain rules, API references, style guides, templates, internal conventions. *Skip it and the agent invents plausible defaults.*
3. **Execution** — the directly runnable artifacts: scripts, commands, workflows, sample queries, scaffolds, checklists. **This is the layer most often missing.** A skill that describes *how* without shipping the *thing* leaves the agent re-deriving the same script every run, differently each time.
4. **Verification** — how to check the output before declaring completion. *Skip it and errors surface downstream, where they cost most.*
5. **Evolution** — how failures feed back into the skill. *Skip it and the same mistake recurs forever.*

Layer 5 is what makes this a system rather than documentation, and it is the same ratchet as [[Agent equals model plus harness, and the harness is the engineering discipline|harness engineering]]: each failure becomes a permanent structural fix instead of a one-off correction.

Reading the layers as a diagnostic is the useful move — when a skill underperforms, identify which layer was thin rather than rewriting the prose.

## Related

- [[A prompt is a temporary instruction, a skill is an encapsulated capability]]
- [[Agent equals model plus harness, and the harness is the engineering discipline]]

## Related

- [[A prompt is a temporary instruction, a skill is an encapsulated capability]]

%% ai-graph-start %%

**Related notes:**
- [[A prompt is a temporary instruction, a skill is an encapsulated capability]]
- [[From Prompt-Based Usage to Skill-Based Execution]]
- [[Prompt, context, and harness engineering nest rather than replace each other]]
- [[Agent skeleton = Instruction + Skills-Resources + Tools + Context]]
- [[Extract reusable skills automatically from settled agent exchanges]]

**Relations:**
- Skill — *HAS_LAYER* — Intent
- Skill — *HAS_LAYER* — Knowledge
- Skill — *HAS_LAYER* — Execution
- Skill — *HAS_LAYER* — Verification
- Skill — *HAS_LAYER* — Evolution
- Skill — *IS_A* — Prompt
- Intent — *DEFINES* — Problem
- Intent — *DEFINES* — Correctness criteria
- Intent — *DEFINES* — Permitted scope
- Knowledge — *INCLUDES* — Domain rules
- Knowledge — *INCLUDES* — API references
- Knowledge — *INCLUDES* — Style guides
- Knowledge — *INCLUDES* — Templates
- Knowledge — *INCLUDES* — Internal conventions
- Execution — *INCLUDES* — Scripts
- Execution — *INCLUDES* — Commands
- Execution — *INCLUDES* — Workflows
- Execution — *INCLUDES* — Sample queries
- Execution — *INCLUDES* — Scaffolds
- Execution — *INCLUDES* — Checklists
- Verification — *CHECKS* — Output
- Evolution — *PROCESSES* — Failures
- Failures — *FEED_BACK_INTO* — Skill
- Evolution — *MAKES_SKILL_A* — System
- Evolution — *DISTINGUISHES_FROM* — Documentation
- Evolution — *IS_ANALOGOUS_TO* — Harness engineering
- Agent — *EQUALS* — Model
- Agent — *EQUALS* — Harness
- Harness — *IS_A* — Engineering discipline
- Prompt — *IS_A* — Instruction
- Skill — *IS_A* — Capability

%% ai-graph-end %%