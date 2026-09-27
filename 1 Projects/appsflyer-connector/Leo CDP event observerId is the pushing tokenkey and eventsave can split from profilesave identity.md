---
ai_hash: 1f2eafc09caaa531
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-30
entities:
- Leo CDP
- observerId
- tokenkey
- tracking event
- mobile_install event
- 30snDokTeHwFK4p6oBnMBB
- write token
- read token
- default_access_key
- workspace
- event/save
- targetUpdateEmail
- profile/save
- primaryEmail
- profile id 1sM9LB0RC8UXRvD0lA5UNm
- visitor profile 3DmymIkokb6VcEB9irMdbO
- CdpHttpSink
- Identity resolution
- eventStatistics
- repeated pushes
- aggregation
- counter increments
- duplication
- Leo CDP Data Observer API
- Identity-keyed CDP API breaks content-hash idempotency
- Leo CDP profilelist ignores start and limit and embeds event data
source: session 2026-06-30; live dump
status: seedling
tags:
- leo-cdp
- identity
- gotcha
- events
title: Leo CDP event observerId is the pushing tokenkey and event/save can split from
  profile/save identity
type: observation
---

# Leo CDP event observerId is the pushing tokenkey and event/save can split from profile/save identity

Two identity facts observed on the live Leo CDP Data Observer API:

1. **`observerId` on a tracking event = the `tokenkey` that pushed it.** Our `mobile_install` event came back with `observerId: 30snDokTeHwFK4p6oBnMBB` — exactly the write token's `tokenkey`. So events are attributed to the ingesting token/observer, which is how a read token (a different key, e.g. `default_access_key`) can still see events written by another key in the same workspace.

2. **`event/save` (targetUpdateEmail) did NOT merge into the `profile/save` (primaryEmail) profile.** `POST /api/profile/save updateByKey=primaryEmail` returned profile id `1sM9LB0RC8UXRvD0lA5UNm` (email set, no events). The event pushed via `CdpHttpSink` with `targetUpdateEmail=<same email>` instead landed on a SEPARATE, email-less visitor profile `3DmymIkokb6VcEB9irMdbO`. Identity resolution between the two endpoints is not guaranteed to converge on one profile by email — so do not assume an event sent by `targetUpdateEmail` attaches to the profile you created by `primaryEmail`.

Positive signal: that event profile's `eventStatistics` showed `mobile_install: 4`, matching the 4 times the demo push was run — confirming repeated pushes aggregate (counter increments) rather than duplicate. Related: [[Identity-keyed CDP API breaks content-hash idempotency]], [[Leo CDP profilelist ignores start and limit and embeds event data]].

## Related

- [[Identity-keyed CDP API breaks content-hash idempotency]]
- [[Leo CDP profilelist ignores start and limit and embeds event data]]

%% ai-graph-start %%

**Related notes:**
- [[Leo CDP save returns 200 but eventlist cannot read it back]]
- [[Leo CDP public REST API contract]]
- [[Identity-keyed CDP API breaks content-hash idempotency]]
- [[Leo CDP profilelist ignores start and limit and embeds event data]]
- [[Leo CDP admin dashboard is a hash-routed SPA on a separate host from the API]]

**Relations:**
- observerId — *is_equivalent_to* — tokenkey
- tracking event — *has_attribute* — observerId
- tokenkey — *pushes* — tracking event
- mobile_install event — *has_observerId* — 30snDokTeHwFK4p6oBnMBB
- 30snDokTeHwFK4p6oBnMBB — *is_a* — tokenkey
- 30snDokTeHwFK4p6oBnMBB — *belongs_to* — write token
- tracking event — *attributed_to* — ingesting token
- read token — *is_a_type_of* — key
- read token — *example* — default_access_key
- read token — *can_see_events_in* — workspace
- event/save — *uses_identity_field* — targetUpdateEmail
- profile/save — *uses_identity_field* — primaryEmail
- event/save — *did_not_merge_into* — profile/save
- profile/save — *returned_profile_id* — profile id 1sM9LB0RC8UXRvD0lA5UNm
- profile id 1sM9LB0RC8UXRvD0lA5UNm — *has_email_set* — true
- profile id 1sM9LB0RC8UXRvD0lA5UNm — *has_events* — false
- mobile_install event — *pushed_via* — CdpHttpSink
- CdpHttpSink — *uses_identity_field* — targetUpdateEmail
- mobile_install event — *landed_on* — visitor profile 3DmymIkokb6VcEB9irMdbO
- visitor profile 3DmymIkokb6VcEB9irMdbO — *is_email_less* — true
- visitor profile 3DmymIkokb6VcEB9irMdbO — *is_separate_from* — profile id 1sM9LB0RC8UXRvD0lA5UNm
- Identity resolution — *between* — event/save
- Identity resolution — *between* — profile/save
- Identity resolution — *not_guaranteed_to_converge_on_one_profile_by_email* — true
- visitor profile 3DmymIkokb6VcEB9irMdbO — *has_attribute* — eventStatistics
- eventStatistics — *shows* — mobile_install: 4
- mobile_install: 4 — *matches* — 4 times demo push was run
- repeated pushes — *result_in* — aggregation
- aggregation — *is_a_form_of* — counter increments
- aggregation — *is_not* — duplication
- Leo CDP — *has_API* — Leo CDP Data Observer API
- Leo CDP Data Observer API — *observes* — identity facts
- Leo CDP — *related_to* — Identity-keyed CDP API breaks content-hash idempotency
- Leo CDP — *related_to* — Leo CDP profilelist ignores start and limit and embeds event data
- Leo CDP — *has_concept* — observerId
- Leo CDP — *has_concept* — tokenkey
- Leo CDP — *has_endpoint* — event/save
- Leo CDP — *has_endpoint* — profile/save

%% ai-graph-end %%