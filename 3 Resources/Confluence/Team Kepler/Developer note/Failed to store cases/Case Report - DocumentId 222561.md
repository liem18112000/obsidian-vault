---
title: "Case Report: DocumentId 222561"
created: 2025-12-10
updated: 2025-12-10
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48953950284/Case+Report+DocumentId+222561
confluence_id: "48953950284"
confluence_path: "Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE on Production > Failed to store cases"
tags: [confluence]
---

# Case Report: DocumentId 222561

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48953950284/Case+Report+DocumentId+222561) · updated 2025-12-10*

### Summary

|                        |                                    |
|------------------------|------------------------------------|
| Field                  | Value                              |
| **Case Number**        | 15 of 24                           |
| **DocumentId**         | 222561                             |
| **DeliveryId**         | 222561                             |
| **Failure Time (UTC)** | 2025-11-25T07:10:26.538Z           |
| **Root Cause**         | luz-jsonstore Connection refused   |
| **Error Code**         | DOCUMENT_IMPORT_UNKNOWN_ERROR      |
| **Cluster**            | Nov25 (second of 5)                |
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
| **FAILED_TO_STORE** | 07:10:26.538 | DocumentId 222561           |
| Deployment          | In progress  | Rolling deployment detected |

**Gap Analysis:** Second failure in Nov 25 spread cluster.

------------------------------------------------------------------------

### Cluster Information

This case is part of the **November 25 Spread Cluster**.

|                        |                            |
|------------------------|----------------------------|
| Cluster                | Nov25                      |
| **Position**           | 2nd of 5 failures          |
| **Time from previous** | ~2 hours after case 222042 |

#### Related Cases in Nov25 Cluster

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
<td><p>222042</p></td>
<td><p>05:00:28</p></td>
<td><ul>
<li><p>(first)</p></li>
</ul></td>
</tr>
<tr>
<td><p>**2**</p></td>
<td><p>**222561**</p></td>
<td><p>07:10:26</p></td>
<td><p>**~2 hours**</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>223411</p></td>
<td><p>09:00:15</p></td>
<td><p>~2 hours</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>223412</p></td>
<td><p>09:00:19</p></td>
<td><p>4 seconds</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>1741</p></td>
<td><p>14:16:21</p></td>
<td><p>~5 hours</p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Detailed Timeline

|  |  |  |
|----|----|----|
| Time (UTC) | Service | Event |
| 07:10:26.538 | luz-eletter | Document storage initiated |
| 07:10:26.538 | luz-docs-batch | Attempted call to luz-jsonstore |
| 07:10:26.538 | luz-jsonstore | **Connection refused** - pod unavailable |
| 07:10:26.538 | luz-docs-batch | `DocumentException: jsonstore.service.unavailable` |
| 07:10:26.538 | luz-docs-view-controller-batch | Received HTTP 503 |
| 07:10:26.538 | luz-eletter | Marked as **FAILED_TO_STORE** |

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
2025-11-25 08:10:26,538 ERROR [DocumentStoreHandler] FAILED_TO_STORE
  documentId=222561
  errorCode=DOCUMENT_IMPORT_UNKNOWN_ERROR
```

#### luz-docs-batch Log

```
2025-11-25 08:10:26,538 ERROR [JsonStoreMongoService] updateOrRemoveMetadataFilterFields:
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
