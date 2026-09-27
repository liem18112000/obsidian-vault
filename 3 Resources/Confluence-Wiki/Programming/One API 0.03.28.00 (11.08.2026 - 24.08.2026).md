---
ai_hash: f8607948850900b1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49691623452/One+API+0.03.28.00+11.08.2026+-+24.08.2026
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: One API 0.03.28.00 (11.08.2026 - 24.08.2026)
topic: programming
type: source
updated: 2026-08-24
---

# One API 0.03.28.00 (11.08.2026 - 24.08.2026)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-08-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49691623452/One+API+0.03.28.00+11.08.2026+-+24.08.2026)
> Relevance 0.724 · topic `programming`

## Publishing postal-services-ordinance businessStatus events

Rolls out the same `businessStatus` state machine — `RECEIVED` → `ACCEPTED` → `DELIVERED`/`FAILURE` — across the SMS, EBILL, and EMAIL channels. Each transition is dual-persisted (Postgres + MongoDB), appended to `businessStatusHistory`, and published as a status-update event so `luz_ecp_log` can seal it for the ordinance audit trail.

**SMS**

- <a href="https://axonivy.atlassian.net/browse/LUZ-147298" class="external-link" rel="nofollow">LUZ-147298</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="6d9cc103-0bdc-4eaa-a74a-2ac9722b7d7a" macro-name="status">DONE</span> — `ACCEPTED` immediately before handoff to the SMS provider.

- <a href="https://axonivy.atlassian.net/browse/LUZ-147310" class="external-link" rel="nofollow">LUZ-147310</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="7421cb5a-a41f-43d9-9c65-53a5e0bb434c" macro-name="status">DONE</span> — `DELIVERED` once the SMS provider confirms the transfer.

- <a href="https://axonivy.atlassian.net/browse/LUZ-147306" class="external-link" rel="nofollow">LUZ-147306</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" hasbody="false" macro-id="de472384-f8bf-4127-9a72-1f601868bf07" macro-name="status">REVIEW</span> — `FAILURE` when the provider call comes back negative (no retry, no channel switch).

**EBILL**

- <a href="https://axonivy.atlassian.net/browse/LUZ-147297" class="external-link" rel="nofollow">LUZ-147297</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="957cb94c-dfc1-48e9-8b50-24f7e8d34708" macro-name="status">DONE</span> — `ACCEPTED` before the transfer to SIX.

- <a href="https://axonivy.atlassian.net/browse/LUZ-147309" class="external-link" rel="nofollow">LUZ-147309</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" hasbody="false" macro-id="cdfdb998-47be-4c4b-8ac8-98153ac7a68e" macro-name="status">REVIEW</span> — `DELIVERED` once SIX reports the eBill status as `OPEN`.

- <a href="https://axonivy.atlassian.net/browse/LUZ-147303" class="external-link" rel="nofollow">LUZ-147303</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" hasbody="false" macro-id="81141d8d-f7a1-4b96-92d6-49779392a780" macro-name="status">REVIEW</span> — `FAILURE` when the transfer/call to SIX fails terminally.

- <a href="https://axonivy.atlassian.net/browse/LUZ-157491" class="external-link" rel="nofollow">LUZ-157491</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" hasbody="false" macro-id="d223959a-febf-4e84-a825-3b3e031a344c" macro-name="status">REVIEW</span> — `FAILURE` for validation/recipient-match errors found on the `RECEIVED` message, before it ever reaches SIX (complements LUZ-147303).

**EMAIL**

- <a href="https://axonivy.atlassian.net/browse/LUZ-149472" class="external-link" rel="nofollow">LUZ-149472</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="421158cc-4adf-4e96-9350-65d0d94477c5" macro-name="status">DONE</span> — sets `RECEIVED` for CC/BCC messages created by zone-mta, so each recipient gets its own trackable message.

- <a href="https://axonivy.atlassian.net/browse/LUZ-157915" class="external-link" rel="nofollow">LUZ-157915</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="758cdfb0-40a0-477e-9b21-267724706019" macro-name="status">DONE</span> — `FAILURE` when the synchronous handoff to zone-mta fails terminally.

