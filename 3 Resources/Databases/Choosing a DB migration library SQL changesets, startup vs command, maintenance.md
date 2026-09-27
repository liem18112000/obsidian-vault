---
ai_hash: 054691c9603ad2d0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Routing tool Database replacement NodeJS Investigation (Arrow)'
status: seedling
tags:
- migrations
- postgres
- nodejs
- tooling
- library-selection
- confluence-distilled
title: 'Choosing a DB migration library: SQL changesets, startup vs command, maintenance'
type: lesson
---

# Choosing a DB migration library: SQL changesets, startup vs command, maintenance

Picking a database-migration library looks like a popularity contest and is actually three questions. An evaluation of the Node/Postgres options (`node-pg-migrate`, `postgres-migrations`, `db-migrate`) surfaced the axes that mattered — none of which was stars or downloads.

**1. Can changesets be plain SQL, or only code?**
`node-pg-migrate` expects migrations written in JavaScript; `postgres-migrations` and `db-migrate` accept **both SQL and JS**. This matters more than it sounds: a DBA reviewing a schema change should not have to read a JS builder DSL, and the SQL you tested in a console should be the SQL that ships. Code-only changesets also make the migration's behaviour dependent on the library version at run time.

**2. Does it run at service startup, or as a separate command?**
`postgres-migrations` runs **at startup out of the box**. The other two need `npm run migrate up` / `db-migrate up` as a distinct step. That distinction decides your deployment topology:

- *Startup migrations* — simple, nothing extra to orchestrate. But every replica races to migrate on boot, so the library must take a lock, and a bad migration becomes a crash-loop rather than a failed job.
- *Separate command* — needs a Kubernetes Job, init container, or pipeline step, but the migration succeeds or fails **before** any new pod serves traffic, and you can run it once rather than per-replica.

The evaluation notes the friction honestly: the separate-command options could be handled via Docker, *"but Routing Tool on Docker yet"* — i.e. the infrastructure to run that step did not exist yet. Which mechanism suits you depends on infrastructure you already have, not on which is nicer in the abstract.

**3. Is it maintained?**
`node-pg-migrate` updated last month; `db-migrate` four months ago; `postgres-migrations` **three years ago**. A dormant migration library is a real risk — this is the component that must keep working against new Postgres versions and that you cannot easily swap once it owns your migration history table.

> [!tip] The uncomfortable shape of this decision
> The library with the best *features* (SQL changesets **and** startup execution) was the least *maintained*. That trade recurs constantly in library selection, and the resolution depends on how much surface you are adopting: for something small and stable, an unmaintained library you could vendor or replace is acceptable; for something owning critical state, prefer maintained.

> [!note] The ORM question is separate
> The same investigation lists Sequelize and Prisma as the Hibernate equivalents. Keep migration tooling and ORM as independent choices — coupling them means switching one forces switching the other.

Source: [[Routing tool Database replacement NodeJS - Investigation]] (Arrow, Confluence).

%% ai-graph-start %%

**Related notes:**
- [[Routing tool Database replacement NodeJS - Investigation]]
- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]
- [[A self-contained migration parity test is only useful during the cutover]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]

%% ai-graph-end %%