---
ai_hash: 23461ed6b559253a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-25
entities: []
source: session 2026-08-25 sql cleanup
status: seedling
tags:
- migrations
- testing
- dbmate
- decision
title: A self-contained migration parity test is only useful during the cutover
type: lesson
---

# A self-contained migration parity test is only useful during the cutover

A self-contained migration **parity test** (build the schema the old way + build it the new way in throwaway DBs, diff them) is only meaningful **during the cutover**, while the two sources can still diverge. Once you delete the legacy bootstrap files and the new-tool migrations become the single source of truth (and are literal copies of the old files), the self-contained comparison degenerates to **dbmate-vs-a-copy-of-itself** — it can never fail, so it is dead weight.

**Decision (leo-customer360):** after deleting database-schema.sql / init-core-database.sql / data-view-for-llm.sql, the parity test was reworked to **live-only** — it now diffs the fresh dbmate build against the **current live DB** (schema = hard gate, data = informational). That is the check with ongoing value: schema drift detection. Apply-from-zero + idempotency stay covered by CI, so no coverage is lost.

**General rule:** a migration-adoption parity harness has a lifecycle — keep the old-vs-new self-contained diff until the cutover merges, then retire it in favor of a new-build-vs-production drift check. Don't keep a tautological test around. Related: [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]].

## Related

- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]

%% ai-graph-start %%

**Related notes:**
- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]
- [[dbmate treats any line starting with -- migrateupdown as a directive]]
- [[Postgres docker-entrypoint-initdb.d runs only once on an empty volume]]

%% ai-graph-end %%