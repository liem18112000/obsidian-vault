---
ai_hash: 59e8f9a841da2392
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27
status: seedling
tags:
- git
- github
- gotcha
- mirror
title: git push --all skips remote-tracking-only branches
type: lesson
---

# git push --all skips remote-tracking-only branches

﻿`git push <remote> --all` pushes only **local** branches (`refs/heads/*`). Branches that exist in the clone solely as remote-tracking refs (`refs/remotes/origin/*`) are silently skipped — so a "mirror everything to the new remote" push can quietly omit `master` and every branch never checked out locally, while the push output still looks like a success.

Why it bites: a fresh clone has exactly one local branch, so `--all` there pushes 1 branch out of 9.

Fix — expand the remote-tracking refs into explicit refspecs:

```powershell
$refs = git for-each-ref --format='%(refname)' refs/remotes/origin |
        Where-Object { $_ -ne 'refs/remotes/origin/HEAD' }
$specs = $refs | ForEach-Object { $_ + ":refs/heads/" + ($_ -replace '^refs/remotes/origin/','') }
git push <remote> @specs --tags
```

Gotcha inside the gotcha: `refs/remotes/origin/HEAD` shortens to just `origin`, so filtering on `%(refname:short)` produces a bogus `refs/heads/origin` branch on the target. Filter on the full refname instead.

Cleaner alternative when a true mirror is wanted: `git clone --mirror <src>` then `git push --mirror <dest>` — but `--mirror` also DELETES refs on the destination that the source lacks, so never aim it at a remote holding independent branches.

Also: `--all` never pushes tags. Add `--tags` (or `--follow-tags`).

## Related
[[GitHub personal repo transfer is pending until the recipient accepts]]

%% ai-graph-start %%

**Related notes:**
- [[GitHub personal repo transfer is pending until the recipient accepts]]
- [[git push sends current branch to its upstream not same-name branch]]
- [[git checkout -B branch originmain rehomes upstream to originmain, so a bare push targets main]]
- [[GitHub user-to-user repo transfer is a pending invitation, not an immediate move]]
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]

%% ai-graph-end %%