- <a href="https://axonivy.atlassian.net/browse/LUZ-157489" class="external-link" rel="nofollow">LUZ-157489</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="ed524696-b1ac-444d-9fc5-80d86ed08ab4" macro-name="status">DONE</span> — `FAILURE` when a `RECEIVED` message fails validation before handoff.

- <a href="https://axonivy.atlassian.net/browse/LUZ-147302" class="external-link" rel="nofollow">LUZ-147302</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="f422c602-a7b6-4e45-9332-025efb7fe91e" macro-name="status">DONE</span> — `FAILURE` when zone-mta itself reports it could not deliver the message.

- <a href="https://axonivy.atlassian.net/browse/LUZ-147311" class="external-link" rel="nofollow">LUZ-147311</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-current conf-macro output-inline" hasbody="false" macro-id="445b0089-3278-4999-8947-a6117775419e" macro-name="status">REVIEW</span> — `DELIVERED` per recipient once zone-mta confirms delivery, matching each To/CC/BCC callback to its own message.

## Performance enhancements

<a href="https://axonivy.atlassian.net/browse/LUZ-156137" class="external-link" rel="nofollow">LUZ-156137</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="ac3501a7-fe6e-41c2-bb7d-16da83d78852" macro-name="status">DONE</span> — the Preview-Price API (`luz_eletter`) was crash-looping under real production load. We removed the root cause and validated the fix with a load test at the same scale as the incident.

**What was happening:** under sustained load, price-preview requests piled up.  
Once workers thread were saturated, Kubernetes couldn't get a health-check response in time and killed the pods — which restarted and immediately hit the same wall.

**What we changed:** move the preview api processing flow with message queue, removed a database lock, and create database indexes


![[49691623452-image-20260824-045402.png]]

![[49691623452-image-20260824-050023.png]]



## Additional

