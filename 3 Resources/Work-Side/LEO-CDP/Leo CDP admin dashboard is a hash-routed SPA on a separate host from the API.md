---
ai_hash: 5ea98f14df53a265
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-30
entities: []
source: session 2026-06-30; user-provided admin link
status: seedling
tags:
- leo-cdp
- dashboard
- spa
- reference
title: Leo CDP admin dashboard is a hash-routed SPA on a separate host from the API
type: reference
---

# Leo CDP admin dashboard is a hash-routed SPA on a separate host from the API

The Leo CDP **admin dashboard** and the **ingestion API** live on **different hosts**:

- API (Data Observer): `datahub4dcdp.bigdatavietnam.org` — `/api/profile/save`, `/api/event/save`, `/api/event/list`.
- Dashboard (admin UI): `dcdp.bigdatavietnam.org` — a **hash-routed single-page app**.

The SPA navigates via a JS router encoded in the URL fragment:

    https://dcdp.bigdatavietnam.org/#calljs-leoCdpRouter('<ViewName>','<param>')

Example given for the journey-map view: `leoCdpRouter('Data_Journey_Map','')`. To deep-link a profile by id, the view name is (best guess) `Profile_Details` with the profile id as the second arg:

    https://dcdp.bigdatavietnam.org/#calljs-leoCdpRouter('Profile_Details','<profileId>')

**Why it matters:** because the API returns 200 on save but the events aren't immediately queryable via `event/list` (see [[Leo CDP save returns 200 but eventlist cannot read it back]]), the dashboard is the practical way to visually confirm a push landed. The connector's `examples/push_to_data_observer.py` bakes this as the default `LEO_CDP_DASHBOARD_URL` template (override env var if the route differs). Related: [[Leo CDP public REST API contract]].

## Related

- [[Leo CDP save returns 200 but eventlist cannot read it back]]
- [[Leo CDP public REST API contract]]

%% ai-graph-start %%

**Related notes:**
- [[Leo CDP save returns 200 but eventlist cannot read it back]]
- [[Leo CDP public REST API contract]]
- [[Leo CDP profilelist ignores start and limit and embeds event data]]
- [[Leo CDP event observerId is the pushing tokenkey and eventsave can split from profilesave identity]]
- [[Verify uat customer360-api health publicly at beta.leocdp.comc360apihealth]]

%% ai-graph-end %%