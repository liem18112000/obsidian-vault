---
ai_hash: dc2c016d31967dd9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-23
entities: []
tags:
- github-actions
- yaml
- workflow_dispatch
- gotcha
title: 'GitHub Actions on: key parses as YAML boolean True; a workflow_dispatch appears
  in the UI only once on the default branch'
type: gotcha
---

# GitHub Actions on: key parses as YAML boolean True; a workflow_dispatch appears in the UI only once on the default branch

Two GitHub Actions facts that bite when authoring/validating workflows:

1) **`on:` parses as the boolean `True` in YAML 1.1.** When you load a workflow with a YAML parser (PyYAML `safe_load`), the top-level trigger key is `True`, NOT the string `'on'` — so `data['on']` raises KeyError even though the file is perfectly valid. Access it as `data.get('on', data.get(True))` (or `data[True]`). GitHub itself parses `on:` correctly; this only trips scripts that inspect the YAML. (Same quirk hits `off/yes/no`.) A KeyError on 'on' is NOT a syntax error — the file loaded fine.

2) **A `workflow_dispatch` workflow's "Run workflow" button only shows in the Actions UI once the workflow file exists on the DEFAULT branch.** Adding it on a feature branch/PR won't make it runnable from the UI until merged to main (same default-branch requirement as `workflow_run` triggers). You can still trigger it via `gh workflow run` against a branch after it's on the default branch.

Source: leo-customer360 admin-uat.yml (workflow_dispatch admin workflow), 2026-08-23.

%% ai-graph-start %%

**Related notes:**
- [[YAML parses a workflow's on key as boolean True (Norway problem)]]
- [[workflow_dispatch Run button only appears on the default branch - use gh workflow run --ref to dispatch from a feature branch]]
- [[Colon-space in an unquoted GitHub Actions run value breaks the workflow YAML]]
- [[A workflow_run-triggered GitHub workflow always runs the version on the DEFAULT branch]]
- [[workflow_run subscribes by workflow name not filename]]

%% ai-graph-end %%