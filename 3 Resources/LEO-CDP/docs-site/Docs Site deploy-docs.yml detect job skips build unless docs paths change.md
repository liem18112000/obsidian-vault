---
ai_hash: fae3d6ee7dc15a9c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: session 2026-09-07 chatbot-gone investigation
status: seedling
tags:
- leo-cdp
- docs-site
- github-actions
- quartz
- gotcha
title: Docs Site deploy-docs.yml detect job skips build unless docs paths change
type: lesson
---

# Docs Site deploy-docs.yml detect job skips build unless docs paths change

The **Docs Site** GitHub Actions workflow (`.github/workflows/deploy-docs.yml`) runs on every push, but a `detect` job gates the actual build+deploy: it only proceeds when the pushed commit range touches markdown, embedded media, `.documentignore`, anything under `docs-site/`, or the workflow file itself. Otherwise it sets `changed=false` and the build + GitHub Pages deploy are **skipped by design**.

**The tell is run duration, not the success badge.** A run that finishes in ~10s = detect ran and skipped everything (still shows green "success"). A real build+deploy takes ~1m. So a commit that only touches `frontend-admin/*` produces a fast green run that did **not** redeploy the docs site — expected, not a failure.

When diagnosing "the docs site did not update after my push," check the run **duration** (and whether your changed paths match the detect regex), not just pass/fail. Deploy also only happens from the default branch; feature branches build but never publish.

Related: [[Verify a Quartz afterBody widget renders live before blaming CSS]]

## Related

- [[Verify a Quartz afterBody widget renders live before blaming CSS]]

%% ai-graph-start %%

**Related notes:**
- [[GitHub Pages build on every branch, deploy only from the default branch]]
- [[GitHub Actions push filters - tags-only skips branch pushes, paths ignored for tags]]
- [[Workflow-level paths-ignore can stop a workflow triggering on the dir you want to build]]
- [[workflow_run subscribes by workflow name not filename]]
- [[Chain a CD workflow after CI with workflow_run, gating on conclusion and ref]]

%% ai-graph-end %%