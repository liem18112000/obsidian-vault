---
ai_hash: 6c24157ad4b5ca56
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: session 2026-09-10 LEO CDP email engine
status: seedling
tags:
- postgres
- sql
- upsert
- gotcha
- leo-cdp
title: Postgres ON CONFLICT DO UPDATE references the target row by unqualified table
  name
type: gotcha
---

# Postgres ON CONFLICT DO UPDATE references the target row by unqualified table name

In an \`INSERT ... ON CONFLICT (...) DO UPDATE\`, the pre-existing row is exposed under the **unqualified** target-table name only. Reference it as \`mytable.col\` in the SET/WHERE clauses — never schema-qualified (\`myschema.mytable.col\`), which raises \`missing FROM-clause entry for table "mytable"\`. The would-be-inserted row is \`EXCLUDED.col\`.

This bites when the INSERT target itself is schema-qualified (common with an explicit schema like \`customer360\`): the INSERT line reads \`INSERT INTO customer360.cdp_campaign_dispatch_logs ...\`, but the conflict-update body must still say \`cdp_campaign_dispatch_logs.attempt_count\`, not \`customer360.cdp_campaign_dispatch_logs.attempt_count\`.

Example (idempotent per-recipient send ledger):
\`\`\`sql
INSERT INTO customer360.cdp_campaign_dispatch_logs (...)
VALUES (...)
ON CONFLICT (campaign_id, master_profile_id) DO UPDATE SET
    attempt_count = cdp_campaign_dispatch_logs.attempt_count + 1,   -- unqualified
    status = EXCLUDED.status
WHERE cdp_campaign_dispatch_logs.status NOT IN ('Sent','Suppressed');  -- guard: never downgrade a terminal row
\`\`\`
The WHERE on the conflict-update is also useful on its own: it makes the upsert refuse to overwrite terminal states, which is how you keep a retried/replayed job from re-sending an already-Sent recipient.

## Related

- [[MongoDB ObjectId]]

%% ai-graph-start %%

**Related notes:**
- [[A UNIQUE index on a partitioned Postgres table must include the partition key, defeating cross-time dedup]]
- [[Migration-free idempotent upserts via deterministic uuid5 primary keys]]
- [[Postgres partitioned-table UNIQUE index must include the partition key]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]
- [[CREATE TABLE IF NOT EXISTS cannot express a rename]]

%% ai-graph-end %%