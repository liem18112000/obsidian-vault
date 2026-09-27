---
ai_hash: 2f13ebce8de74cb0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-23
entities: []
source: session 2026-08-22 leo-customer360
status: seedling
tags:
- git
- branches
- cleanup
- gotcha
title: 'Verify a branch is fully merged before deleting: git log main..branch count
  0'
type: lesson
---

# Verify a branch is fully merged before deleting: git log main..branch count 0

To confirm a branch has nothing unique before deleting it, check that **`git log <base>..<branch>` returns zero commits** (e.g. `git log --oneline origin/main..origin/feature | wc -l` == 0). Zero means every commit on the branch is already reachable from the base, so deletion loses nothing.

## Why not `--is-ancestor`
`git merge-base --is-ancestor origin/branch origin/main` is only true when the branch tip literally sits in main's history — i.e. a fast-forward or a real merge-commit. A **squash-merged** branch (GitHub's default squash) produces a *new* commit on main, so the original branch tip is NOT an ancestor and `--is-ancestor` reports false even though the work is fully in main. The `main..branch` commit-count test is the robust check because it works for squash, rebase, and merge-commit alike (the equivalent code lands in main under a different SHA, so there are no *unique-by-content* commits… caveat below).

## Caveat
For a squash merge the counted commits are compared by SHA, not content — a squashed branch can still show `>0` original commits even though its content is in main. So: count == 0 => definitely safe; count > 0 => check whether it was squash-merged (look up the PR state = MERGED) before assuming there is unmerged work. Combine both: `git merge-base --is-ancestor` OR PR is MERGED OR count==0.

Applied in leo-customer360 to safely delete `infras/cicd/v4` (count==0, ancestor of main) after its PR merged.

## Related

- [[GitHub squash merge]]

%% ai-graph-start %%

**Related notes:**
- [[git merge-base --is-ancestor returns false after a SQUASH merge even though the branch's changes are merged]]
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]
- [[Branch created from current HEAD drags unrelated commits — verify against originmaster]]
- [[Create an empty git branch with commit-tree + the empty-tree hash (never git rm -rf)]]
- [[ReviewPR scope diff the branch's real base, not main, when base is ahead of main]]

%% ai-graph-end %%