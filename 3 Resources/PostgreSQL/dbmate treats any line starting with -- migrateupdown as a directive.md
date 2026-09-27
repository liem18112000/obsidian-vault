---
ai_hash: fac718dba9d2dc25
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
source: session 2026-08-24 dbmate implementation
status: seedling
tags:
- dbmate
- migrations
- gotcha
- postgres
title: dbmate treats any line starting with -- migrate:up/down as a directive
type: lesson
---

# dbmate treats any line starting with -- migrate:up/down as a directive

dbmate splits a migration file into its up and down halves by scanning for **lines that begin with** `-- migrate:up` and `-- migrate:down`. The match is line-prefix based, not exact — so a **comment** you write inside the down section that happens to start with `-- migrate:up ...` is misread as a *second* up directive, corrupting the parse (dbmate may error or apply the wrong SQL half).

**Gotcha hit:** a no-op down section documented as `-- migrate:up recreates them idempotently` registered as a 2nd up marker. Fix: reword so no ordinary comment line starts with the literal `-- migrate:up` / `-- migrate:down` token (e.g. "The up section recreates ...").

**Rule:** in a dbmate .sql file, the only lines starting with `-- migrate:up` or `-- migrate:down` should be the real directives. Sanity check: `grep -cE '^-- migrate:(up|down)'` must return exactly 1 each. Same class of trap applies when wrapping a legacy SQL dump into a migration — scan the original body for stray `-- migrate:` prefixes too.

Related: [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]].

## Related

- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]

%% ai-graph-start %%

**Related notes:**
- [[Never pipe a dbmate migration file through a raw psql replay — its down section is destructive]]
- [[Guard a destructive down-migration with a session-GUC opt-in]]
- [[A self-contained migration parity test is only useful during the cutover]]
- [[leo-customer360 uses dbmate for Postgres migrations, not Alembic]]
- [[LEO CDP schema migrations are ordered plain SQL, not dbmate or alembic]]

%% ai-graph-end %%