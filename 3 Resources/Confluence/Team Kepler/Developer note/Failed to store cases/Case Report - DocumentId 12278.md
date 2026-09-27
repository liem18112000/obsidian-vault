---
ai_hash: 92beaed88069f467
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48955490347'
confluence_path: 'Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE
  on Production > Failed to store cases'
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Case Report: DocumentId 12278'
type: source
updated: 2025-12-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48955490347/Case+Report+DocumentId+12278
---

# Case Report: DocumentId 12278

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48955490347/Case+Report+DocumentId+12278) · updated 2025-12-10*

### Summary

|                        |                                    |
|------------------------|------------------------------------|
| Field                  | Value                              |
| **Case Number**        | 11 of 24                           |
| **DocumentId**         | 12278                              |
| **DeliveryId**         | 12278                              |
| **Failure Time (UTC)** | 2025-11-18T11:15:56.256Z           |
| **Root Cause**         | luz-jsonstore Connection refused   |
| **Error Code**         | DOCUMENT_IMPORT_UNKNOWN_ERROR      |
| **Cluster**            | Nov18 (third of 3)                 |
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
| **FAILED_TO_STORE** | 11:15:56.256 | DocumentId 12278            |
| Deployment          | In progress  | Rolling deployment detected |

**Gap Analysis:** Last failure in Nov 18 spread cluster.

------------------------------------------------------------------------

### Cluster Information

This case is the **last failure** in the November 18 Spread Cluster.

|                        |                           |
|------------------------|---------------------------|
| Cluster                | Nov18                     |
| **Position**           | 3rd of 3 failures (last)  |
| **Time from previous** | ~3 hours after case 20512 |
| **Total Duration**     | ~11 hours                 |

#### Related Cases in Nov18 Cluster

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
<td><p>218735</p></td>
<td><p>00:02:40</p></td>
<td><ul>
<li><p>(first)</p></li>
</ul></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>20512</p></td>
<td><p>08:16:22</p></td>
<td><p>~8 hours</p></td>
</tr>
<tr>
<td><p>**3**</p></td>
<td><p>**12278**</p></td>
<td><p>11:15:56</p></td>
<td><p>**~3 hours**</p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Detailed Timeline

|  |  |  |
|----|----|----|
| Time (UTC) | Service | Event |
| 11:15:56.256 | luz-eletter | Document storage initiated |
| 11:15:56.256 | luz-docs-batch | Attempted call to luz-jsonstore |
| 11:15:56.256 | luz-jsonstore | **Connection refused** - pod unavailable |
| 11:15:56.256 | luz-docs-batch | `DocumentException: jsonstore.service.unavailable` |
| 11:15:56.256 | luz-docs-view-controller-batch | Received HTTP 503 |
| 11:15:56.256 | luz-eletter | Marked as **FAILED_TO_STORE** |

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
2025-11-18 12:15:56,256 ERROR [DocumentStoreHandler] FAILED_TO_STORE
  documentId=12278
  errorCode=DOCUMENT_IMPORT_UNKNOWN_ERROR
```

#### luz-docs-batch Log

```
2025-11-18 12:15:56,256 ERROR [JsonStoreMongoService] updateOrRemoveMetadataFilterFields:
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
- [[Case Report - DocumentId 20512]]
- [[Case Report - DocumentId 223412]]
- [[Case Report - DocumentId 218735]]
- [[Case Report - DocumentId 222042]]
- [[Case Report - DocumentId 222561]]

%% ai-graph-end %%