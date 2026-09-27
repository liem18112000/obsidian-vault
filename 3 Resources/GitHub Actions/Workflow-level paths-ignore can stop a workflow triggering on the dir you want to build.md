---
ai_hash: 6563a66378dea12f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: session 2026-08-20 leo-customer360 CI
status: seedling
tags:
- github-actions
- ci
- gotcha
title: Workflow-level paths-ignore can stop a workflow triggering on the dir you want
  to build
type: lesson
---

# Workflow-level paths-ignore can stop a workflow triggering on the dir you want to build

A workflow-level `paths-ignore` skips the **entire workflow run** when *all* changed files match its patterns. The trap: if you list a directory there that you actually want to build/test, a commit that touches only that directory triggers **no jobs at all** — so the build for it never runs.

Example: `ci.yml` had `paths-ignore: [frontend-admin/**]` (added when frontend had no tests). Later we wanted to build the frontend image on changes — but a frontend-only commit would not start the workflow. Fix: remove that entry from `paths-ignore` so the workflow triggers, then scope the *test* job separately if you still want to skip it there.

Rule of thumb: `paths-ignore` is about *whether the workflow starts*, not per-job filtering. Use per-job path filters (e.g. dorny/paths-filter) for "run this job only when X changed"; reserve `paths-ignore` for dirs that should never start CI (docs, wireframes).

Related: [[Feed dorny/paths-filter changes output into a build matrix for selective monorepo builds]].

## Related

- [[Feed dorny/paths-filter changes output into a build matrix for selective monorepo builds]]

%% ai-graph-start %%

**Related notes:**
- [[Feed dornypaths-filter changes output into a build matrix for selective monorepo builds]]
- [[GitHub Actions in a monorepo workflows live at repo root, scope per project with paths filters]]
- [[CI path-filter must mirror the Docker build context, not the service folder]]
- [[BuildKit honors a per-Dockerfile .dockerignore]]
- [[Docs Site deploy-docs.yml detect job skips build unless docs paths change]]

%% ai-graph-end %%