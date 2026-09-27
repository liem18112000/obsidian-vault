---
title: "Case Report: DocumentId 590"
created: 2025-12-10
updated: 2025-12-10
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954015851/Case+Report+DocumentId+590
confluence_id: "48954015851"
confluence_path: "Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE on Production > Failed to store cases"
tags: [confluence]
---

# Case Report: DocumentId 590

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954015851/Case+Report+DocumentId+590) · updated 2025-12-10*

### Summary

|                        |                                    |
|------------------------|------------------------------------|
| Field                  | Value                              |
| **Case Number**        | 6 of 24                            |
| **DocumentId**         | 590                                |
| **DeliveryId**         | 590                                |
| **Failure Time (UTC)** | 2025-11-10T11:11:21.765Z           |
| **Root Cause**         | luz-jsonstore Connection refused   |
| **Error Code**         | DOCUMENT_IMPORT_UNKNOWN_ERROR      |
| **Cluster**            | Nov10-B (last failure)             |
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
| Pod Starting        | 11:11:12.858 | `luz-jsonstore-77cfd496bc-q7m4r` |
| **FAILED_TO_STORE** | 11:11:21.765 | DocumentId 590                   |

**Gap Analysis:** Failure occurred **9 seconds after** pod started loading but pod was still initializing (WildFly takes 10-17s to start, plus MongoDB connection time).

------------------------------------------------------------------------

### Detailed Timeline

|  |  |  |
|----|----|----|
| Time (UTC) | Service | Event |
| 11:11:12.858 | Kubernetes | New pod `luz-jsonstore-77cfd496bc-q7m4r` starting |
| 11:11:21.765 | luz-eletter | Document storage initiated |
| 11:11:21.765 | luz-docs-batch | Attempted call to luz-jsonstore |
| 11:11:21.765 | luz-jsonstore | **Connection refused** - pod still initializing |
| 11:11:21.765 | luz-docs-batch | `DocumentException: jsonstore.service.unavailable` |
| 11:11:21.765 | luz-docs-view-controller-batch | Received HTTP 503 |
| 11:11:21.765 | luz-eletter | Marked as **FAILED_TO_STORE** |

------------------------------------------------------------------------

### Cluster Information

This is the **last failure** in the November 10 Major Cluster.

|                        |                                    |
|------------------------|------------------------------------|
| Cluster                | Nov10-B                            |
| **Position**           | 6th of 6 failures (last)           |
| **Time from previous** | 10 seconds after case 589          |
| **Note**               | Pod was starting but not ready yet |

#### Related Cases in Cluster

|       |            |              |                   |              |
|-------|------------|--------------|-------------------|--------------|
| \#    | DocumentId | Time (UTC)   | Gap from Previous | Wave         |
| 1     | 217599     | 11:04:31.042 | \-                | A            |
| 2     | 749107     | 11:04:37.232 | 6 sec             | A            |
| 3     | 588        | 11:10:52.743 | 6 min 15 sec      | B            |
| 4     | 3507       | 11:10:56.477 | 4 sec             | B            |
| 5     | 589        | 11:11:11.550 | 15 sec            | B            |
| **6** | **590**    | 11:11:21.765 | **10 sec**        | **B (last)** |

#### Cluster Summary

|                    |                            |
|--------------------|----------------------------|
| Metric             | Value                      |
| **Total Duration** | 6 min 50 sec               |
| **Total Failures** | 6 documents                |
| **Root Cause**     | Extended deployment outage |

------------------------------------------------------------------------

### Error Propagation

```
luz-jsonstore:     Connection refused (pod initializing)
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
2025-11-10 12:11:21,765 ERROR [DocumentStoreHandler] FAILED_TO_STORE
  documentId=590
  errorCode=DOCUMENT_IMPORT_UNKNOWN_ERROR
```

#### luz-docs-batch Log

```
2025-11-10 12:11:21,765 ERROR [JsonStoreMongoService] updateOrRemoveMetadataFilterFields:
  ch.klara.luzdocs.core.exception.DocumentException: jsonstore.service.unavailable
```

#### Pod Startup Log

```
2025-11-10 12:11:12,858 INFO [org.jboss.modules] JBoss Modules version 2.0.2.Final
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

### Key Insight

This case demonstrates the **file-based readiness probe problem**: The pod had started (JBoss Modules loading at 11:11:12), but it wasn't ready to serve traffic yet. The file-based probe (`luz_jsonstore.war.deployed`) likely passed, but the application wasn't fully initialized.

------------------------------------------------------------------------

### Recommendation

1.  **Implement HTTP readiness probe** instead of file-based probe

2.  **Add startup probe** to protect during initialization

3.  **Implement graceful shutdown** with preStop hook

4.  **Add retry logic** in luz-docs-batch for transient failures

See main report for detailed fix implementation.
