---
ai_hash: 948c7fd362188918
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- prompt
- skill
- skill system
- AI well-usage
- context window
- intent
- knowledge
- executable artifacts
- verification criteria
- ask → answer → tweak → retry loop
- task
- operational asset
- failure
- complete skill
- execution
- evolution
- Agent
- model
- harness
- person
- organisation
- temporary instruction
- encapsulated capability
- operational memory
- one exchange
- reusable
- short, isolated, low-risk work
- multiple steps
- multiple data sources
- multiple correctness criteria
- multiple people
- prompt wording
- nothing captured
- every run re-derives context
- every teammate re-derives context separately
- prompt trick
- versioned
- tested
- improved
- each time
- lessons evaporate
- engineering discipline
source: 'Confluence: From Prompt-Based Usage to Skill-Based Execution (2026-03-19)'
status: seedling
tags:
- ai-agents
- skills
- prompt-engineering
- knowledge-management
- llm
title: A prompt is a temporary instruction, a skill is an encapsulated capability
type: concept
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

%% ai-graph-start %%

**Related notes:**
- [[A complete skill has five layers - intent, knowledge, execution, verification, evolution]]
- [[From Prompt-Based Usage to Skill-Based Execution]]
- [[Prompt, context, and harness engineering nest rather than replace each other]]
- [[Extract reusable skills automatically from settled agent exchanges]]
- [[Agent skeleton = Instruction + Skills-Resources + Tools + Context]]

**Relations:**
- prompt — *IS_A* — temporary instruction
- skill — *IS_A* — encapsulated capability
- skill system — *IS_A* — operational memory
- skill system — *IS_OPERATIONAL_MEMORY_OF* — person
- skill system — *IS_OPERATIONAL_MEMORY_OF* — organisation
- AI well-usage — *INCLUDES* — prompt
- AI well-usage — *INCLUDES* — skill
- AI well-usage — *INCLUDES* — skill system
- prompt — *EXISTS_FOR* — one exchange
- prompt — *DIES_WITH* — context window
- skill — *INCLUDES* — intent
- skill — *INCLUDES* — knowledge
- skill — *INCLUDES* — executable artifacts
- skill — *INCLUDES* — verification criteria
- skill — *HAS_PROPERTY* — reusable
- skill system — *IS_ACCUMULATED_SET_OF* — skill
- ask → answer → tweak → retry loop — *IS_SUITABLE_FOR* — short, isolated, low-risk work
- ask → answer → tweak → retry loop — *STOPS_SCALING_WHEN* — task HAS multiple steps
- ask → answer → tweak → retry loop — *STOPS_SCALING_WHEN* — task HAS multiple data sources
- ask → answer → tweak → retry loop — *STOPS_SCALING_WHEN* — task HAS multiple correctness criteria
- ask → answer → tweak → retry loop — *STOPS_SCALING_WHEN* — task REQUIRES multiple people
- failure — *IS_NOT_DUE_TO* — prompt wording
- failure — *IS_DUE_TO* — nothing captured
- nothing captured — *LEADS_TO* — every run re-derives context
- nothing captured — *LEADS_TO* — every teammate re-derives context separately
- skill — *IS_A* — operational asset
- skill — *IS_NOT* — prompt trick
- operational asset — *IS* — versioned
- operational asset — *IS* — tested
- operational asset — *IS* — improved
- operational asset — *IS_IMPROVED_AFTER* — failure
- prompt — *IS_REWRITTEN_FROM_SCRATCH* — each time
- prompt — *CAUSES* — lessons evaporate
- complete skill — *HAS_LAYER* — intent
- complete skill — *HAS_LAYER* — knowledge
- complete skill — *HAS_LAYER* — execution
- complete skill — *HAS_LAYER* — verification
- complete skill — *HAS_LAYER* — evolution
- Agent — *EQUALS* — model PLUS harness
- harness — *IS_A* — engineering discipline

%% ai-graph-end %%