- <a href="https://axonivy.atlassian.net/browse/LUZ-157887" class="external-link" rel="nofollow">LUZ-157887</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="644a1cb1-dc2f-4bb5-a88f-35b09b1fab00" macro-name="status">DONE</span> — fixed a pricing bug eventhough billing is correct the price showing zero *price* values in the Monitoring Journal.

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><img src="https://media-cdn.atlassian.com/file/346ef75f-e6bb-40b5-87c7-e038c2c1ffa0/image/cdn?allowAnimated=true&amp;client=ec1deedb-e10d-4d5f-8686-39b6639540f4&amp;collection=&amp;height=125&amp;max-age=2592000&amp;mode=full-fit&amp;source=mediaCard&amp;token=eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJlYzFkZWVkYi1lMTBkLTRkNWYtODY4Ni0zOWI2NjM5NTQwZjQiLCJhY2Nlc3MiOnsidXJuOmZpbGVzdG9yZTpmaWxlOjk0OGZlZDJmLTVkOWYtNDVjYS1iOWI3LWI4MzQ3N2FhODBiMiI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTphNGUyMDU0NC03NTczLTQwNGMtOTRmOC1iM2U3YzE3YWI3YzIiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6MDFmMzYzMGUtNTY0Yy00M2E3LWE4ZjMtZDgzMjNjZDE0YjE2IjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOmIxNTFiMjQ4LTBjMzctNGI5OS1hOGUzLTc3OTAxYzI0MzFkYyI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTpiN2M1Mzc0MS1iY2JmLTRlOWItYjEyYi0zZjRhOWEyNGFlOGMiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6YzZjYzUyMzQtNmI2Ni00YTZhLWJkNGItMGRhYWYyNDNiNGZiIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjcxODdlMTUwLTBjN2QtNDhlNS05NTY1LTNhM2QxYTg4ZWFmOSI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTpiYjI1OTY3MC04YWI2LTRmMWItOWZjNC05NDkyZWUxNTA1YWQiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6MWMwMzZkM2UtM2FiNi00ZjNhLThkOWMtZTU3NTQ0ODliYTdjIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOmQ2NzNjMmY0LTFkM2YtNDJlOS05Njc4LTYwMDZlZmJjNjA1NCI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTpjZDA1ZTA4MC02YTNjLTQ2NDktODAzMC04ZWM4N2NmNmNkZWQiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6MmQ1OTE4NTAtMzE1Ni00YzZhLTliZjktYWZjN2Y2NTM5Y2YzIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjM0NmVmNzVmLWU2YmItNDBiNS04N2M3LWUwMzhjMmMxZmZhMCI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTowODVlZjljNC0yMWUyLTQ5OTEtODE1NS02NGEwZDkxN2UwZWQiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6MzgyMmY2NDYtOWQ1ZC00NzlmLTkwNzUtYmZmMTg4NTNhYTAzIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjM4YTdiNzI2LTc4MTYtNDhhYy1iMTBkLTc0MjIzYmVmNzdhYSI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZToyYTgzYmVkNi01NmY0LTQ4NDAtYTgwOS0wOGQzOTQzZTE4OTYiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6Zjc0MzU1ZTMtMTNlOS00OGVlLWExMjMtZDcyMmVjOGRjY2MzIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOmIwN2I4Mzk4LWRlYWQtNDNiOC1iYTdkLTNjZDQwMzJmNjg1OSI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZToyNGNlZjNkNi0xZDdhLTRhY2YtODU4Yy01MGEzYWQ1MDJiM2MiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6ZTIyZWIwMzItNzc0MS00ZGIyLTkzNGQtZmE3NzkwZGQ2NWEzIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjBiMDVkNjE4LWRjYzYtNDgyYS04MzFkLWI4YWNmMmQ0ZmJhNCI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTphYzUxZjQxMS0wZDIxLTRlODItODA1My0wYzc4YTEyN2RhMTMiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6ZGQ1MjhkZjYtMWI2MC00NzkwLWIyOWItMjNiNzA1YjRmMmQyIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjc2ZDAwY2JiLTFmZmMtNDdkYi1hMTUzLWJhMjgwZjE1ZjcxZCI6WyJyZWFkIl19LCJleHAiOjE3ODc1NTQ4ODUsIm5iZiI6MTc4NzU1Mzk4NSwiYWFJZCI6IjYzNDM3YmM1M2Y0MjI0YTQ2ZGIxZGY1YiIsImh0dHBzOi8vaWQuYXRsYXNzaWFuLmNvbS9hcHBBY2NyZWRpdGVkIjpmYWxzZX0.XmV9MhF1PnH2_TQUHqqIJC8GJViUL_D4aBoLncmOdnY&amp;width=1350#media-blob-url=true&amp;id=346ef75f-e6bb-40b5-87c7-e038c2c1ffa0&amp;clientId=ec1deedb-e10d-4d5f-8686-39b6639540f4&amp;contextId=&amp;collection=" class="confluence-embedded-image confluence-external-resource image-center" data-image-src="https://media-cdn.atlassian.com/file/346ef75f-e6bb-40b5-87c7-e038c2c1ffa0/image/cdn?allowAnimated=true&amp;client=ec1deedb-e10d-4d5f-8686-39b6639540f4&amp;collection=&amp;height=125&amp;max-age=2592000&amp;mode=full-fit&amp;source=mediaCard&amp;token=eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJlYzFkZWVkYi1lMTBkLTRkNWYtODY4Ni0zOWI2NjM5NTQwZjQiLCJhY2Nlc3MiOnsidXJuOmZpbGVzdG9yZTpmaWxlOjk0OGZlZDJmLTVkOWYtNDVjYS1iOWI3LWI4MzQ3N2FhODBiMiI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTphNGUyMDU0NC03NTczLTQwNGMtOTRmOC1iM2U3YzE3YWI3YzIiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6MDFmMzYzMGUtNTY0Yy00M2E3LWE4ZjMtZDgzMjNjZDE0YjE2IjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOmIxNTFiMjQ4LTBjMzctNGI5OS1hOGUzLTc3OTAxYzI0MzFkYyI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTpiN2M1Mzc0MS1iY2JmLTRlOWItYjEyYi0zZjRhOWEyNGFlOGMiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6YzZjYzUyMzQtNmI2Ni00YTZhLWJkNGItMGRhYWYyNDNiNGZiIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjcxODdlMTUwLTBjN2QtNDhlNS05NTY1LTNhM2QxYTg4ZWFmOSI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTpiYjI1OTY3MC04YWI2LTRmMWItOWZjNC05NDkyZWUxNTA1YWQiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6MWMwMzZkM2UtM2FiNi00ZjNhLThkOWMtZTU3NTQ0ODliYTdjIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOmQ2NzNjMmY0LTFkM2YtNDJlOS05Njc4LTYwMDZlZmJjNjA1NCI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTpjZDA1ZTA4MC02YTNjLTQ2NDktODAzMC04ZWM4N2NmNmNkZWQiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6MmQ1OTE4NTAtMzE1Ni00YzZhLTliZjktYWZjN2Y2NTM5Y2YzIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjM0NmVmNzVmLWU2YmItNDBiNS04N2M3LWUwMzhjMmMxZmZhMCI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTowODVlZjljNC0yMWUyLTQ5OTEtODE1NS02NGEwZDkxN2UwZWQiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6MzgyMmY2NDYtOWQ1ZC00NzlmLTkwNzUtYmZmMTg4NTNhYTAzIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjM4YTdiNzI2LTc4MTYtNDhhYy1iMTBkLTc0MjIzYmVmNzdhYSI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZToyYTgzYmVkNi01NmY0LTQ4NDAtYTgwOS0wOGQzOTQzZTE4OTYiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6Zjc0MzU1ZTMtMTNlOS00OGVlLWExMjMtZDcyMmVjOGRjY2MzIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOmIwN2I4Mzk4LWRlYWQtNDNiOC1iYTdkLTNjZDQwMzJmNjg1OSI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZToyNGNlZjNkNi0xZDdhLTRhY2YtODU4Yy01MGEzYWQ1MDJiM2MiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6ZTIyZWIwMzItNzc0MS00ZGIyLTkzNGQtZmE3NzkwZGQ2NWEzIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjBiMDVkNjE4LWRjYzYtNDgyYS04MzFkLWI4YWNmMmQ0ZmJhNCI6WyJyZWFkIl0sInVybjpmaWxlc3RvcmU6ZmlsZTphYzUxZjQxMS0wZDIxLTRlODItODA1My0wYzc4YTEyN2RhMTMiOlsicmVhZCJdLCJ1cm46ZmlsZXN0b3JlOmZpbGU6ZGQ1MjhkZjYtMWI2MC00NzkwLWIyOWItMjNiNzA1YjRmMmQyIjpbInJlYWQiXSwidXJuOmZpbGVzdG9yZTpmaWxlOjc2ZDAwY2JiLTFmZmMtNDdkYi1hMTUzLWJhMjgwZjE1ZjcxZCI6WyJyZWFkIl19LCJleHAiOjE3ODc1NTQ4ODUsIm5iZiI6MTc4NzU1Mzk4NSwiYWFJZCI6IjYzNDM3YmM1M2Y0MjI0YTQ2ZGIxZGY1YiIsImh0dHBzOi8vaWQuYXRsYXNzaWFuLmNvbS9hcHBBY2NyZWRpdGVkIjpmYWxzZX0.XmV9MhF1PnH2_TQUHqqIJC8GJViUL_D4aBoLncmOdnY&amp;width=1350#media-blob-url=true&amp;id=346ef75f-e6bb-40b5-87c7-e038c2c1ffa0&amp;clientId=ec1deedb-e10d-4d5f-8686-39b6639540f4&amp;contextId=&amp;collection=" loading="lazy" width="740" alt="image-20260807-023945.png" /></span>

![[49691623452-image-20260805-034513.png]]



- <a href="https://axonivy.atlassian.net/browse/LUZ-157767" class="external-link" rel="nofollow">LUZ-157767</a> <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="1531c3f5-ff17-4bf7-9dbc-d285c668932c" macro-name="status">DONE</span> — fixed sync data to public table for when admin resent delivery.

![[49691623452-KLARA (1) (1).mp4]]

%% ai-graph-start %%

**Related notes:**
- [[Copy 5. How to extend modify ONE API delivery API Research]]
- [[5. How to extend modify ONE API delivery API Research]]
- [[OneAPI Architecture overview]]
- [[ePost API (28.02.2023 - 13.03.2023)]]
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]

%% ai-graph-end %%