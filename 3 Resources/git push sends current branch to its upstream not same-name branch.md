---
title: git push (no args) targets the branch's UPSTREAM, not a same-name remote branch
tags: [git, gotcha, ci-cd]
created: 2026-09-14
---

# `git push` (no args) targets the branch's UPSTREAM, not a same-name remote branch

With `push.default=simple` (the default), `git push` with no arguments pushes
the **current local branch to its configured upstream ref** — which is *not*
necessarily a remote branch of the same name.

A local branch created from `origin/main` (e.g. `git checkout -b fix/foo origin/main`)
has **upstream = `origin/main`**. So running `git push` while on `fix/foo`
fast-forwards **`origin/main`** — landing your commits on the default branch,
silently, even though your branch is called `fix/foo`.

This is dangerous when a push to `main` auto-triggers CI/CD (a deploy).

**Guards before pushing:**
- `git rev-parse --abbrev-ref '@{upstream}'` — see where `git push` will actually go.
- Push with an explicit refspec: `git push origin HEAD:refs/heads/my-branch`.
- Set `push.default=current` so `git push` creates/updates a *same-name* remote branch.
- `git ls-remote origin refs/heads/main` is the authoritative remote SHA
  (the local `origin/main` tracking ref can be stale).

Recovery if it lands on main: prefer a new branch at the pushed commit + a PR;
avoid force-pushing the shared default branch.

Related: [[workflow_run subscribes by workflow name not filename]]
