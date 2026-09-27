---
ai_hash: e847a923a6a34159
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-19
entities: []
source: leo-customer360 SCRUM-102 code review 2026-09-19
status: seedling
tags:
- sql
- postgres
- migrations
- schema
- gotcha
title: CREATE TABLE IF NOT EXISTS cannot express a rename
type: lesson
---

# CREATE TABLE IF NOT EXISTS cannot express a rename

A declarative schema file that only issues `CREATE TABLE IF NOT EXISTS` provides **no upgrade path for a table or column RENAME**. Re-running it against an existing DB creates a fresh *empty* table under the new name (IF NOT EXISTS finds nothing to skip-and-fill) while the old table, its rows, and inbound FK references are left stranded. A rename is inherently *stateful* — it cannot be expressed through an idempotent CREATE.

## Why it bites
"Idempotent schema" lulls you into thinking re-applying it upgrades any DB. That holds for *adding* objects, not for *transforming* existing ones. Rename, retype, split, and merge all need an explicit migration that inspects and mutates current state.

## Consequence pattern
- Fresh installs: fine (new table created correctly).
- Existing/mid-upgrade DBs: silent data loss — new empty table shadows the populated old one; FKs pointing at the old table dangle; lookups return nothing.

## Real instance
leo-customer360 renamed `crm_email_templates` -> `crm_message_templates`, deleted the in-place rename migration, and left only `CREATE TABLE IF NOT EXISTS crm_message_templates` in `database-schema.sql`. Any DB provisioned before the rename orphans all templates on redeploy.

## Fix pattern
Keep an explicit idempotent migration for the transform: `ALTER TABLE ... RENAME TO ...` (guarded by an existence check), plus `ADD COLUMN IF NOT EXISTS` for new columns. Never rely on CREATE to move data between names.

## Related

- [[Never sign a security token with a secret documented as unused]]

%% ai-graph-start %%

**Related notes:**
- [[CREATE TABLE IF NOT EXISTS never upgrades existing tables - pair new columns with ALTER IF NOT EXISTS]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[Per-feature migration scripts leave new tables silently missing until run]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]

%% ai-graph-end %%