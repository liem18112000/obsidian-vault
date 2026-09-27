---
ai_hash: d7f367c8fd020aa2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities:
- dbmate
- psql
- leo-customer360 repo
- schema-apply mechanisms
- docker-compose
- migrate service
- database-init/db/migrations/*.sql
- schema_migrations
- -- migrate:up
- -- migrate:down
- DROP SCHEMA IF EXISTS customer360 CASCADE;
- deployments/postgres/run-sql.sh
- Terraform
- bastion
- UAT
- prod
- blind replay
- '*.sql files'
- postgres/init
- database-init/*.sql
- database-init/migrations/*
- psql -f -
- SSH
- version tracking
- IF NOT EXISTS
- ON CONFLICT
- SQL comment
- customer360 schema
- APP_SQL_DIR
- MIGRATIONS_DIR
- dbmate binary
- psql-client
- database-init/migrations/001_*.sql
- RLS hardening
- Alembic
- database
- markers
- Postgres migrations
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
- schema-apply mechanisms — *are* — mutually incompatible
- dbmate — *is a* — schema-apply mechanisms
- dbmate — *used in* — leo-customer360 repo
- dbmate — *associated with* — docker-compose
- docker-compose — *uses* — migrate service
- migrate service — *applies* — database-init/db/migrations/*.sql
- dbmate — *tracks versions in* — schema_migrations
- database-init/db/migrations/*.sql — *contains* — -- migrate:up
- database-init/db/migrations/*.sql — *contains* — -- migrate:down
- -- migrate:down — *is* — DROP SCHEMA IF EXISTS customer360 CASCADE;
- deployments/postgres/run-sql.sh — *is a* — schema-apply mechanisms
- deployments/postgres/run-sql.sh — *used in* — UAT
- deployments/postgres/run-sql.sh — *used in* — prod
- deployments/postgres/run-sql.sh — *uses* — Terraform
- deployments/postgres/run-sql.sh — *uses* — bastion
- deployments/postgres/run-sql.sh — *is a* — blind replay
- blind replay — *pipes* — *.sql files
- blind replay — *pipes* — postgres/init
- blind replay — *pipes* — database-init/*.sql
- blind replay — *pipes* — database-init/migrations/*
- blind replay — *pipes to* — psql -f -
- psql -f - — *operates over* — SSH
- deployments/postgres/run-sql.sh — *lacks* — version tracking
- deployments/postgres/run-sql.sh — *relies on* — IF NOT EXISTS
- deployments/postgres/run-sql.sh — *relies on* — ON CONFLICT
- deployments/postgres/run-sql.sh — *can collect* — database-init/db/migrations/*.sql
- deployments/postgres/run-sql.sh — *pipes* — database-init/db/migrations/*.sql
- database-init/db/migrations/*.sql — *pipes to* — psql
- psql — *runs* — WHOLE file
- -- migrate:down — *is interpreted as* — SQL comment
- SQL comment — *by* — psql
- DROP SCHEMA IF EXISTS customer360 CASCADE; — *executes* — database
- DROP SCHEMA IF EXISTS customer360 CASCADE; — *wipes* — database
- dbmate — *parses* — markers
- psql — *does not parse* — markers
- APP_SQL_DIR — *should not point at* — database-init/db/migrations
- MIGRATIONS_DIR — *should not point at* — database-init/db/migrations
- Migrating prod/bastion path — *to* — dbmate
- Migrating prod/bastion path — *involves* — dbmate binary
- dbmate binary — *is a* — single static binary
- dbmate binary — *installable like* — psql-client
- database-init/migrations/001_*.sql — *is* — old hand-rolled
- database-init/migrations/001_*.sql — *is* — safe to replay
- deployments/postgres/run-sql.sh — *applies* — RLS hardening
- RLS hardening — *is in* — prod
- leo-customer360 — *uses* — dbmate
- dbmate — *for* — Postgres migrations
- leo-customer360 — *does not use* — Alembic
- Alembic — *for* — Postgres migrations
- dbmate — *treats* — -- migrate:up/down
- -- migrate:up/down — *as a* — directive

%% ai-graph-end %%