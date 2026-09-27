---
ai_hash: 6a4465c8398d2a9c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: session 2026-09-05 (leo-customer360 Quartz deploy workflow)
status: seedling
tags:
- github-actions
- github-pages
- ci-cd
- quartz
- deploy
- gotcha
title: 'GitHub Pages: build on every branch, deploy only from the default branch'
type: lesson
---

# GitHub Pages: build on every branch, deploy only from the default branch

GitHub Pages publishes **one** version of a site per repo, so a Pages-deploy workflow must not run its deploy step from feature branches — a WIP branch would overwrite the live site.

## Pattern: build everywhere, deploy from default only
Trigger on `on: push:` (no branch filter) so every branch runs, then split into jobs:
- **build** runs on every branch → validates the site compiles (per-branch CI on the PR).
- **deploy** is guarded: `if: github.ref == format(refs/heads/{0}, github.event.repository.default_branch)` — only the default branch publishes. Using `event.repository.default_branch` beats hardcoding `main`.

For per-branch *published previews* with their own URLs, GitHub Pages cannot help — use a preview host (Cloudflare Pages, Netlify).

## Companion: in-workflow change-detection instead of on.push.paths
To "run on every commit but only rebuild when relevant files changed", drop the trigger-level `paths:` filter and add a `detect` job that diffs the pushed range:
`git diff --name-only "$GITHUB_EVENT_BEFORE" "$GITHUB_SHA" | grep -qE (\.md$|^docs-site/|...)` → sets an output the build job gates on (`needs: detect`, `if: needs.detect.outputs.changed == true`).
- Handle the **zero base SHA** (`0000...0`) on first push / new branch, and `workflow_dispatch`, by treating them as "changed=true" (you cannot diff a missing base).
- Why prefer this over `on.push.paths`: the run always registers as a check (useful for branch-protection required checks), and skip logic lives in one place you can extend.

Related: [[Enumerate a whole-repo docs site with git ls-files, filter with a separate ignore file]]

%% ai-graph-start %%

**Related notes:**
- [[Docs Site deploy-docs.yml detect job skips build unless docs paths change]]
- [[GitHub Actions push filters - tags-only skips branch pushes, paths ignored for tags]]
- [[git push sends current branch to its upstream not same-name branch]]
- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]
- [[Publish every CI build via a rolling latest pre-release on GitHub]]

%% ai-graph-end %%