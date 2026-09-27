---
title: "A complete skill has five layers - intent, knowledge, execution, verification, evolution"
created: 2026-09-27
type: model
status: seedling
source: "Confluence: From Prompt-Based Usage to Skill-Based Execution (2026-03-19)"
tags: [ai-agents, skills, claude-skills, knowledge-management, llm]
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
