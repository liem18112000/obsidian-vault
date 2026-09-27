---
ai_hash: 6b61a6c4661a420a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Skills
- Commands
- Playbooks
- Agent exchanges
- Automatic extraction
- Skill library
- Server-driven extraction pass
- SKILL.md
- Agent
- Classifier problem
- Model (cheap fast)
- Settled exchanges
- Knowledge
- Surrounding system
- Session insights
- Daily transcripts
- Synthesised notes
- Backlinks
- Existing vault
- Stub notes
- New topics
- Knowledge base
- Versioning
- Human review
- Automation
- Judgement of quality
- Agent CLI
- Engine
- Surface (software component)
- Vinnstack vs. Claude Code (native)
- Wrap the agent CLI
- Reusable procedure
- Procedure
- Artefacts (auto-generated)
- Provenance
- System
- Judging step
- Capture
source: 'Confluence: Vinnstack vs Claude Code native (TK)'
status: seedling
tags:
- agents
- skills
- knowledge-capture
- self-improvement
- claude-code
- confluence-distilled
title: Extract reusable skills automatically from settled agent exchanges
type: lesson
---

# Extract reusable skills automatically from settled agent exchanges

Skills, commands and playbooks only exist if someone stops to author them — so in practice they lag far behind what the team actually does repeatedly. Make the extraction **automatic** and the library grows from real work instead of from good intentions.

The mechanism:

> A server-driven extraction pass **judges each settled exchange** for a genuinely reusable procedure and writes a versioned `SKILL.md` when it finds one. The agent is also instructed to **consult the skill library before tasks** — so the system gets measurably better at recurring work without anyone maintaining it.

**Three design choices doing the work here:**

1. **Judge, don't capture everything.** An extractor that writes a skill per session produces noise that makes the library worse than empty. A judging step — is there a *genuinely reusable procedure* here? — is what keeps precision up. This is a classifier problem, and a good place for a cheap fast model rather than the main agent.
2. **Only settled exchanges.** Extracting mid-conversation captures the false starts. Waiting until an exchange has resolved means you capture the procedure that *worked*, not the three that did not.
3. **Close the loop by consulting before acting.** Extraction alone builds a library nobody reads. Instructing the agent to check the library first is what converts stored skills into changed behaviour — and it is the half most easily forgotten.

**The same principle applies to knowledge, not just procedures.** The surrounding system captures session insights structurally — daily transcripts, idle-triggered synthesised notes (decisions, action items, open questions) with **backlinks into the existing vault**, and stub notes for new topics. The point is identical: *a working session should leave behind an organised, linked knowledge base as a side effect*, not as a discipline someone has to remember at 6pm.

> [!tip] Version the extracted skill
> Writing a **versioned** `SKILL.md` means a later, better extraction can supersede an earlier one without silently rewriting history — and you can tell whether a regression came from a skill change. Auto-generated artefacts need provenance more than hand-written ones, not less.

> [!warning] Auto-extracted skills encode whatever you actually did, including the wrong things
> If the team routinely does something badly, extraction will faithfully capture the bad procedure and then instruct future agents to follow it. The library needs occasional human review — automation is for the *capture*, not for the *judgement of quality*.

Related: [[Wrap the agent CLI rather than reimplementing the agent loop]] — extraction is one of the things the surface adds around the engine.

Source: [[Vinnstack vs. Claude Code (native)]] (TK, Confluence).

## Related

- [[Wrap the agent CLI rather than reimplementing the agent loop]]

%% ai-graph-start %%

**Related notes:**
- [[Wrap the agent CLI rather than reimplementing the agent loop]]
- [[A prompt is a temporary instruction, a skill is an encapsulated capability]]
- [[Vinnstack vs. Claude Code (native)]]
- [[A complete skill has five layers - intent, knowledge, execution, verification, evolution]]
- [[From Prompt-Based Usage to Skill-Based Execution]]

**Relations:**
- Automatic extraction — *grows* — Skill library
- Server-driven extraction pass — *judges* — Settled exchanges
- Server-driven extraction pass — *creates* — SKILL.md
- SKILL.md — *is* — versioned
- Agent — *consults* — Skill library
- Consulting skill library — *improves* — System
- Judging step — *is a* — Classifier problem
- Classifier problem — *uses* — Model (cheap fast)
- Settled exchanges — *capture* — Procedure
- Automatic extraction — *applies to* — Knowledge
- Surrounding system — *captures* — Session insights
- Synthesised notes — *include* — Backlinks
- Backlinks — *link to* — Existing vault
- Working session — *generates* — Knowledge base
- Versioning — *enables* — superseding skills
- Artefacts (auto-generated) — *require* — Provenance
- Auto-extracted skills — *encode* — Procedure
- Skill library — *requires* — Human review
- Automation — *is for* — Capture
- Automation — *is not for* — Judgement of quality
- Extraction — *is a function of* — Surface (software component)
- Surface (software component) — *wraps* — Engine
- Vinnstack vs. Claude Code (native) — *is source for* — Automatic extraction
- Wrap the agent CLI — *is related to* — Automatic extraction
- Skills — *are a type of* — Reusable procedure
- Commands — *are a type of* — Reusable procedure
- Playbooks — *are a type of* — Reusable procedure
- Surface (software component) — *adds* — Extraction

%% ai-graph-end %%