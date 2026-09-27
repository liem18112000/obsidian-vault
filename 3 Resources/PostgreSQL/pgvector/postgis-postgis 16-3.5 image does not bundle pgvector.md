---
ai_hash: 7895eb8eb4a3477d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-16
entities: []
source: session 2026-08-16 postgres/docs research
status: seedling
tags:
- pgvector
- postgis
- docker
- postgres
- gotcha
title: postgis-postgis 16-3.5 image does not bundle pgvector
type: gotcha
---

# postgis-postgis 16-3.5 image does not bundle pgvector

The official `postgis/postgis:16-3.5` Docker image ships PostGIS but **not** pgvector. To get the `vector` extension you must add it — e.g. `apt-get install postgresql-16-pgvector` from the PGDG repo (the image is Debian/PGDG-compatible) in a derived Dockerfile, then `CREATE EXTENSION vector;`.

**Backup/restore consequence:** any restore target — logical restore, physical restore, a streaming replica, or a logical-replication subscriber — must run an image that carries the pgvector binary at a version **≥** the source, or `CREATE EXTENSION vector` fails with `could not open extension control file ".../vector.control"` and the whole restore aborts. (The SQL name is `vector`, not `pgvector`.) In LEO CDP Customer360 the derived image is `customer360-postgres:local` built from `postgres/Dockerfile`; never restore into stock `postgis/postgis:16-3.5`.

Related: [[pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies them as-is]].

## Related

- [[pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies them as-is]]

%% ai-graph-start %%

**Related notes:**
- [[pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies them as-is]]
- [[VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)]]
- [[vDB PostgreSQL supports PostGIS and pgvector plus the fuzzy-match extensions]]
- [[pgBackRest runs on the DB host, so it cannot back up a managed DB like VNG vDB]]
- [[pgBackRest archive_command needs the binary IN the postgres image; the scheduler is a separate sidecar]]

%% ai-graph-end %%