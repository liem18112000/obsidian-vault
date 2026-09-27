---
ai_hash: 4b51bec663949b63
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-14
entities: []
tags:
- git
- gotcha
- ci-cd
title: git push (no args) targets the branch's UPSTREAM, not a same-name remote branch
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

%% ai-graph-start %%

**Related notes:**
- [[git checkout -B branch originmain rehomes upstream to originmain, so a bare push targets main]]
- [[Branch created from current HEAD drags unrelated commits — verify against originmaster]]
- [[GitHub Pages build on every branch, deploy only from the default branch]]
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]
- [[git submodule update --remote can clobber unpushed submodule HEAD]]

%% ai-graph-end %%