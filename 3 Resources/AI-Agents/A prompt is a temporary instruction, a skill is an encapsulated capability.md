---
title: "A prompt is a temporary instruction, a skill is an encapsulated capability"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: From Prompt-Based Usage to Skill-Based Execution (2026-03-19)"
tags: [ai-agents, skills, prompt-engineering, knowledge-management, llm]
---

# A prompt is a temporary instruction, a skill is an encapsulated capability

Three things get called "using AI well", and collapsing them hides where the leverage is:

- A **prompt** is a *temporary instruction*. It exists for one exchange and dies with the context window.
- A **skill** is an *encapsulated capability* — intent, the knowledge needed, executable artifacts, verification criteria, all packaged and reusable.
- A **skill system** is the *operational memory* of a person or organisation — the accumulated set of skills, which is what actually compounds.

The ask → answer → tweak → retry loop is fine for short, isolated, low-risk work. It stops scaling the moment a task has **multiple steps, multiple data sources, multiple correctness criteria, or more than one person who needs it to work the same way.** At that point the failure is not prompt wording — it is that nothing was captured, so every run re-derives the same context and every teammate re-derives it separately.

The mindset shift that matters: **a skill is an operational asset, not a prompt trick.** Assets are versioned, tested, and improved after each failure. Prompts are rewritten from scratch each time and their lessons evaporate.

The practical test for whether something should become a skill: *would I be annoyed to explain this context again next week?* If yes, it is an asset — write it down as one.

## Related

- [[A complete skill has five layers - intent, knowledge, execution, verification, evolution]]
- [[Agent equals model plus harness, and the harness is the engineering discipline]]

## Related

- [[A complete skill has five layers - intent]]
- [[knowledge]]
- [[execution]]
- [[verification]]
- [[evolution]]
