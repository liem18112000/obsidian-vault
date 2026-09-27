---
ai_hash: a88c54604998c4db
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-13
entities: []
source: session 2026-06-13 accesstrade_integration
status: seedling
tags:
- docker
- docker-compose
- gotcha
- volumes
title: Relocating docker-compose.yml renames the Compose project and orphans volumes
type: lesson
---

# Relocating docker-compose.yml renames the Compose project and orphans volumes

Docker Compose derives the **project name** from the directory of the compose file (unless overridden). So moving `docker-compose.yml` from the repo root into a subfolder like `dockers/` silently changes the project name (e.g. `accesstrade_integration` → `dockers`), which **renames every named volume and network** (`accesstrade_integration_atdata` → `dockers_atdata`). Compose then creates fresh, EMPTY volumes and the old data is orphaned — a nasty surprise for a stateful service (a SQLite DB on a named volume just "vanishes").

**Fix:** pin a top-level `name:` in the compose file so the project identity is stable no matter where the file lives:

```yaml
name: accesstrade-integration
services:
  ...
```

Discovered while splitting one Dockerfile into purpose-built web/mcp/cli images under a `dockers/` folder. See [[Docker Compose path resolution env_file vs build context vs dockerfile]] for the relative-path rules that bite at the same time.

## Related

- [[Docker Compose path resolution env_file vs build context vs dockerfile]]

%% ai-graph-start %%

**Related notes:**
- [[Docker Compose path resolution env_file vs build context vs dockerfile]]
- [[Docker hostname for reaching a service depends on where the caller runs]]
- [[A failed docker compose --build leaves latest on the OLD image (silent stale run)]]
- [[Separate docker-compose files are isolated networks; use one file + a profile for optional services]]
- [[CI path-filter must mirror the Docker build context, not the service folder]]

%% ai-graph-end %%