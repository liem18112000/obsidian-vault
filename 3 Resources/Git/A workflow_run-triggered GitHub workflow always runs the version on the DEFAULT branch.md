---
ai_hash: 678bfbcff53d452d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-23
entities: []
tags:
- github-actions
- workflow_run
- cd
- gotcha
title: A workflow_run-triggered GitHub workflow always runs the version on the DEFAULT
  branch
type: gotcha
---

# A workflow_run-triggered GitHub workflow always runs the version on the DEFAULT branch

GitHub Actions `on: workflow_run` (a workflow that fires after another workflow completes) only ever triggers if the workflow file exists on the repo's DEFAULT branch, and the version that RUNS is the one on the default branch — NOT the version on the branch/PR that the triggering run came from.

Consequence: editing a workflow_run-triggered workflow (e.g. a CD pipeline that fires after CI) on a feature branch has NO effect until it's MERGED to the default branch. You can't validate the new trigger behavior from the PR; it only takes effect post-merge, and the merge commit's own CI run is the first to exercise it.

(Contrast: `on: push`/`on: pull_request` workflows run the version AT the pushed/PR ref, so branch edits take effect immediately for those.)

Practical: test workflow_run logic changes by merging to a throwaway default-like branch, or by refactoring the risky logic into a script the workflow calls (the script can be tested on the branch) so the YAML change stays trivial.

Source: leo-customer360 cd.yml (workflow_run-based CD), 2026-08-23.

%% ai-graph-start %%

**Related notes:**
- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]
- [[workflow_run subscribes by workflow name not filename]]
- [[CD didn't fire on an infra-only merge (CI paths-ignore); re-run a workflow_run deploy via workflow_dispatch, not run rerun]]
- [[workflow_dispatch Run button only appears on the default branch - use gh workflow run --ref to dispatch from a feature branch]]
- [[GitHub Actions on key parses as YAML boolean True; a workflow_dispatch appears in the UI only once on the default branch]]

%% ai-graph-end %%