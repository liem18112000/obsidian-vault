---
ai_hash: 72aa7fe0b60022aa
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-19
entities: []
source: test-agent-v2 code review 2026-09-19
status: seedling
tags:
- postgres
- sql
- truncate
- gotcha
- data-loss
title: TRUNCATE CASCADE defeats a table allowlist
type: lesson
---

# TRUNCATE CASCADE defeats a table allowlist

If you keep an allowlist of tables that are safe to wipe (to protect config/metadata tables from a destructive reset), running `TRUNCATE {allowed} CASCADE` **reintroduces the exact hole the allowlist was built to close**. `CASCADE` also truncates *any* table holding a foreign key into a truncated table — regardless of your allowlist — so an unlisted, meant-to-be-preserved table can be silently emptied.

## The key insight
Listing the full co-dependent set in ONE statement — `TRUNCATE a, b, c` — is already FK-safe (Postgres accepts mutually-referencing tables truncated together). `CASCADE` buys nothing in that direction; it only *adds* the dangerous direction: reaching out to unlisted dependents.

## Fix — fail loud
Drop `CASCADE`. Truncate the co-dependent set explicitly. Then if a future schema change adds an FK from a preserved table into a wiped one, the `TRUNCATE` **raises loudly** ("cannot truncate a table referenced in a foreign key constraint") instead of silently wiping the preserved table. Loud failure > silent data loss for a destructive admin op.

Seen in test-agent-v2 `common/admin/wipe.py::_truncate`, where a `_RUNTIME_TABLES` allowlist had been added after a prod outage — and `CASCADE` quietly re-opened that same outage's failure mode.

## Related
[[Fail-open bearer auth middleware antipattern]]

## Related

- [[Fail-open bearer auth middleware antipattern]]

%% ai-graph-start %%

**Related notes:**
- [[Fail-open bearer auth middleware antipattern]]
- [[Delete-and-reinsert aggregate saves silently cascade-wipe new child tables]]
- [[Empty array in Postgres WHERE NOT (id = ANY(...)) deletes every row]]

%% ai-graph-end %%