---
ai_hash: f2875904f2ee805d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27
status: seedling
tags:
- github
- gh-cli
- gotcha
- repo-admin
title: GitHub user-to-user repo transfer is a pending invitation, not an immediate
  move
type: gotcha
---

# GitHub user-to-user repo transfer is a pending invitation, not an immediate move

Transferring a GitHub repo from one **user** account to another user does not happen when you call the API — it only creates a **pending invitation** that the receiving account must accept. Until then the repo stays where it is.

The trap is that the call looks like it worked:

- `gh api -X POST repos/{owner}/{repo}/transfer -f new_owner=<user>` returns `202` plus the repo JSON, and that JSON still shows the **old** owner. A success response is not confirmation that ownership moved.
- `gh api repos/<new_owner>/<repo>` returns **404** until the invitation is accepted — that 404 is the real status signal, not an error.
- Re-issuing the POST changes nothing. Only the target account accepting (via the repo page or its notifications) completes it, and no token you hold for the *source* account can do that for them.

Contrast with transferring into an **organization** where you already hold admin: that completes immediately, no acceptance step.

Once accepted, the old `owner/repo` URL redirects to the new one, so existing git remotes and clones keep working without edits — no need to rewrite remote URLs ahead of the transfer.

## Related
[[gh CLI]]

%% ai-graph-start %%

**Related notes:**
- [[GitHub personal repo transfer is pending until the recipient accepts]]
- [[git push --all skips remote-tracking-only branches]]
- [[gh CLI is GitHub-only, not Bitbucket-aware]]

%% ai-graph-end %%