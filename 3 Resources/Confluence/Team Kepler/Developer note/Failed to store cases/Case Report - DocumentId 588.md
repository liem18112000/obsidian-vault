---
ai_hash: 8f16bc6fcd4880f0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48951165265'
confluence_path: 'Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE
  on Production > Failed to store cases'
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Case Report: DocumentId 588'
type: source
updated: 2025-12-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48951165265/Case+Report+DocumentId+588
---

# Case Report: DocumentId 588

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48951165265/Case+Report+DocumentId+588) · updated 2025-12-10*

### Summary

|                        |                                    |
|------------------------|------------------------------------|
| Field                  | Value                              |
| **Case Number**        | 3 of 24                            |
| **DocumentId**         | 588                                |
| **DeliveryId**         | 588                                |
| **Failure Time (UTC)** | 2025-11-10T11:10:52.743Z           |
| **Root Cause**         | luz-jsonstore Connection refused   |
| **Error Code**         | DOCUMENT_IMPORT_UNKNOWN_ERROR      |
| **Cluster**            | Nov10-B (first in second wave)     |
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

|                     |              |                                  |
|---------------------|--------------|----------------------------------|
| Event               | Time (UTC)   | Details                          |
| **FAILED_TO_STORE** | 11:10:52.743 | DocumentId 588                   |
| Pod Starting        | 11:11:12.858 | `luz-jsonstore-77cfd496bc-q7m4r` |

**Gap Analysis:** Failure occurred **20 seconds before** pod restart. Pod was in the process of starting but not yet ready.

------------------------------------------------------------------------

### Detailed Timeline

|  |  |  |
|----|----|----|
| Time (UTC) | Service | Event |
| 11:10:52.743 | luz-eletter | Document storage initiated |
| 11:10:52.743 | luz-docs-batch | Attempted call to luz-jsonstore |
| 11:10:52.743 | luz-jsonstore | **Connection refused** - pod unavailable |
| 11:10:52.743 | luz-docs-batch | `DocumentException: jsonstore.service.unavailable` |
| 11:10:52.743 | luz-docs-view-controller-batch | Received HTTP 503 |
| 11:10:52.743 | luz-eletter | Marked as **FAILED_TO_STORE** |

------------------------------------------------------------------------

### Cluster Information

This case marks the **start of the second wave** in the November 10 cluster.

|                         |                                        |
|-------------------------|----------------------------------------|
| Cluster                 | Nov10-B                                |
| **Position**            | 3rd of 6 failures (1st in second wave) |
| **Gap from first wave** | 6 minutes 15 seconds                   |

#### Related Cases in Cluster

|       |            |              |                   |       |
|-------|------------|--------------|-------------------|-------|
| \#    | DocumentId | Time (UTC)   | Gap from Previous | Wave  |
| 1     | 217599     | 11:04:31.042 | \-                | A     |
| 2     | 749107     | 11:04:37.232 | 6 sec             | A     |
| **3** | **588**    | 11:10:52.743 | **6 min 15 sec**  | **B** |
| 4     | 3507       | 11:10:56.477 | 4 sec             | B     |
| 5     | 589        | 11:11:11.550 | 15 sec            | B     |
| 6     | 590        | 11:11:21.765 | 10 sec            | B     |

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
2025-11-10 12:10:52,743 ERROR [DocumentStoreHandler] FAILED_TO_STORE
  documentId=588
  errorCode=DOCUMENT_IMPORT_UNKNOWN_ERROR
```

#### luz-docs-batch Log

```
2025-11-10 12:10:52,743 ERROR [JsonStoreMongoService] updateOrRemoveMetadataFilterFields:
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
- [[Case Report - DocumentId 589]]
- [[Case Report - DocumentId 3507]]
- [[Case Report - DocumentId 386]]
- [[Case Report - DocumentId 21579]]
- [[Case Report - DocumentId 778]]

%% ai-graph-end %%