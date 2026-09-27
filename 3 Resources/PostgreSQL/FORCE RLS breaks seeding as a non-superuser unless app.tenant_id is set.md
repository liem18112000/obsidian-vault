---
ai_hash: b5b941d38277932a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 run-sql session 2026-08-19
status: seedling
tags:
- postgresql
- rls
- row-level-security
- multi-tenant
- seeding
- gotcha
title: FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set
type: lesson
---

# FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set

A table with ENABLE + FORCE ROW LEVEL SECURITY and a tenant policy like
`WITH CHECK (tenant_id = current_setting('app.tenant_id', true)::uuid)` will REJECT
every INSERT unless the session GUC is set to the row's tenant. With the GUC unset,
`current_setting(..., true)` returns NULL, so `tenant_id = NULL` is false and the row
is denied -- fail-closed.

The trap: PostgreSQL SUPERUSERS always bypass RLS (cannot be overridden), and plain
ENABLE (without FORCE) also lets the table OWNER bypass. So a seed script that runs
fine locally as the `postgres` superuser SILENTLY relies on that bypass. Run the same
script against a MANAGED database as its non-superuser master role (e.g. app_admin) and
every INSERT into a FORCE-RLS tenant table fails with:
`new row violates row-level security policy for table "..."`.

Fix (the mechanism the policy already assumes): set the tenant context once per
session before the seed inserts --
`SELECT set_config('app.tenant_id', '<tenant-uuid>', false);` (false = session-level,
persists for the whole `psql -f` run). Works for any non-superuser role; a no-op for
superusers. Only valid when all seeded rows share one tenant; otherwise re-set the GUC
per tenant. Do NOT "fix" it with `ALTER ROLE app SET app.tenant_id=...` -- that leaks a
default tenant into the runtime app and hides missing-context bugs.

Discovered in leo-customer360: database-init/init-core-database.sql seeded cdp_segments
under FORCE RLS; failed on the managed VNG vDB (app_admin) but passed on local docker
(postgres superuser). Related: [[ssh host 'bash -s' flattens args into a remote shell string]]

## Related

- [[ssh host 'bash -s' flattens args into a remote shell string]]

%% ai-graph-start %%

**Related notes:**
- [[Inject a Postgres session GUC into an unmodifiable script via PGOPTIONS]]
- [[Postgres Row-Level Security is bypassed by superusers so the app needs a non-superuser role]]
- [[pgAdmin shows 0 rows on customer360 tenant tables until you SET app.tenant_id (FORCE RLS)]]
- [[Postgres RLS is silently bypassed by superuser connections]]
- [[Postgres RLS should be defense-in-depth, not the sole tenant boundary]]

%% ai-graph-end %%