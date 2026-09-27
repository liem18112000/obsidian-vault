---
title: workflow_run subscribes by workflow name, not filename
tags: [github-actions, ci-cd, gotcha]
created: 2026-09-14
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
