---
ai_hash: 18bb80b6425c30fd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- greennode
- vngcloud
- vdb
- api
- pagination
title: GreenNode vDB list-instances API needs pageNumber/pageSize and nests results
  at data.data
type: reference
---

# GreenNode vDB list-instances API needs pageNumber/pageSize and nests results at data.data

GreenNode/VNG Cloud vDB **list database instances**: `GET https://vdb-gateway.vngcloud.vn/vdb-relational/v1/database-instances?pageNumber=1&pageSize=100` (Bearer token + `portal-user-id` header).

Two gotchas:
- **Pagination param names matter:** `pageNumber`/`pageSize` work; `page`/`size` -> HTTP 400 Bad Request (unhelpful, no detail). A 400 here is a malformed query, NOT 'no instances'.
- **Response is doubly nested:** `{ "code":200, "data": { "projectId":..., "data": [ {instance}, ... ] } }`. The instance array is at `resp['data']['data']`, not `resp['data']`. Each instance has `id` (format `db-<uuid>`), `name`, `status`, `zoneId`, `volumeType`, `ip`, etc.

Used to resolve a DB instance id by name in the vMonitor alarm setup script (leo-customer360). See [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]].

## Related

- [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]]

%% ai-graph-start %%

**Related notes:**
- [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]]
- [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]
- [[vMonitor is not in the vngcloud Terraform provider; alarms go via POST vmonitor-apiapiv1alarmsmetrics]]
- [[GreenNode Cloud is VNG Cloud rebranded (same IAM, gateway, Terraform provider)]]
- [[VNG Cloud list-projects endpoint is GET vserver-gatewayv1projects (not accounts-api)]]

%% ai-graph-end %%