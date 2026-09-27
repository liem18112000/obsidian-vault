---
ai_hash: 6bc89b82a32984aa
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48954802213'
confluence_path: 'Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE
  on Production > Failed to store cases'
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Case Report: DocumentId 8250'
type: source
updated: 2025-12-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954802213/Case+Report+DocumentId+8250
---

# Case Report: DocumentId 8250

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954802213/Case+Report+DocumentId+8250) · updated 2025-12-10*

### Summary

|                    |                                  |
|--------------------|----------------------------------|
| Field              | Value                            |
| DocumentId         | 8250                             |
| DeliveryId         | 8250                             |
| Failure Time (UTC) | 2025-12-05T08:16:41Z             |
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

- **Method**: `JsonStoreMongoService.createDocumentMetadata()` (line 103)

- **Action**: Creating document metadata in MongoDB via luz-jsonstore

### Timeline

|  |  |
|----|----|
| Time (UTC) | Event |
| 08:16:30.463 | luz-docs-batch attempts to call luz-jsonstore |
| 08:16:30.463 | Connection refused - luz-jsonstore unavailable |
| 08:16:41.790 | luz-docs-batch throws `DocumentException: jsonstore.service.unavailable` |
| 08:16:41.871 | luz-eletter marks document as FAILED_TO_STORE |

### Recommendation

Same as main report - implement retry logic and improve luz-jsonstore availability during restarts.

%% ai-graph-start %%

**Related notes:**
- [[Case Report - DocumentId 27748]]
- [[Case Report - DocumentId 21579]]
- [[Case Report - DocumentId 386]]
- [[Case Report - DocumentId 778]]
- [[Case Report - DocumentId 643]]

%% ai-graph-end %%