---
ai_hash: 8a5c4427f5d12fb1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Proposal eArchived architecture direction for ePost web 2 (Helios)'
status: seedling
tags:
- architecture
- decision-making
- adr
- proposals
- confluence-distilled
title: Strike what every option shares to find the real architecture decision
type: lesson
---

# Strike what every option shares to find the real architecture decision

When an architecture proposal lists options, the fastest way to unblock the decision is to find what every option shares and **strike it from the discussion**. What remains is the actual choice — and it is usually smaller and more answerable than the framing suggested.

The worked example: a proposal for where eArchive should live in a new web frontend. The options appeared to be about whether to route through the Next.js server-side layer. The clarification that resolved it:

> The **Next.js server-side layer is common to all options.** That means the real architecture decision is **not** whether to use the server-side layer. The real decision is: *which backend should eArchive call behind that layer?*

Once stated, the debate stops being "should we adopt this layer" — a large, architectural-sounding question nobody can answer quickly — and becomes "pick one of three backends", which is decidable with known trade-offs.

**Why options get framed badly.** Each option is usually written as a complete end-to-end story, because that is how you validate it works. But presenting them that way repeats the shared parts in every option, and shared parts are visually loud — readers start arguing about the component that appears in all three rather than the one that differs.

**The practice:**

1. Write the options out fully to check each is viable.
2. Diff them. Anything identical across all options is **context**, not a choice.
3. Restate the decision as only the differing part, and say explicitly that the rest is settled.
4. Put the shared parts in a "Background" section so nobody re-litigates them.

> [!tip] The reciprocal check
> If after the diff *nothing* differs, there was never a decision to make — you have one option described three ways. If *everything* differs, the options are not comparable and you are missing a shared frame; find the constraint they all have to satisfy before scoring them.

The same proposal also landed on a **dedicated module** (`luz-earchive`) rather than folding eArchive into the adjacent `luz-unified-inbox` — chosen for clearest separation of responsibilities while leaving the frontend flow unchanged. That is the other half of a good proposal: state which property you optimised for, so a future reader can tell whether the reason still holds.

Source: [[Proposal eArchived architecture direction for ePost web 2]] (Helios, Confluence).

## Related

- [[Proposal eArchived architecture direction for ePost web 2]]

%% ai-graph-start %%

**Related notes:**
- [[Proposal eArchived architecture direction for ePost web 2]]
- [[Share features as vertical slices with app-owned routes and an injected adapter]]
- [[Discuss LUZ-154249 Architecture for shared components features between apps]]
- [[Architecture]]
- [[Test the decisions that override the story description, not the description]]

%% ai-graph-end %%