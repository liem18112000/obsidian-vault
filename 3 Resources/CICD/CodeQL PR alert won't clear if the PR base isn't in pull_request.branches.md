---
ai_hash: e77b5a9ee4d8981c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: session 2026-09-16
status: seedling
tags:
- cicd
- github-actions
- codeql
- code-scanning
- pull-request
- gotcha
title: CodeQL PR alert won't clear if the PR base isn't in pull_request.branches
type: gotcha
---

# CodeQL PR alert won't clear if the PR base isn't in pull_request.branches

A CodeQL / code-scanning alert shown on a PR will NOT refresh (clear) after you push a fix if the PR's BASE branch is not listed in the workflow's `on.pull_request.branches` filter.

Why: the per-PR CodeQL analysis (which tracks alert instances on `refs/pull/<n>/head`) is driven by the `pull_request` event. If that event is filtered to e.g. `branches: [main, master]` and the PR targets a different base (e.g. `dev-uat`), no pull_request analysis runs on later pushes, so the alert instance stays frozen at the commit it was first seen on -- even though `push`-triggered CI (branches: ['**']) still runs build/test. `push` CodeQL analyzes the branch ref, not the PR ref, so it doesn't update the PR's alert either.

Symptoms: `gh api .../code-scanning/alerts/<id>` shows `most_recent_instance.commit_sha` stuck at a pre-fix commit and `state: open`; `.../alerts/<id>/instances` lists only a `refs/pull/<n>/head` instance; `.../analyses?ref=refs/heads/<branch>` is empty.

Fixes: add the base branch to `on.pull_request.branches` (also closes a real gap -- an active integration branch getting no PR gate). For `pull_request` events GitHub uses the workflow file from the PR HEAD (merge ref), so pushing the trigger change onto the feature branch takes effect on the next synchronize; the branches filter matches the PR's BASE. (For `pull_request_target` it would use the base branch's workflow instead.) Alternatively the alert auto-resolves once the fix reaches a branch the workflow does analyze (e.g. main).

## Related
[[LEO CI pushes images only on main/tags; feature branches build-only]]
[[Review/PR scope: diff the branch's real base, not main, when base is ahead of main]]

## Related

- [[LEO CI pushes images only on main/tags; feature branches build-only]]

%% ai-graph-start %%

**Related notes:**
- [[ReviewPR scope diff the branch's real base, not main, when base is ahead of main]]
- [[Same-repo branch push fires both push and pull_request events (duplicate CI runs)]]
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]
- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]
- [[Feature-branch images are never pushed to GHCR in leo-customer360 CI]]

%% ai-graph-end %%