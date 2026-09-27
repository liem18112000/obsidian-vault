---
ai_hash: 24b94ba46ee5874e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: LUZ-158644 remove Print and Send user role (TS)'
status: seedling
tags:
- qa
- requirements
- test-planning
- scope
- product-owner
- confluence-distilled
title: Test the decisions that override the story description, not the description
type: lesson
---

# Test the decisions that override the story description, not the description

By the time a story reaches test, the description is usually the **oldest** artefact attached to it. Decisions made in comments, refinement calls, and discovery have moved past it — and testing the description instead of the decisions produces confident, wrong results.

One test plan makes this explicit, with a heading that is worth stealing verbatim:

> **PO decisions and discovery outcomes that override a literal reading of the story description — test these, not the description.**

The overrides it then lists show the shapes this takes:

- **The stated condition was incomplete.** The description implied an only-role check; the real block is *has `PRINT_AND_SEND_BASIC` **and** lacks `COMPANY_ADMINISTRATOR`*. So a user with `epost_letterbox + print_and_send_basic` **is** blocked, while an administrator holding the same role is **exempt — deliberately, so the tenant can still fix itself.** Testing the literal description would have filed that exemption as a bug.
- **Two paths that sound like one.** "Remove the role" means removing it from the *assignable* list only. Users and API keys that already hold it **must still display it**. Assignment and display are separate code paths, and only one was in scope.
- **Scope narrowed in a comment.** A PO comment dated later than the description cut the API work to a single lookup, because no API consumer used the role. The description still described the wider change.
- **An architectural fact that invalidated a sibling ticket.** No feature switch and no migration means rollback is a **revert, not a flag flip** — which makes the sibling sub-task "Set release flag" obsolete as written. Discovery findings can retire other tickets, not just change this one.
- **Wording differs per brand**: one app uses formal address and its own product name, the other informal. Same title text, different body — easy to test as a defect if you only read one.

> [!tip] Put the overrides at the top, with dates
> This plan leads with the overrides *before* the test cases, each traceable to a decision (a PO comment of a given date, a discovery outcome). That gives a reviewer one place to check "is this still true?" and gives the tester explicit permission to contradict the description. A test plan that silently encodes the new behaviour in step 7 of case 12 cannot be reviewed the same way.

> [!warning] Flag scope widening rather than absorbing it
> The same plan notes that the original request named only two hosts, so covering a third app "is a deliberate widening of the requested scope" — marked **open with the PO, not blocking**. That is the right handling: do the broader work, say plainly that you widened it, and let the PO object. Silent scope widening is indistinguishable from scope creep when someone reviews the effort later.

Related: [[Strike what every option shares to find the real architecture decision]] — both are about finding what is actually being decided rather than what is written down.

Source: [[CROSS-TEST LUZ-158644 Investigate and remove Print&Send user role (UI, backend, Public API — no]] (TS, Confluence).

## Related

- [[Strike what every option shares to find the real architecture decision]]

%% ai-graph-start %%

**Related notes:**
- [[CROSS-TEST LUZ-158644 Investigate and remove Print&Send user role (UI, backend, Public API — no]]
- [[testing-agent implement_plan generates scenarios per pack-node x 4 kinds, amplifying pack noise and ignoring non-functional-kind guidance]]
- [[TPD test_kinds must be additive over the base four, not replace them]]
- [[Xray Test Management - Manual Test Guideline]]
- [[Verify an existing flag's data quality before designing on top of it]]

%% ai-graph-end %%