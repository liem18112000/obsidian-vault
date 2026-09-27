---
ai_hash: 7f5aeaa5176585e0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 seed_data.sh 2026-08-19
status: seedling
tags:
- postgresql
- rls
- pgoptions
- libpq
- seeding
- gotcha
title: Inject a Postgres session GUC into an unmodifiable script via PGOPTIONS
type: lesson
---

# Inject a Postgres session GUC into an unmodifiable script via PGOPTIONS

To run a seed/ETL script that writes RLS-protected tables but does NOT itself set the
tenant GUC (`app.tenant_id`), set it for every libpq connection from OUTSIDE the code via
the `PGOPTIONS` environment variable — no script edit needed:

    docker run -e PGOPTIONS="-c app.tenant_id=<tenant-uuid>" ...

libpq applies PGOPTIONS as connection startup options when the code calls
`psycopg2.connect(host=..., dbname=..., ...)` without an explicit `options=` arg, so the
GUC is set at session start for every connection the script opens. The RLS policy
`current_setting(app.tenant_id, true)::uuid` then evaluates to the tenant and the
FORCE-RLS DELETE/INSERT for that tenant succeed under a plain (non-superuser) role.

Only valid when all rows belong to ONE tenant (a single PGOPTIONS value). Transaction-local
`set_config(...,true)` calls inside the script still override it per-transaction, which is
fine. Works for any libpq client (psql honors PGOPTIONS too).

Used in leo-customer360 deployments/server/seed_data.sh to run the CIR demo seed
(init_sample_data / run_demo_resolution / seed_full_demo_data — none set app.tenant_id)
against the managed non-superuser DB. Related: [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]

## Related

- [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]

%% ai-graph-start %%

**Related notes:**
- [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]
- [[Set Postgres RLS session GUC via the raw DBAPI connection, not Session.execute]]
- [[Postgres Row-Level Security is bypassed by superusers so the app needs a non-superuser role]]
- [[Postgres RLS is silently bypassed by superuser connections]]
- [[pgAdmin shows 0 rows on customer360 tenant tables until you SET app.tenant_id (FORCE RLS)]]

%% ai-graph-end %%