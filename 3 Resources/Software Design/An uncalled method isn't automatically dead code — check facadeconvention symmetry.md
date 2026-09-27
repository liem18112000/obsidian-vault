---
ai_hash: 0fcad57ba9f141f8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-19
entities: []
source: leo-customer360 PR 77 review 2026-09-19
status: seedling
tags:
- refactoring
- dead-code
- code-review
- gotcha
- ponytail
title: An uncalled method isn't automatically dead code — check facade/convention
  symmetry
type: lesson
---

# An uncalled method isn't automatically dead code — check facade/convention symmetry

Before deleting a method/function because "grep shows no caller", check whether it belongs to a **convention or symmetric family** where peers keep equivalent members even when currently uncalled. Removing one to save a few lines breaks the pattern and can delete an intended-but-not-yet-wired entry point.

## The trap
"No caller" is necessary but NOT sufficient for "dead". A facade/registry that exposes one method per member (one trigger per service, one handler per route, one adapter per provider) is a contract: peers may be uncalled today yet deliberately present as the API surface.

## Real instance
leo-customer360 `dagster_client.py`: I deleted `NotificationEngineDagsterService.dispatch()` as "no caller". But the sibling `EmailEngineDagsterService.send_campaign()` is ALSO uncalled from the API and was kept — both are the API-side trigger contract for their engine (the live path triggers the job from campaign_activation instead). Deleting one broke the symmetry; restored it.

## Rule of thumb
When a deletion candidate has same-shaped siblings, either delete the WHOLE family (if all truly dead) or none. Deleting the odd one that happens to have no caller today is a confident-wrong cut.

## Related

- [[A field written everywhere and read nowhere is dead code]] — the opposite case: data fields have no symmetry argument, so a never-read field really is dead.

%% ai-graph-start %%

**Related notes:**
- [[A field written everywhere and read nowhere is dead code]]
- [[Dead-code refcount scans flag intentional seams as unused; vet before deleting]]

%% ai-graph-end %%