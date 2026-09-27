---
ai_hash: 9a48432541909c37
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-02
entities:
- Vinnstack Interrogation Room
- JSON files
- Interrogation (data structure)
- lib/interrogationStore.ts
- epic
- questions
- options
- answers
- visuals
- prd
- revisions
- stories
- flows
- PostgreSQL
- DB-only (persistence strategy)
- Sync (programming paradigm)
- Async (programming paradigm)
- tsc (TypeScript compiler)
- app/api/interrogation/route.ts
- lib/interrogationRunner.ts
- chat agent
- ultracodeRunner
- VAULT_DIR
- DATABASE_URL
- Docker
- postgres:16 (Docker image)
- db/schema.sql
- doc/interrogation-persistence-plan.md
- Vinnstack auth providers
- Vinnstack
source: session 2026-07-02
status: seedling
tags:
- vinnstack
- postgres
- persistence
- migration
- interrogation-room
title: Migrating Vinnstack Interrogation Room from JSON files to normalized Postgres
  (design)
type: argument
---

# Migrating Vinnstack Interrogation Room from JSON files to normalized Postgres (design)

Vinnstack's Interrogation Room store (lib/interrogationStore.ts) is **aggregate-oriented**: the whole `Interrogation` (epic + questions + options + answers + visuals + prd + revisions + stories + flows) is always loaded whole, mutated in memory, and saved whole. Persistence was one JSON file per epic + a Markdown mirror the chat agent reads + git auto-commit.

Decision (2026-07-02, branch feature/interrogation-persistence): move to **PostgreSQL, fully normalized, DB-only** (drop the .json + .md files).

Key implications captured for the implementation:
- **Sync → async is all-or-nothing.** The store fns are synchronous; Postgres makes them async, which is a type-level breaking change — do it in ONE pass and let `tsc` enumerate every caller. (Direct importers were few: app/api/interrogation/route.ts + lib/interrogationRunner.ts.)
- **Normalized + aggregate access = delete-and-reinsert children inside one transaction** on each save (or upsert). Simple + correct for load-whole/save-whole; revisit only if records grow large.
- **"DB-only" fights the chat agent's file context.** The agent reads <epic>.md via ultracodeRunner's --add-dir VAULT_DIR. Dropping the mirror means re-pointing it — cleanest is to keep regenerating <epic>.md as a READ-ONLY projection from the DB (DB stays source of truth) rather than going fully fileless.
- **Runtime prerequisite:** DB-only means the feature is dead until a reachable Postgres + DATABASE_URL exists. None on the dev machine — provision Docker postgres:16 first.

Schema + full plan live in the repo: db/schema.sql and doc/interrogation-persistence-plan.md.

## Related

- [[Vinnstack auth providers two patterns and the rule for adding one]]

%% ai-graph-start %%

**Related notes:**
- [[Vinnstack interrogationStore full-aggregate rewrite loses concurrent updates to the same epic]]
- [[Batch multi-row INSERTs to cut round-trips on aggregate saves (Postgres)]]
- [[DB-first-with-file-fallback opt-in Postgres persistence over a file cache]]
- [[Per-key write lock for parallel aggregate writes; self-migrating column via idempotent ALTER]]
- [[Version artifacts by lifecycle event with content-dedupe, store in DB not files]]

**Relations:**
- Vinnstack Interrogation Room — *persists with* — JSON files
- Vinnstack Interrogation Room — *is* — aggregate-oriented
- lib/interrogationStore.ts — *manages data for* — Vinnstack Interrogation Room
- Interrogation (data structure) — *comprises* — epic
- Interrogation (data structure) — *comprises* — questions
- Interrogation (data structure) — *comprises* — options
- Interrogation (data structure) — *comprises* — answers
- Interrogation (data structure) — *comprises* — visuals
- Interrogation (data structure) — *comprises* — prd
- Interrogation (data structure) — *comprises* — revisions
- Interrogation (data structure) — *comprises* — stories
- Interrogation (data structure) — *comprises* — flows
- Vinnstack Interrogation Room — *will migrate to* — PostgreSQL
- PostgreSQL — *will be* — fully normalized
- PostgreSQL — *will be* — DB-only (persistence strategy)
- lib/interrogationStore.ts — *will change from* — Sync (programming paradigm)
- lib/interrogationStore.ts — *will change to* — Async (programming paradigm)
- tsc (TypeScript compiler) — *identifies breaking changes in* — lib/interrogationStore.ts
- app/api/interrogation/route.ts — *calls functions in* — lib/interrogationStore.ts
- lib/interrogationRunner.ts — *calls functions in* — lib/interrogationStore.ts
- DB-only (persistence strategy) — *conflicts with* — chat agent's file context
- chat agent — *reads* — <epic>.md
- ultracodeRunner — *uses directory* — VAULT_DIR
- <epic>.md — *will be a projection from* — PostgreSQL
- PostgreSQL — *is* — source of truth
- DB-only (persistence strategy) — *requires* — PostgreSQL
- PostgreSQL — *can be provisioned with* — Docker
- Docker — *provides image* — postgres:16 (Docker image)
- DATABASE_URL — *is a prerequisite for* — DB-only (persistence strategy)
- db/schema.sql — *defines schema for* — PostgreSQL
- doc/interrogation-persistence-plan.md — *details plan for* — migration
- Vinnstack auth providers — *is related to* — Vinnstack
- Vinnstack Interrogation Room — *migrates from* — JSON files
- Vinnstack Interrogation Room — *migrates to* — PostgreSQL

%% ai-graph-end %%