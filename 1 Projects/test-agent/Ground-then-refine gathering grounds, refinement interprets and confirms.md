---
ai_hash: 9cd8711619d0876d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities:
- Ground-then-refine
- AI agent
- grounding
- interpretation
- human
- Stage 1
- Gather
- read-only crawl
- sources
- context pack
- curated notes
- provenance
- link graph
- declared gaps
- Stage 2
- Refine
- questions
- business questions
- technical questions
- QA rounds
- assumptions
- judgement calls
- options
- recommendation
- insight note
- memory bank
- understanding
- loop
- missing source
- gather seed
- confidence bar
- round budget
- leftover questions
- gaps
- downstream planner
- grounded facts
- settled decisions
- confirmed understanding
- vinnstack
- interrogate-business
- interrogate-technical
- interrogate-qa
- round engines
- dont ask what you can answer rule
- altitude rules
- test-agent Testing Agent
- Step 1 (Knowledge Gathering)
- Step 2 (Knowledge Refinement)
- LUZ-159671
- A2A multi-turn human-in-the-loop via input-required Task state
source: session 2026-08-27 test-agent
status: seedling
tags:
- agentic-workflow
- qa-agent
- context-engineering
- interrogation
- test-agent
title: 'Ground-then-refine: gathering grounds, refinement interprets and confirms'
type: model
---

# Ground-then-refine: gathering grounds, refinement interprets and confirms

A robust two-stage pattern for feeding an AI agent trustworthy context: separate **grounding** from **interpretation**, and put a human between them.

**Stage 1 — Gather (grounds).** Read-only crawl of the sources (Jira/Confluence/links/code/logs) into a distilled **context pack**: curated notes with provenance, a link graph, and *declared gaps*. Rule: **invent nothing, drop nothing silently** — an out-of-scope link is recorded, a broken one flagged, an unreachable node becomes a gap row, never an absent one. Grounding does NOT interpret.

**Stage 2 — Refine (interprets & confirms).** Turn the pack into a small, ranked set of **questions** (business → technical → QA rounds), **self-answering everything derivable** (recorded as vetoable assumptions) and surfacing only genuine judgement calls with options + a recommendation. The human answers; each confirmed answer is distilled into a **provenance-carrying "insight" note** (who answered, when, confidence) written back to the memory bank. The agent then **restates its understanding** in plain language for the human to confirm/correct.

The two stages form a **loop**: an answer that names a missing source becomes a new gather seed → re-ground → re-interrogate; terminate on a confidence bar / round budget, leftover questions declared as gaps. Net effect: the downstream planner reads *grounded facts + settled decisions + a confirmed understanding*, instead of guessing at the forks a raw crawl leaves open.

This maps onto the vinnstack `interrogate-business` / `interrogate-technical` / `interrogate-qa` skills as the three round engines, with the "dont ask what you can answer" and altitude rules carried over.

Source: test-agent Testing Agent, Step 1 (Knowledge Gathering) → Step 2 (Knowledge Refinement), LUZ-159671.

## Related

- [[A2A multi-turn human-in-the-loop via input-required Task state]]

%% ai-graph-start %%

**Related notes:**
- [[A2A multi-turn human-in-the-loop via input-required Task state]]
- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]
- [[Testing Agent workflow step to AI Skill mapping]]
- [[Pipeline stages sharing a context_id need separate memory-bank path prefixes]]

**Relations:**
- Ground-then-refine — *is a pattern for* — feeding an AI agent trustworthy context
- Ground-then-refine — *separates* — grounding
- Ground-then-refine — *separates* — interpretation
- Ground-then-refine — *involves* — human
- human — *is placed between* — grounding
- human — *is placed between* — interpretation
- Ground-then-refine — *has* — Stage 1
- Ground-then-refine — *has* — Stage 2
- Stage 1 — *is also known as* — Gather
- Stage 1 — *is also known as* — grounding
- Stage 2 — *is also known as* — Refine
- Stage 2 — *is also known as* — interpretation
- Gather — *performs* — read-only crawl
- read-only crawl — *of* — sources
- Gather — *produces* — context pack
- context pack — *contains* — curated notes
- curated notes — *have* — provenance
- context pack — *contains* — link graph
- context pack — *contains* — declared gaps
- grounding — *does not* — interpret
- Refine — *transforms* — context pack
- Refine — *generates* — questions
- questions — *are* — ranked
- questions — *include* — business questions
- questions — *include* — technical questions
- questions — *include* — QA rounds
- Refine — *self-answers* — derivable items
- derivable items — *are recorded as* — assumptions
- Refine — *surfaces* — judgement calls
- judgement calls — *include* — options
- judgement calls — *include* — recommendation
- human — *answers* — judgement calls
- confirmed answer — *is distilled into* — insight note
- insight note — *carries* — provenance
- insight note — *is written to* — memory bank
- AI agent — *restates* — understanding
- human — *confirms or corrects* — understanding
- Stage 1 — *and Stage 2 form a* — loop
- loop — *is triggered by* — missing source
- missing source — *becomes a* — gather seed
- loop — *terminates on* — confidence bar
- loop — *terminates on* — round budget
- leftover questions — *are declared as* — gaps
- downstream planner — *reads* — grounded facts
- downstream planner — *reads* — settled decisions
- downstream planner — *reads* — confirmed understanding
- Ground-then-refine — *maps onto* — vinnstack
- vinnstack — *has skill* — interrogate-business
- vinnstack — *has skill* — interrogate-technical
- vinnstack — *has skill* — interrogate-qa
- interrogate-business — *is a type of* — round engines
- interrogate-technical — *is a type of* — round engines
- interrogate-qa — *is a type of* — round engines
- vinnstack — *carries over* — dont ask what you can answer rule
- vinnstack — *carries over* — altitude rules
- Ground-then-refine — *is described in* — test-agent Testing Agent
- test-agent Testing Agent — *details* — Step 1 (Knowledge Gathering)
- test-agent Testing Agent — *details* — Step 2 (Knowledge Refinement)
- Step 1 (Knowledge Gathering) — *is equivalent to* — Stage 1
- Step 2 (Knowledge Refinement) — *is equivalent to* — Stage 2
- Ground-then-refine — *is associated with* — LUZ-159671
- Ground-then-refine — *is related to* — A2A multi-turn human-in-the-loop via input-required Task state

%% ai-graph-end %%