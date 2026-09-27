---
ai_hash: b5658360645df655
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48955785254'
confluence_path: 'Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE
  on Production > Failed to store cases'
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Case Report: DocumentId 458627'
type: source
updated: 2025-12-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48955785254/Case+Report+DocumentId+458627
---

# Case Report: DocumentId 458627

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48955785254/Case+Report+DocumentId+458627) · updated 2025-12-10*

### Summary

|                        |                                    |
|------------------------|------------------------------------|
| Field                  | Value                              |
| **Case Number**        | 21 of 24                           |
| **DocumentId**         | 458627                             |
| **DeliveryId**         | 458627                             |
| **Failure Time (UTC)** | 2025-12-02T05:37:12.388Z           |
| **Root Cause**         | luz-jsonstore Connection refused   |
| **Error Code**         | DOCUMENT_IMPORT_UNKNOWN_ERROR      |
| **Cluster**            | Dec02 (second of 2)                |
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
| **FAILED_TO_STORE** | 05:37:12.388 | DocumentId 458627           |
| Deployment          | In progress  | Rolling deployment detected |

**Gap Analysis:** Second and last failure in Dec 02 mini-cluster.

------------------------------------------------------------------------

### Cluster Information

This case is part of the **December 02 Mini-Cluster**.

|                        |                               |
|------------------------|-------------------------------|
| Cluster                | Dec02                         |
| **Position**           | 2nd of 2 failures (last)      |
| **Time from previous** | ~33 minutes after case 458224 |

#### Related Cases in Dec02 Cluster

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p>#</p></th>
<th><p>DocumentId</p></th>
<th><p>Time (UTC)</p></th>
<th><p>Gap from Previous</p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>458224</p></td>
<td><p>05:04:19</p></td>
<td><ul>
<li><p>(first)</p></li>
</ul></td>
</tr>
<tr>
<td><p>**2**</p></td>
<td><p>**458627**</p></td>
<td><p>05:37:12</p></td>
<td><p>**~33 minutes**</p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Detailed Timeline

|  |  |  |
|----|----|----|
| Time (UTC) | Service | Event |
| 05:37:12.342 | luz-docs-batch | `DocumentException: jsonstore.service.unavailable` |
| 05:37:12.388 | luz-eletter | Marked as **FAILED_TO_STORE** |

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
2025-12-02 06:37:12,388 ERROR [DocumentStoreHandler] FAILED_TO_STORE
  documentId=458627
  errorCode=DOCUMENT_IMPORT_UNKNOWN_ERROR
```

#### luz-docs-batch Log

```
2025-12-02 06:37:12,342 ERROR [JsonStoreMongoService] updateOrRemoveMetadataFilterFields:
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
- [[Case Report - DocumentId 458224]]
- [[Case Report - DocumentId 588]]
- [[Case Report - DocumentId 589]]
- [[Case Report - DocumentId 3507]]
- [[Case Report - DocumentId 21579]]

%% ai-graph-end %%