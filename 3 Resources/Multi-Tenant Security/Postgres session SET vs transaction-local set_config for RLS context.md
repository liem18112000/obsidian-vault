---
ai_hash: ef1b69beeb9a9998
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: c360 python code review 2026-09-05
status: seedling
tags:
- postgres
- rls
- connection-pool
- gotcha
title: Postgres session SET vs transaction-local set_config for RLS context
type: lesson
---

# Postgres session SET vs transaction-local set_config for RLS context

In Postgres, `SET app.tenant_id = x` is SESSION-scoped: it persists after COMMIT and, with a connection pool, remains set when the connection is handed to the next borrower. A background worker that sets tenant context this way and then relies on RLS will read/write the *previous* tenant's rows on a reused connection.

Use transaction-local scope instead: `SET LOCAL app.tenant_id = x` or `select set_config('app.tenant_id', v, true)` (third arg true = tx-local), so it resets automatically at transaction end. Per-request handlers should also use tx-local scope.

## Related

- [[Postgres RLS should be defense-in-depth, not the sole tenant boundary]]

%% ai-graph-start %%

**Related notes:**
- [[Postgres RLS should be defense-in-depth, not the sole tenant boundary]]
- [[Response caches must include the authenticated tenant in the key]]
- [[Set Postgres RLS session GUC via the raw DBAPI connection, not Session.execute]]
- [[Derive tenant identity from the verified token, never from request input]]
- [[Postgres RLS is silently bypassed by superuser connections]]

%% ai-graph-end %%