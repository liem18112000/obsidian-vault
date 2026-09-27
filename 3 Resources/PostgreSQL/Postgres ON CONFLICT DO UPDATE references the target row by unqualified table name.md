---
title: "Postgres ON CONFLICT DO UPDATE references the target row by unqualified table name"
created: 2026-09-10
type: gotcha
status: seedling
source: "session 2026-09-10 LEO CDP email engine"
tags: [postgres, sql, upsert, gotcha, leo-cdp]
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
