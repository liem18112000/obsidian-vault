---
ai_hash: 99610a82bdd31dd7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-12
entities:
- Regenerate-from-review-feedback pattern
- AI-driven code-authoring pipeline
- Agent
- Human reviewer
- Pull Request (PR)
- Branch
- Worktree
- Review tools
- Retry mechanism
- vinnstack
- BDD "Test implementation" stage
- '`lib/bdd/implementRunner.ts`'
- '`regenerateImplementation` function'
- '`implementBddScenario` function'
- '`commitPushAndFinish` function'
- '`openOrGetPr` logic'
- Bitbucket Cloud API pagination
- '`next` field'
- Full URLs
- Relative paths
- Code
- Comments
- Files
- Commits
source: vinnstack BDD Implement regenerate-from-PR-comments feature, 2026-07-12
status: seedling
tags:
- ai-agents
- code-review
- git
- design-pattern
- vinnstack
title: 'Regenerate-from-review-feedback pattern: reuse the branch/PR, don''t open
  a new one'
type: lesson
---

# Regenerate-from-review-feedback pattern: reuse the branch/PR, don't open a new one

When adding a "regenerate from reviewer feedback" capability to an AI-driven code-authoring pipeline (an agent writes code, opens a PR, a human reviews and leaves comments, then you want the agent to address them), the right shape is: reuse the SAME branch/worktree and the SAME open PR — never create a fresh one.

Concretely: fetch every comment currently on the PR, feed them to the agent as explicit context ("here is real reviewer feedback, address each one with an actual code change"), let it edit the existing files in place on the existing branch, then commit + push to that SAME branch. Since the PR already tracks that branch, pushing new commits updates the existing PR automatically — no new PR needed, and the reviewer sees an updated diff on the same review thread they already commented on.

Why this matters: if you instead spawned a fresh worktree/branch/PR for the "regenerate" action, you'd fragment the review — the reviewer's original comments would live on an abandoned PR while the fix appears in an unrelated new one, breaking the natural feedback loop review tools are built around (comment → push → comment resolves).

Practical gating rules that fell out of this: only allow regenerate on an implementation that's already `done` (not `failed` — that's what a plain "Retry" is for) and that actually has a PR to pull comments from; and fail fast with a clear error if there are zero comments to consume, rather than silently running a pointless no-feedback re-generation.

Applied in vinnstack's BDD "Test implementation" stage (`lib/bdd/implementRunner.ts`'s `regenerateImplementation`, alongside the original `implementBddScenario`) — the two functions share a `commitPushAndFinish` tail (commit → push → detect-or-open PR → mark complete) extracted specifically so "open a NEW PR" logic (`openOrGetPr`) naturally becomes "reuse the existing one" when the branch/PR already exist, with zero special-casing needed.

Related: [[Bitbucket Cloud API pagination returns full URLs in 'next', not relative paths]] — the mechanics of actually fetching "every comment on the PR" that this pattern depends on.

## Related

- [[Bitbucket Cloud API pagination returns full URLs in 'next', not relative paths]]

%% ai-graph-start %%

**Related notes:**
- [[PRD-parity checklist - what comment-driven regenerate with versions actually requires]]
- [[Vinnstack withholds gitgh from the model in BDD step implementation]]
- [[vinnstack BDD pipeline stops at JiraXray, never writes files into a cloned repo]]
- [[A refine regenerate can close open items from evidence the first pass left unexploited]]
- [[A feedback loop with only its write side wired looks like a broken feature]]

**Relations:**
- Regenerate-from-review-feedback pattern — *is a capability of* — AI-driven code-authoring pipeline
- AI-driven code-authoring pipeline — *involves* — Agent
- AI-driven code-authoring pipeline — *involves* — Human reviewer
- Agent — *writes* — Code
- Agent — *opens* — Pull Request (PR)
- Human reviewer — *reviews* — Code
- Human reviewer — *leaves* — Comments
- Regenerate-from-review-feedback pattern — *reuses* — Branch
- Regenerate-from-review-feedback pattern — *reuses* — Pull Request (PR)
- Regenerate-from-review-feedback pattern — *avoids creating* — new Pull Request (PR)
- Regenerate-from-review-feedback pattern — *feeds comments to* — Agent
- Agent — *addresses* — Comments
- Agent — *edits* — Files
- Agent — *commits* — Commits
- Agent — *pushes to* — Branch
- Pull Request (PR) — *tracks* — Branch
- Pushing new commits — *updates* — Pull Request (PR)
- Review tools — *support* — feedback loop
- Regenerate-from-review-feedback pattern — *requires implementation status* — done
- Regenerate-from-review-feedback pattern — *requires existing* — Pull Request (PR)
- Pull Request (PR) — *is for pulling* — Comments
- Retry mechanism — *is for implementation status* — failed
- Regenerate-from-review-feedback pattern — *is applied in* — vinnstack
- Regenerate-from-review-feedback pattern — *is applied in* — BDD "Test implementation" stage
- BDD "Test implementation" stage — *is located at* — `lib/bdd/implementRunner.ts`
- `lib/bdd/implementRunner.ts` — *contains* — `regenerateImplementation` function
- `lib/bdd/implementRunner.ts` — *contains* — `implementBddScenario` function
- `regenerateImplementation` function — *shares tail with* — `commitPushAndFinish` function
- `implementBddScenario` function — *shares tail with* — `commitPushAndFinish` function
- `commitPushAndFinish` function — *includes* — `openOrGetPr` logic
- `openOrGetPr` logic — *adapts to* — reuse existing Pull Request (PR)
- Regenerate-from-review-feedback pattern — *depends on* — mechanics of fetching comments
- mechanics of fetching comments — *is related to* — Bitbucket Cloud API pagination
- Bitbucket Cloud API pagination — *returns* — Full URLs
- Full URLs — *are in* — `next` field
- Bitbucket Cloud API pagination — *does not return* — Relative paths
- Relative paths — *are in* — `next` field

%% ai-graph-end %%