---
ai_hash: 1905a66af328b6f1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-09
entities: []
source: session 2026-08-09 leo-customer360 terraform adapt
status: seedling
tags:
- postgres
- rls
- security
- multi-tenant
- gotcha
title: Postgres RLS is silently bypassed by superuser connections
type: lesson
---

# Postgres RLS is silently bypassed by superuser connections

Postgres Row-Level Security policies are **silently bypassed** when a connection is made as a superuser (or any role with the `BYPASSRLS` attribute), and also by the table owner unless `FORCE ROW LEVEL SECURITY` is set. No error, no warning — queries just return/allow everything, so tenant isolation quietly does not apply.

**Consequence for multi-tenant apps:** the application MUST connect as a dedicated **non-superuser** login role (e.g. `customer360_app`) for RLS to enforce anything. If config still sets `DB_USER=postgres` (as the LEO Customer360 k8s `c360-config` did), the RLS policies are inert even though they exist in the schema — a security footgun that looks fine in tests run as postgres.

Provisioning the app role is necessary but not sufficient: the deployed config must actually *use* it.

Related: [[Managed DB provisioners create the server but not in-database objects]].

## Related

- [[Managed DB provisioners create the server but not in-database objects]]

%% ai-graph-start %%

**Related notes:**
- [[Postgres Row-Level Security is bypassed by superusers so the app needs a non-superuser role]]
- [[Postgres RLS should be defense-in-depth, not the sole tenant boundary]]
- [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]
- [[pgAdmin shows 0 rows on customer360 tenant tables until you SET app.tenant_id (FORCE RLS)]]
- [[Managed DB provisioners create the server but not in-database objects]]

%% ai-graph-end %%