---
ai_hash: 8133bee37eee674e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-11
entities:
- interrogate-qa skill
- Vinnstack
- vinnstack-skills/interrogate-qa/SKILL.md
- Interrogation Room UI
- business track
- technical track
- db/schema.sql
- questions table
- track CHECK constraint
- schema migration
- QA track
- prompt file
- tooling
- agents
- app
- persistence layer
- standalone tool
- app wiring
- Epics
- Stories
- Flows
source: session 2026-07-11 — creating interrogate-qa skill
status: seedling
tags:
- vinnstack
- ai-first-pipeline
- schema-constraint
- skill-design
title: interrogate-qa skill built standalone due to track CHECK constraint
type: lesson
---

# interrogate-qa skill built standalone due to track CHECK constraint

Vinnstack's new `interrogate-qa` skill (vinnstack-skills/interrogate-qa/SKILL.md) was deliberately built as a standalone, manually-invoked skill rather than wired into the Interrogation Room UI as a third "track" alongside business/technical.

Reason: `db/schema.sql`'s `questions` table has a hard constraint `track TEXT NOT NULL CHECK (track IN ('business','technical'))`. Adding a QA track would need a schema migration (new enum value + UI wiring in the Interrogation Room), which was out of scope for "just create a skill". So the skill exists as a prompt file other tooling/agents can invoke directly, without being surfaced as a tab in the app's Interrogation Room.

General lesson: when a new "kind" of interrogation/question-set is requested but the persistence layer enumerates allowed kinds via a DB CHECK constraint, check that constraint before assuming the new kind can just plug into the existing UI/track system — it may need to ship as a standalone tool first, with app wiring as a separate, explicitly-scoped follow-up.

## Related
[[interrogate-qa is cross-cutting across all Epics, Stories, and Flows]]

## Related

- [[interrogate-qa is cross-cutting across all Epics, Stories, and Flows]]

%% ai-graph-start %%

**Related notes:**
- [[interrogate-qa is cross-cutting across all Epics, Stories, and Flows]]
- [[Technical interrogation questions must pass the 2-minute tech-lead test]]
- [[Vinnstack vinnstack-data-model.html predates the BDD workspace]]
- [[A feedback loop with only its write side wired looks like a broken feature]]
- [[Migrating Vinnstack Interrogation Room from JSON files to normalized Postgres (design)]]

**Relations:**
- interrogate-qa skill — *is part of* — Vinnstack
- interrogate-qa skill — *is located at* — vinnstack-skills/interrogate-qa/SKILL.md
- interrogate-qa skill — *was built as* — standalone, manually-invoked skill
- interrogate-qa skill — *was not wired into* — Interrogation Room UI
- Interrogation Room UI — *includes* — business track
- Interrogation Room UI — *includes* — technical track
- db/schema.sql — *defines* — questions table
- questions table — *has* — track CHECK constraint
- track CHECK constraint — *allows* — business track
- track CHECK constraint — *allows* — technical track
- QA track — *would need* — schema migration
- schema migration — *involves* — new enum value
- schema migration — *involves* — UI wiring
- interrogate-qa skill — *exists as* — prompt file
- prompt file — *can be invoked by* — tooling
- prompt file — *can be invoked by* — agents
- interrogate-qa skill — *is not surfaced as tab in* — app's Interrogation Room
- persistence layer — *enumerates allowed kinds via* — DB CHECK constraint
- standalone tool — *is a prerequisite for* — app wiring
- interrogate-qa skill — *is cross-cutting across* — Epics
- interrogate-qa skill — *is cross-cutting across* — Stories
- interrogate-qa skill — *is cross-cutting across* — Flows

%% ai-graph-end %%