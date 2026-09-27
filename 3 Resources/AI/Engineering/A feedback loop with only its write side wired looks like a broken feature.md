---
ai_hash: 5b1b08b7ac029433
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-22
entities:
- Feedback loop
- Write side
- Broken feature
- Vinnstack
- Track-comments bug
- Inline comments
- UI panel
- API
- Storage
- Regeneration prompt
- generateInterrogation
- listComments
- Comment feature
- Read side
- Consumer
- buildInlineCommentsBlock
- comments
- dir
- docLabel
- PRD regens
- Process-flow regens
- Tier LLM effort per pipeline stage - pay where quality compounds, cut where the
  task is bounded
source: session 2026-07-22
status: seedling
tags:
- feedback-loop
- prompt
- vinnstack
- gotcha
title: A feedback loop with only its write side wired looks like a broken feature
type: lesson
---

# A feedback loop with only its write side wired looks like a broken feature

Vinnstack's track-comments bug (found 2026-07-22): inline comments on the business/technical question sets were fully wired on the WRITE side — UI panel, API, storage, even post-regenerate re-anchoring — but the regeneration prompt never READ them back (`generateInterrogation` built its prompt without `listComments`). To the user this presents as "the comment feature doesn't work": comments save fine, but regenerating ignores them, and after regen most go orphaned (re-anchoring against a replaced question set), which looks like they were silently discarded.

General lesson: a feedback loop has a write side (capture the feedback) and a read side (act on it in the next iteration). Wiring only the write side creates a convincing illusion of a working feature — every UI interaction succeeds — while the loop is actually open. When auditing a feedback feature, trace the CONSUMER: find where the stored feedback re-enters a prompt/decision, not just where it's saved.

Fix shape: inject `buildInlineCommentsBlock(comments, dir, docLabel)` into the track regen prompt (the same block the PRD and process-flow regens already used), with a docLabel param so the instruction text names the right artifact.

## Related

- [[Tier LLM effort per pipeline stage - pay where quality compounds, cut where the task is bounded]]

%% ai-graph-start %%

**Related notes:**
- [[Gesture-only features need an always-visible teacher - empty state must not hide the affordance]]
- [[PRD-parity checklist - what comment-driven regenerate with versions actually requires]]
- [[Inject anchored inline comments into LLM regeneration prompts as quoted passages]]
- [[Regenerate-from-review-feedback pattern reuse the branchPR, don't open a new one]]
- [[Async-enriched columns need a lazy backfill for pre-feature rows]]

**Relations:**
- Feedback loop — *HAS_COMPONENT* — Write side
- Feedback loop — *HAS_COMPONENT* — Read side
- Feedback loop — *IS_A* — Broken feature
- Vinnstack — *HAS* — Track-comments bug
- Track-comments bug — *INVOLVES* — Inline comments
- Inline comments — *WIRED_ON* — Write side
- Write side — *INCLUDES* — UI panel
- Write side — *INCLUDES* — API
- Write side — *INCLUDES* — Storage
- Regeneration prompt — *DID_NOT_READ* — Inline comments
- generateInterrogation — *BUILDS* — Regeneration prompt
- generateInterrogation — *EXCLUDES* — listComments
- Comment feature — *APPEARS_AS* — Broken feature
- comments — *ARE_IGNORED_BY* — Regeneration prompt
- comments — *BECOME* — Orphaned
- Write side — *CAPTURES* — feedback
- Read side — *ACTS_ON* — feedback
- Wiring only Write side — *CREATES* — Illusion of working feature
- Auditing feedback feature — *REQUIRES* — Tracing Consumer
- Consumer — *USES* — Stored feedback
- Fix — *INJECTS* — buildInlineCommentsBlock
- buildInlineCommentsBlock — *INTO* — Regeneration prompt
- buildInlineCommentsBlock — *USES_PARAMETER* — comments
- buildInlineCommentsBlock — *USES_PARAMETER* — dir
- buildInlineCommentsBlock — *USES_PARAMETER* — docLabel
- buildInlineCommentsBlock — *USED_BY* — PRD regens
- buildInlineCommentsBlock — *USED_BY* — Process-flow regens
- Feedback loop — *RELATED_TO* — Tier LLM effort per pipeline stage - pay where quality compounds, cut where the task is bounded

%% ai-graph-end %%