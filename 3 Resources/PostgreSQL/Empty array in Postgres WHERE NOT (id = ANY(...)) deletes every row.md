---
title: "Empty array in Postgres WHERE NOT (id = ANY(...)) deletes every row"
created: 2026-09-08
type: lesson
status: seedling
source: "session 2026-09-08 — docs-vector-search hardening"
tags: [postgresql, sql, gotcha, data-loss]
---

# Empty array in Postgres WHERE NOT (id = ANY(...)) deletes every row

A prune/cleanup query of the shape `DELETE FROM t WHERE NOT (id = ANY(%s))` silently deletes **every** row when the passed array is empty. `id = ANY('{}')` evaluates to false for all rows, so `NOT false` is true for all of them. This turns an innocent "keep these ids, drop the rest" prune into a full table wipe the moment the keep-list is empty — e.g. a mis-mounted or empty corpus directory produces zero chunk ids and destroys the whole index.

**Why it bites:** the empty case looks like a no-op ("keep nothing extra") but is actually "keep nothing at all".

**Mitigation (two layers):**
1. Guard the empty keep-set at the *caller*: refuse to run and exit non-zero (so CI/deploy notices) rather than pruning to nothing.
2. Make the prune function itself a no-op (`return 0`) when the id list is empty — a last-line safety net for any future caller.

Generalizes to any `NOT IN (...)` / `NOT = ANY(...)` delete or update: always special-case the empty collection before issuing the statement.

## Related

- [[PostgreSQL]]
