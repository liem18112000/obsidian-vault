---
ai_hash: 4ed4794037fadcc2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: 'session 2026-08-20, leo-customer360 issue #6'
status: seedling
tags:
- github-actions
- ci
- deprecation
- gotcha
- leo-customer360
title: An if-guarded GitHub Actions step emits no deprecation annotation until its
  condition is true
type: lesson
---

# An if-guarded GitHub Actions step emits no deprecation annotation until its condition is true

A GitHub Actions deprecation warning (e.g. "Node.js 20 is deprecated … forced to run on Node.js 24") is only emitted for steps that **actually execute** in a given run. A step guarded by `if:` that evaluates false is skipped, so it produces **no annotation** — even if it uses a deprecated action.

**Consequence / gotcha:** you can miss a deprecated action when auditing CI from run annotations alone. In `leo-customer360` CI, `docker/login-action@v3` (which runs on node20) was NOT in the run-#5 deprecation annotations because it is guarded by `if: ${{ github.ref == refs/heads/main }}` and that run was on a feature branch, so the login step never ran. It would warn (and eventually break) only on a `main` run.

**Takeaway:** when bumping actions off a deprecated runtime, grep every `uses:` in the workflow and check each action manifest directly (see [[Verify a GitHub Action's Node runtime and inputs via gh api on action.yml at a tag|Verify a GitHub Actions Node runtime and inputs via gh api on action.yml at a tag]]) rather than trusting the annotation list from one run — conditional/rarely-taken branches hide deprecated actions.

## Related

- [[Verify a GitHub Action's Node runtime and inputs via gh api on action.yml at a tag]]

%% ai-graph-start %%

**Related notes:**
- [[Verify a GitHub Action's Node runtime and inputs via gh api on action.yml at a tag]]
- [[GitHub Actions Node 20 deprecation warning from v4 actions is harmless noise]]
- [[secrets context is not available in GitHub Actions if conditions]]
- [[CI build Docker image on every run, push only on non-PR]]
- [[GHCR-always plus Docker Hub-optional GitHub Actions publishing pattern]]

%% ai-graph-end %%