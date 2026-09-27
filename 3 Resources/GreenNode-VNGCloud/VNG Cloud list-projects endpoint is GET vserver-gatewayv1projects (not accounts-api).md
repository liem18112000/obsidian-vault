---
ai_hash: 9c10459f3c31a842
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- greennode
- vngcloud
- api
- project
- iam
title: VNG Cloud list-projects endpoint is GET vserver-gateway/v1/projects (not accounts-api)
type: reference
---

# VNG Cloud list-projects endpoint is GET vserver-gateway/v1/projects (not accounts-api)

To get your VNG Cloud/GreenNode **project id** (`pro-...`) via API (not just the console): authenticate at `https://iamapis.vngcloud.vn/accounts-api/v2/auth/token` (OAuth2 client_credentials, Basic auth) for a Bearer token, then:

`GET https://hcm-3.api.vngcloud.vn/vserver/vserver-gateway/v1/projects`  (header `Authorization: Bearer <token>`)

returns the account's projects; walk the JSON for values matching `^pro-`.

**Endpoints that do NOT work (tested):** `accounts-api/v2/projects` -> 403; `vstorage-gateway/v1/projects` -> 404; `vserver-gateway/**v2**/projects` -> 404. The IAM Accounts API has NO project-list endpoint (only `/v1/auth/userinfo` = userId/accountId, no project). So the **v1 vserver-gateway** path is the one that works.

Why it matters: `vngcloud_vserver_network`/`_subnet` Terraform resources REQUIRE `project_id`, but the console/docs bury it. See [[Terraform optional-resource toggle create-or-reuse via count + a local that picks the id|Terraform optional-resource toggle: create-or-reuse via count + a local that picks the id]] and [[GreenNode Cloud is VNG Cloud rebranded (same IAM, gateway, Terraform provider)]].

## Related

- [[Terraform optional-resource toggle create-or-reuse via count + a local that picks the id|Terraform optional-resource toggle: create-or-reuse via count + a local that picks the id]]

%% ai-graph-start %%

**Related notes:**
- [[vStorage REST control-plane API endpoints and vIAM bearer auth]]
- [[GreenNode Cloud is VNG Cloud rebranded (same IAM, gateway, Terraform provider)]]
- [[VNG Cloud resource ID prefixes and the HCM zone_id label gotcha]]
- [[VNG Cloud vServer discovering account catalog names via the vserver-gateway API]]
- [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]

%% ai-graph-end %%