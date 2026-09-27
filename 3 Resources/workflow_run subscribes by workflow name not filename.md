---
ai_hash: 56a93900b81299d0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
tags:
- github-actions
- ci-cd
- gotcha
title: workflow_run subscribes by workflow name, not filename
---

# workflow_run subscribes by workflow name, not filename

A GitHub Actions workflow triggered by `on: workflow_run` matches the upstream
workflows by their **`name:` field**, not by their `.yml` filename.

```yaml
on:
  workflow_run:
    workflows: ["CI", "Docs Site"]   # must equal each workflow's `name:`
    types: [requested, completed]
```

So `deploy-docs.yml` whose header is `name: Docs Site` is subscribed to as
`"Docs Site"`, **not** `"Deploy Docs"` or the filename. Get the name wrong and
the trigger silently never fires — no error, just nothing.

Verify with: `grep -m1 '^name:' .github/workflows/*.yml`.

- `types: [requested, completed]` gives both start and finish events;
  `requested` has a null `conclusion`.
- Checkout is needed in the `workflow_run` job before a local composite action
  (`uses: ./.github/actions/...`) can be referenced.

Related: [[GitHub composite action for one reusable step]]

%% ai-graph-start %%

**Related notes:**
- [[A workflow_run-triggered GitHub workflow always runs the version on the DEFAULT branch]]
- [[GitHub Actions in a monorepo workflows live at repo root, scope per project with paths filters]]
- [[workflow_dispatch Run button only appears on the default branch - use gh workflow run --ref to dispatch from a feature branch]]
- [[Colon-space in an unquoted GitHub Actions run value breaks the workflow YAML]]
- [[GitHub Actions on key parses as YAML boolean True; a workflow_dispatch appears in the UI only once on the default branch]]

%% ai-graph-end %%