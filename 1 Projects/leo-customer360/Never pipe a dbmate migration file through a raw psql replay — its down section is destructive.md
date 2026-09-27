---
ai_hash: 51b0414d7349ced0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities:
- dbmate
- psql
- leo-customer360 repo
- schema-apply mechanisms
- docker-compose migrate service
- dbmate migration files
- schema_migrations
- -- migrate:up
- -- migrate:down
- DROP SCHEMA IF EXISTS customer360 CASCADE;
- run-sql.sh
- Terraform
- bastion
- UAT
- prod
- blind replay
- SQL files
- postgres/init
- database-init/*.sql
- database-init/migrations/*
- version tracking
- IF NOT EXISTS
- ON CONFLICT
- SQL comment
- database wipe
- markers
- dbmate binary/container
- psql-client
- old hand-rolled migrations
- RLS hardening
- Alembic
- Postgres migrations
- APP_SQL_DIR
- MIGRATIONS_DIR
source: session 2026-08-24 dbmate implementation
status: seedling
tags:
- dbmate
- migrations
- gotcha
- prod-safety
- leo-customer360
title: Never pipe a dbmate migration file through a raw psql replay — its down section
  is destructive
type: lesson
---

# Never pipe a dbmate migration file through a raw psql replay — its down section is destructive

In the leo-customer360 repo, two schema-apply mechanisms coexist and are **mutually incompatible**:

- **dbmate** (docker-compose `migrate` service): applies `database-init/db/migrations/*.sql`, tracking versions in `schema_migrations`. Each file has a `-- migrate:up` and a `-- migrate:down` half; the baseline's down is `DROP SCHEMA IF EXISTS customer360 CASCADE;`.
- **`deployments/postgres/run-sql.sh`** (Terraform + bastion, UAT/prod): a **blind replay** that pipes every `*.sql` it finds (postgres/init, database-init/*.sql, database-init/migrations/*) straight into `psql -f -` over SSH. No version tracking; relies on IF NOT EXISTS / ON CONFLICT idempotency.

**The trap:** if `run-sql.sh` ever collects a *dbmate* migration file and pipes it to psql, psql runs the WHOLE file — including the `-- migrate:down` section. `-- migrate:down` is just a SQL comment to psql, so `DROP SCHEMA customer360 CASCADE` executes and **wipes the database**. dbmate only runs one half because it parses the markers; raw psql does not.

**Rules:**
- Never point a raw psql replay (run-sql.sh's `APP_SQL_DIR`/`MIGRATIONS_DIR`) at `database-init/db/migrations`.
- Migrating the prod/bastion path to dbmate means running the dbmate binary/container there (single static binary, installable like they install psql-client), NOT feeding migration files to psql.
- Until that follow-up lands, keep `database-init/migrations/001_*.sql` (old hand-rolled, safe to replay) so run-sql.sh still applies RLS hardening in prod.

Related: [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]], [[dbmate treats any line starting with -- migrateupdown as a directive|dbmate treats any line starting with -- migrate:up/down as a directive]].

## Related

- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]
- [[dbmate treats any line starting with -- migrateupdown as a directive|dbmate treats any line starting with -- migrate:up/down as a directive]]

%% ai-graph-start %%

**Related notes:**
- [[Guard a destructive down-migration with a session-GUC opt-in]]
- [[dbmate treats any line starting with -- migrateupdown as a directive]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]
- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]

**Relations:**
- leo-customer360 repo — *has* — schema-apply mechanisms
- schema-apply mechanisms — *include* — dbmate
- schema-apply mechanisms — *include* — run-sql.sh
- dbmate — *is a* — schema-apply mechanism
- run-sql.sh — *is a* — schema-apply mechanism
- dbmate — *used by* — docker-compose migrate service
- dbmate — *applies* — dbmate migration files
- dbmate — *tracks versions in* — schema_migrations
- dbmate migration files — *contain* — -- migrate:up
- dbmate migration files — *contain* — -- migrate:down
- -- migrate:down — *is* — DROP SCHEMA IF EXISTS customer360 CASCADE;
- run-sql.sh — *used in* — Terraform
- run-sql.sh — *used with* — bastion
- run-sql.sh — *used in* — UAT
- run-sql.sh — *used in* — prod
- run-sql.sh — *performs* — blind replay
- run-sql.sh — *pipes* — SQL files
- SQL files — *include* — postgres/init
- SQL files — *include* — database-init/*.sql
- SQL files — *include* — database-init/migrations/*
- run-sql.sh — *lacks* — version tracking
- run-sql.sh — *relies on* — IF NOT EXISTS
- run-sql.sh — *relies on* — ON CONFLICT
- dbmate — *is incompatible with* — run-sql.sh
- dbmate migration files — *processed by* — psql
- -- migrate:down — *is a* — SQL comment
- psql — *executes* — DROP SCHEMA IF EXISTS customer360 CASCADE;
- DROP SCHEMA IF EXISTS customer360 CASCADE; — *causes* — database wipe
- dbmate — *parses* — markers
- psql — *does not parse* — markers
- run-sql.sh — *should not point at* — dbmate migration files
- prod/bastion path — *migration means* — running dbmate binary/container
- dbmate binary/container — *is* — single static binary
- dbmate binary/container — *installable like* — psql-client
- run-sql.sh — *uses* — APP_SQL_DIR
- run-sql.sh — *uses* — MIGRATIONS_DIR
- old hand-rolled migrations — *are* — safe to replay
- run-sql.sh — *applies* — RLS hardening
- RLS hardening — *in* — prod
- RLS hardening — *using* — old hand-rolled migrations
- leo-customer360 — *uses* — dbmate
- dbmate — *for* — Postgres migrations
- leo-customer360 — *does not use* — Alembic
- Alembic — *for* — Postgres migrations
- dbmate — *treats* — any line starting with -- migrate:up/down
- any line starting with -- migrate:up/down — *as a* — directive

%% ai-graph-end %%