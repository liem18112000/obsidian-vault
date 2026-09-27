---
ai_hash: d12c84b738093d1b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-30
entities:
- Leo CDP
- Leo CDP Data Observer API
- GET /api/event/list
- profileId
- errorCode 500
- GET /api/profile/list
- lastTrackingEvent
- behavioralEvents
- eventStatistics
- funnelStage
- funnelStageTimeline
- inJourneyMaps
- pagination
- start
- limit
- AppsFlyer
- maximum_rows
- examples/list_all_events.py
- Leo CDP public REST API contract
- Leo CDP save returns 200 but eventlist cannot read it back
source: session 2026-06-30; live probes with read token
status: seedling
tags:
- leo-cdp
- api
- pagination
- gotcha
- events
title: Leo CDP profile/list ignores start and limit and embeds event data
type: lesson
---

# Leo CDP profile/list ignores start and limit and embeds event data

On the Leo CDP Data Observer API there is **no global "list all events" endpoint**, and `GET /api/event/list?profileId=...` reliably returns `errorCode 500` "No profile found" even for profile ids that `/api/profile/list` just returned. So the practical way to read events is **`GET /api/profile/list`**, which embeds each profile's event data:

- `lastTrackingEvent` — the most recent event (full object: metricName, metricValue, createdAt, observerId, isConversion, ...)
- `behavioralEvents` — list of metric names the profile has fired
- `eventStatistics` — per-`{journeyId}-{metric}` counts, e.g. `{"<journey>-mobile_install": 4}`
- `funnelStage`, `funnelStageTimeline`, `inJourneyMaps`

**Gotcha — pagination is fake:** `profile/list?segment_id=&start=&limit=` **ignores both `start` and `limit`**; every call returns the same fixed page (~10 most-recent profiles). Probed start=0/10/20/30 and limit=5/10/50/100 — all returned the identical 10 ids. So you cannot page to more than that top set with this token; a paging loop must stop when it sees no NEW ids or it loops forever. (Same family as the AppsFlyer `maximum_rows` floor: a limit param that is silently overridden.)

Implemented in `examples/list_all_events.py`. Related: [[Leo CDP public REST API contract]], [[Leo CDP save returns 200 but eventlist cannot read it back]].

## Related

- [[Leo CDP public REST API contract]]
- [[Leo CDP save returns 200 but eventlist cannot read it back]]

%% ai-graph-start %%

**Related notes:**
- [[Leo CDP save returns 200 but eventlist cannot read it back]]
- [[Leo CDP event observerId is the pushing tokenkey and eventsave can split from profilesave identity]]
- [[Leo CDP public REST API contract]]
- [[Leo CDP admin dashboard is a hash-routed SPA on a separate host from the API]]
- [[Identity-keyed CDP API breaks content-hash idempotency]]

**Relations:**
- Leo CDP Data Observer API — *is part of* — Leo CDP
- GET /api/event/list — *is an endpoint of* — Leo CDP Data Observer API
- GET /api/event/list — *requires* — profileId
- GET /api/event/list — *returns* — errorCode 500
- GET /api/profile/list — *is an endpoint of* — Leo CDP Data Observer API
- GET /api/profile/list — *is a practical way to read* — event data
- GET /api/profile/list — *embeds* — lastTrackingEvent
- GET /api/profile/list — *embeds* — behavioralEvents
- GET /api/profile/list — *embeds* — eventStatistics
- GET /api/profile/list — *embeds* — funnelStage
- GET /api/profile/list — *embeds* — funnelStageTimeline
- GET /api/profile/list — *embeds* — inJourneyMaps
- GET /api/profile/list — *ignores* — start
- GET /api/profile/list — *ignores* — limit
- pagination — *is fake for* — GET /api/profile/list
- GET /api/profile/list — *returns* — fixed page
- fixed page — *contains* — ~10 most-recent profiles
- AppsFlyer — *has a parameter called* — maximum_rows
- maximum_rows — *is silently overridden* — parameter
- examples/list_all_events.py — *implements* — paging loop
- Leo CDP public REST API contract — *is related to* — Leo CDP
- Leo CDP save returns 200 but eventlist cannot read it back — *is related to* — Leo CDP

%% ai-graph-end %%