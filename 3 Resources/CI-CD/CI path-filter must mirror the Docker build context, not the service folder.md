---
ai_hash: 1e72e6c6899c5c56
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-30
entities: []
source: session 2026-08-30 leo-customer360
status: seedling
tags:
- ci-cd
- github-actions
- docker
- paths-filter
- gotcha
title: CI path-filter must mirror the Docker build context, not the service folder
type: lesson
---

# CI path-filter must mirror the Docker build context, not the service folder

A path-based CI change-filter (e.g. `dorny/paths-filter`) that decides whether to rebuild an image must watch **every directory the image's build context actually reads from** — not just the folder the Dockerfile lives in.

## Why it bites
An image can COPY files that live outside its own directory. In `leo-customer360`, `postgres/Dockerfile` is built with **build context = repo root** and does `COPY database-init/*.sql /docker-entrypoint-initdb.d/`. The CI filter was `postgres: 'postgres/**'`, so editing `database-init/persona360-schema.sql` matched no filter and the postgres image **silently did not rebuild** — the schema change never reached the published image.

## Fix
List all real inputs in the filter:
```yaml
postgres:
  - 'postgres/**'
  - 'database-init/**'
```

## Principle
CI rebuild triggers should mirror the build-context inputs (the Dockerfile's `COPY`/`ADD` sources), not the service's home directory. A build context that reaches "sideways" into sibling folders is the classic trap — audit `COPY` paths against the filter whenever either changes.

## Related
[[leo-customer360 applies DB schema via two paths that must stay in sync]]

## Related

- [[leo-customer360 applies DB schema via two paths that must stay in sync]]

%% ai-graph-start %%

**Related notes:**
- [[CD only redeploys a leo-customer360 service when its own path changes (dorny paths-filter)]]
- [[leo-customer360 applies DB schema via two paths that must stay in sync]]
- [[Feed dornypaths-filter changes output into a build matrix for selective monorepo builds]]
- [[Workflow-level paths-ignore can stop a workflow triggering on the dir you want to build]]
- [[LEO CDP schema changes must go in both database-schema.sql and a migrations file]]

%% ai-graph-end %%