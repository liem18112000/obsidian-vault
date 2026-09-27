---
ai_hash: fce7a055dcb20e8d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48951722361'
confluence_path: 'Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE
  on Production > Failed to store cases'
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Case Report: DocumentId 27748'
type: source
updated: 2025-12-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48951722361/Case+Report+DocumentId+27748
---

# Case Report: DocumentId 27748

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48951722361/Case+Report+DocumentId+27748) · updated 2025-12-10*

### Summary

|                    |                                  |
|--------------------|----------------------------------|
| Field              | Value                            |
| DocumentId         | 27748                            |
| DeliveryId         | 27748                            |
| Failure Time (UTC) | 2025-12-05T14:15:50Z             |
| Root Cause         | luz-jsonstore Connection refused |
| Error Code         | DOCUMENT_IMPORT_UNKNOWN_ERROR    |

### Failure Chain

```
┌─────────────┐    ┌────────────────────────────────┐    ┌────────────────┐    ┌───────────────┐
│ luz-eletter │───>│ luz-docs-view-controller-batch │───>│ luz-docs-batch │───>│ luz-jsonstore │
└─────────────┘    └────────────────────────────────┘    └────────────────┘    └───────┬───────┘
                                                                                       │
                                                                                       X Connection refused
                                                                                       │
┌──────────────────┐    ┌──────────┐    ┌──────────┐    ┌─────────┐                    │
│ FAILED_TO_STORE  │<───│ HTTP 500 │<───│ HTTP 503 │<───│ FAILURE │<───────────────────┘
└──────────────────┘    └──────────┘    └──────────┘    └─────────┘
```

### Root Cause Analysis

**luz-jsonstore pod was unavailable/restarting** at the time of the request.

#### Exception Details

```
javax.ws.rs.ProcessingException: RESTEASY004655: Unable to invoke request:
org.apache.http.conn.HttpHostConnectException: Connect to luz-jsonstore:8080
[luz-jsonstore/10.8.6.234] failed: Connection refused

Caused by: java.net.ConnectException: Connection refused
```

#### Failed Operation

- **Service**: luz-docs-batch

- **Method**: `JsonStoreMongoService.updateOrRemoveMetadataFilterFields()` (line 761)

- **Action**: Updating document metadata filter fields in MongoDB via luz-jsonstore

### Detailed Timeline

|  |  |
|----|----|
| Time (UTC) | Event |
| 14:15:50.009 | luz-eletter starts storing document `heidipay.pdf` |
| 14:15:50.285 | luz-docs-batch successfully creates document in luz-jsonstore (PUT /documents/add -\> 200) |
| 14:15:50.468 | luz-docs-batch fails calling luz-jsonstore for `updateOrRemoveMetadataFilterFields` |
| 14:15:50.468 | **Root Cause**: Connection refused - luz-jsonstore pod terminated |
| 14:15:50.692 | luz-docs-batch throws `DocumentException: jsonstore.service.unavailable` |
| 14:15:50.697 | luz-docs-batch returns HTTP 503 |
| 14:15:50.702 | luz-docs-view-controller-batch receives 503, logs "jsonstore.service.unavailable" |
| 14:15:50.713 | luz-docs-view-controller-batch returns HTTP 500 |
| 14:15:50.720 | luz-eletter catches `InternalServerErrorException` |
| 14:15:50.839 | luz-eletter marks document as FAILED_TO_STORE |
| 14:15:54.860 | luz-jsonstore pod starting up (JBoss Modules loading) |

### Key Insight

The first call to luz-jsonstore (PUT /documents/add at 14:15:50.285) **succeeded** because luz-jsonstore was still running. But by the time the second call (PATCH for updateOrRemoveMetadataFilterFields at 14:15:50.468) was made, **the pod had terminated and was restarting**.

### Recommendation

Same as main report - implement retry logic and improve luz-jsonstore availability during restarts.

%% ai-graph-start %%

**Related notes:**
- [[Case Report - DocumentId 8250]]
- [[Case Report - DocumentId 21579]]
- [[Case Report - DocumentId 778]]
- [[Case Report - DocumentId 386]]
- [[Case Report - DocumentId 643]]

%% ai-graph-end %%