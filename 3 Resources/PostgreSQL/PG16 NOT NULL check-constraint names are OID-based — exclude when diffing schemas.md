---
ai_hash: 8dea708ac2e92a78
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
source: session 2026-08-24 migration parity test
status: seedling
tags:
- postgres
- schema-diff
- gotcha
- testing
title: PG16 NOT NULL check-constraint names are OID-based — exclude when diffing schemas
type: gotcha
---

# PG16 NOT NULL check-constraint names are OID-based — exclude when diffing schemas

When diffing two PostgreSQL 16 databases via `information_schema.table_constraints`/`check_constraints`, every `NOT NULL` column shows up as a CHECK constraint whose **name is OID-based** — e.g. `25344_26793_1_not_null` (pattern `<reloid>_<?>_<attnum>_not_null`). Those OIDs differ between any two databases (different object-creation order → different OID allocation), so a naive constraint-name comparison reports **false-positive diffs** even when the schemas are identical.

**Fix when comparing schemas across DBs:** exclude the auto NOT NULL constraints — `WHERE tc.constraint_name NOT LIKE '%\_not\_null' ESCAPE '\'` — and rely on `information_schema.columns.is_nullable` for nullability instead (that column-level fact IS deterministic and comparable). Real named CHECKs (`chk_...`) and PK/FK/UNIQUE names ARE deterministic (derived from table/column names), so keep those.

Hit while building the leo-customer360 migration parity test (dbmate build vs legacy build): every schema/data group matched except `constraints`, whose only diff was the OID-named `*_not_null` rows. Related: [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]].

## Related

- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]

%% ai-graph-start %%

**Related notes:**
- [[A self-contained migration parity test is only useful during the cutover]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[CREATE TABLE IF NOT EXISTS cannot express a rename]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]

%% ai-graph-end %%