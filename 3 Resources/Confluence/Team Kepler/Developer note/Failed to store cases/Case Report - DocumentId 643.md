---
ai_hash: 018b0bb8d6a39e80
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48955654177'
confluence_path: 'Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE
  on Production > Failed to store cases'
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Case Report: DocumentId 643'
type: source
updated: 2025-12-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48955654177/Case+Report+DocumentId+643
---

# Case Report: DocumentId 643

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48955654177/Case+Report+DocumentId+643) · updated 2025-12-10*

### Summary

|                        |                                    |
|------------------------|------------------------------------|
| Field                  | Value                              |
| **Case Number**        | 7 of 24                            |
| **DocumentId**         | 643                                |
| **DeliveryId**         | 643                                |
| **Failure Time (UTC)** | 2025-11-11T17:16:41.579Z           |
| **Root Cause**         | luz-jsonstore Connection refused   |
| **Error Code**         | DOCUMENT_IMPORT_UNKNOWN_ERROR      |
| **Cluster**            | Isolated                           |
| **Failed Operation**   | updateOrRemoveMetadataFilterFields |

------------------------------------------------------------------------

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

------------------------------------------------------------------------

### Root Cause Analysis

**luz-jsonstore pod was unavailable during rolling deployment** at the time of the request.

#### Exception Details

```
javax.ws.rs.ProcessingException: RESTEASY004655: Unable to invoke request:
org.apache.http.conn.HttpHostConnectException: Connect to luz-jsonstore:8080
[luz-jsonstore/10.8.6.234] failed: Connection refused

Caused by: java.net.ConnectException: Connection refused
    at java.base/sun.nio.ch.Net.pollConnect(Native Method)
    at java.base/sun.nio.ch.Net.pollConnectNow(Net.java:672)
    at java.base/sun.nio.ch.NioSocketImpl.timedFinishConnect(NioSocketImpl.java:542)
```

#### Failed Operation

|  |  |
|----|----|
| Field | Value |
| **Service** | luz-docs-batch |
| **Method** | `JsonStoreMongoService.updateOrRemoveMetadataFilterFields()` |
| **Line** | 761 |
| **Action** | Updating document metadata filter fields in MongoDB via luz-jsonstore |

------------------------------------------------------------------------

### Pod Restart Evidence

|                     |              |                             |
|---------------------|--------------|-----------------------------|
| Event               | Time (UTC)   | Details                     |
| **FAILED_TO_STORE** | 17:16:41.579 | DocumentId 643              |
| Deployment          | In progress  | Rolling deployment detected |

**Gap Analysis:** Isolated failure during deployment window.

------------------------------------------------------------------------

### Detailed Timeline

|  |  |  |
|----|----|----|
| Time (UTC) | Service | Event |
| 17:16:41.579 | luz-eletter | Document storage initiated |
| 17:16:41.579 | luz-docs-batch | Attempted call to luz-jsonstore |
| 17:16:41.579 | luz-jsonstore | **Connection refused** - pod unavailable |
| 17:16:41.579 | luz-docs-batch | `DocumentException: jsonstore.service.unavailable` |
| 17:16:41.579 | luz-docs-view-controller-batch | Received HTTP 503 |
| 17:16:41.579 | luz-eletter | Marked as **FAILED_TO_STORE** |

------------------------------------------------------------------------

### Error Propagation

```
luz-jsonstore:     Connection refused
       ↓
luz-docs-batch:    DocumentException → HTTP 503
       ↓
luz-docs-view-controller-batch: ServiceUnavailableException → HTTP 500
       ↓
luz-eletter:       InternalServerError → FAILED_TO_STORE
```

------------------------------------------------------------------------

### Log Evidence

#### luz-eletter Log

```
2025-11-11 18:16:41,579 ERROR [DocumentStoreHandler] FAILED_TO_STORE
  documentId=643
  errorCode=DOCUMENT_IMPORT_UNKNOWN_ERROR
```

#### luz-docs-batch Log

```
2025-11-11 18:16:41,579 ERROR [JsonStoreMongoService] updateOrRemoveMetadataFilterFields:
  ch.klara.luzdocs.core.exception.DocumentException: jsonstore.service.unavailable
```

------------------------------------------------------------------------

### Impact Assessment

|                        |                                  |
|------------------------|----------------------------------|
| Metric                 | Value                            |
| **Documents Affected** | 1                                |
| **User Impact**        | Document failed to store         |
| **Data Loss**          | No (document can be reprocessed) |
| **Recovery**           | Manual retry required            |

------------------------------------------------------------------------

### Recommendation

1.  **Implement HTTP readiness probe** instead of file-based probe

2.  **Add startup probe** to protect during initialization

3.  **Implement graceful shutdown** with preStop hook

4.  **Add retry logic** in luz-docs-batch for transient failures

See main report for detailed fix implementation.

%% ai-graph-start %%

**Related notes:**
- [[Case Report - DocumentId 386]]
- [[Case Report - DocumentId 21579]]
- [[Case Report - DocumentId 18219]]
- [[Case Report - DocumentId 778]]
- [[Case Report - DocumentId 20211]]

%% ai-graph-end %%