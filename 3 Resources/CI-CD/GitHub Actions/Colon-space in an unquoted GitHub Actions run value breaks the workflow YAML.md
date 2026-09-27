---
ai_hash: a3df9ae489de4ccb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: session 2026-09-05 (leo-customer360 deploy-docs.yml 0s failure)
status: seedling
tags:
- github-actions
- yaml
- actionlint
- ci-cd
- gotcha
title: Colon-space in an unquoted GitHub Actions run value breaks the workflow YAML
type: gotcha
---

# Colon-space in an unquoted GitHub Actions run value breaks the workflow YAML

In a GitHub Actions workflow, a **single-line unquoted `run:` value that contains `": "` (colon-space) breaks YAML parsing** — YAML reads the text after the colon as a nested mapping and GitHub rejects the entire workflow with "mapping values are not allowed in this context". The run fails in **0 seconds** as an "invalid workflow file", and `gh run list` shows the workflow name as its file path (a tell-tale sign GitHub could not parse `name:`).

## Example that fails
```yaml
- name: Report decision
  run: echo "Docs deploy needed: ${{ steps.check.outputs.changed }}"
```
The shell double-quotes do NOT count as YAML quoting (they are not at the start of the scalar), so `needed: ` triggers the mapping error.

## Fixes
- Block scalar (cleanest, matches multi-line steps): `run: |` then the command on the next line.
- Or YAML-quote the whole value: `run: echo "... needed: ..."`.
- Multi-line `run: |` blocks are immune — the trap is only single-line unquoted `run:`.

## Meta-lesson — lint workflows locally
A local app build never parses `.github/workflows/*.yml`, so a broken workflow ships green locally and fails only on GitHub. Run **actionlint** before committing:
`bash <(curl -sSL https://raw.githubusercontent.com/rhysd/actionlint/main/scripts/download-actionlint.bash) && ./actionlint`. Note actionlint stops at the first YAML *parse* error, so re-run after fixing to surface schema errors it could not reach.

Related: [[GitHub Pages build on every branch, deploy only from the default branch]]

## Follow-up — a workflow's display name comes from the DEFAULT branch
GitHub's canonical name for a workflow (shown in the Actions list and in `gh run list` / the Workflows API) is read from the workflow file **on the repository's default branch**. Consequences, verified empirically:

- A workflow file that exists **only on a feature branch** is displayed by its **path** (`.github/workflows/x.yml`), regardless of a valid `name:` in the file. Editing `name:` and pushing on the feature branch does **not** fix the display — I renamed it and the next run still showed the path.
- It corrects itself automatically **once the file lands on the default branch** (merge the branch).
- An invalid first version makes this worse/more confusing (the run also fails 0s), but the underlying rule is the default-branch source, not "sticky from the invalid registration."

Check what GitHub actually stores: `gh api repos/{owner}/{repo}/actions/workflows --jq '.workflows[] | "\(.name)\t\(.path)"'` — workflows present on the default branch show real names; a feature-branch-only one shows its path.

%% ai-graph-start %%

**Related notes:**
- [[GitHub Actions on key parses as YAML boolean True; a workflow_dispatch appears in the UI only once on the default branch]]
- [[workflow_run subscribes by workflow name not filename]]
- [[workflow_dispatch Run button only appears on the default branch - use gh workflow run --ref to dispatch from a feature branch]]
- [[YAML parses a workflow's on key as boolean True (Norway problem)]]
- [[A workflow_run-triggered GitHub workflow always runs the version on the DEFAULT branch]]

%% ai-graph-end %%