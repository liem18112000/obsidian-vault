---
ai_hash: a264d5d20b5ec0cc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-28
status: seedling
tags:
- git
- plumbing
- git-flow
- howto
title: Create an empty git branch with commit-tree + the empty-tree hash (never git
  rm -rf)
type: howto
---

# Create an empty git branch with commit-tree + the empty-tree hash (never git rm -rf)

To make a branch that points at a commit with NO files (e.g. an intentionally empty `main` in a git-flow where code lives on feature branches), do NOT `git checkout --orphan` + `git rm -rf .` — that wipes your working tree. Instead build an empty-tree commit with plumbing and move the branch to it, leaving the working directory and other branches untouched:

```bash
EMPTY_TREE=$(git hash-object -t tree /dev/null)   # 4b825dc6...4904, the canonical empty tree
E=$(git commit-tree "$EMPTY_TREE" -m "chore: initialize empty main")
git branch -f main "$E"                            # main now has 0 files
```

To keep a feature branch based on that empty root (so a later PR feature->main merges cleanly instead of hitting "unrelated histories"), replay the code onto E:
```bash
git rebase --onto "$E" --root feature/test-agent   # feature = E -> code
```
Then `git push --force-with-lease origin main feature/test-agent`. `commit-tree` writes a commit object directly from a tree + parents without touching HEAD, index, or the working copy — the safe primitive for history surgery.

See [[Bridge-gateway use separate secrets for the inbound caller token and the outbound backend token]].

%% ai-graph-start %%

**Related notes:**
- [[Verify a branch is fully merged before deleting git log main..branch count 0]]
- [[git checkout -B branch originmain rehomes upstream to originmain, so a bare push targets main]]
- [[git push sends current branch to its upstream not same-name branch]]
- [[Branch created from current HEAD drags unrelated commits — verify against originmaster]]
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]

%% ai-graph-end %%