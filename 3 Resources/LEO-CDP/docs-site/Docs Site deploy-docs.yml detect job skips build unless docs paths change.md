---
title: "Docs Site deploy-docs.yml detect job skips build unless docs paths change"
created: 2026-09-07
type: lesson
status: seedling
source: "session 2026-09-07 chatbot-gone investigation"
tags: [leo-cdp, docs-site, github-actions, quartz, gotcha]
---

# Docs Site deploy-docs.yml detect job skips build unless docs paths change

The **Docs Site** GitHub Actions workflow (`.github/workflows/deploy-docs.yml`) runs on every push, but a `detect` job gates the actual build+deploy: it only proceeds when the pushed commit range touches markdown, embedded media, `.documentignore`, anything under `docs-site/`, or the workflow file itself. Otherwise it sets `changed=false` and the build + GitHub Pages deploy are **skipped by design**.

**The tell is run duration, not the success badge.** A run that finishes in ~10s = detect ran and skipped everything (still shows green "success"). A real build+deploy takes ~1m. So a commit that only touches `frontend-admin/*` produces a fast green run that did **not** redeploy the docs site — expected, not a failure.

When diagnosing "the docs site did not update after my push," check the run **duration** (and whether your changed paths match the detect regex), not just pass/fail. Deploy also only happens from the default branch; feature branches build but never publish.

Related: [[Verify a Quartz afterBody widget renders live before blaming CSS]]

## Related

- [[Verify a Quartz afterBody widget renders live before blaming CSS]]
