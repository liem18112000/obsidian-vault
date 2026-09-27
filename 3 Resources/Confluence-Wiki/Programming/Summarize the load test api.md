---
ai_hash: 264f79157c32148f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48714743893/Summarize+the+load+test+api
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Summarize the load test api
topic: programming
type: source
updated: 2025-10-02
---

# Summarize the load test api

> [!info] Imported from Confluence
> Space **TS** · updated 2025-10-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48714743893/Summarize+the+load+test+api)
> Relevance 0.731 · topic `programming`

<div>

|  |  |  |  |
|----|----|----|----|
| **Service name** | **Endpoint (URL)** |  | **Background** |
| luz_communities | **POST** /matrix-account/exists | Check if the Tenant has a matrix account | **Tuwunel server (1 ..5)**: \_matrix/client/v3/login |
| luz_communities | **POST** /login | Log in to the Tuwunel server by Tenant Token and create a new Matrix ID if needed | **Tuwunel server (1 ..5)**: \_matrix/client/v3/login |
| luz_communities | **POST** /jwt | Generate the JWT token for Tuwunel Server |  |
| luz_communities | **POST** /epost-directory/rooms-mapping/find-by-ids |  | **luz-cached:** not apply yet |
| luz_communities | **GET** /{tenantId}/epost-directory/tenant-mapping |  | **luz-cached:** not apply yet |
| luz_communities | **POST** /epost-directory/users-mapping/find-by-ids |  | **luz-cached:** not apply yet |
| luz_communities | **POST** /epost-directory/users-mapping/find-by-tenant-ids |  |  |
| luz_communities | **PUT** /\${tenantId}/epost-directory/rooms-mapping/\${roomId} | Update room mapping data | **luz-cached:** not apply yet |
| Tuwunel Server | **PUT** /\_matrix/client/v3/rooms/{roomId}/send/m.room.message/{transactionId} | Send a message to a room |  |
| Tuwunel Server | **GET** /\_matrix/client/v3/sync | Long poll api that syncs the new message to the client |  |
| Tuwunel Server | **POST** /\_matrix/client/v3/user/\${encodeURIComponent(matrixId)}/filter |  |  |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Load test get document id API]]
- [[IVY API Calls Overview for luz Modules]]
- [[Load Test Physical Order API]]
- [[Token JWT Security]]
- [[HowToUseNewTokenAPI]]

%% ai-graph-end %%