---
ai_hash: 7e8241515bcc2183
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48954736737'
confluence_path: 'Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE
  on Production > Failed to store cases'
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Case Report: DocumentId 217599'
type: source
updated: 2025-12-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954736737/Case+Report+DocumentId+217599
---

# Case Report: DocumentId 217599

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production › Failed to store cases · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954736737/Case+Report+DocumentId+217599) · updated 2025-12-10*

### Summary

|                        |                                    |
|------------------------|------------------------------------|
| Field                  | Value                              |
| **Case Number**        | 1 of 24                            |
| **DocumentId**         | 217599                             |
| **DeliveryId**         | 217599                             |
| **Failure Time (UTC)** | 2025-11-10T11:04:31.042Z           |
| **Root Cause**         | luz-jsonstore Connection refused   |
| **Error Code**         | DOCUMENT_IMPORT_UNKNOWN_ERROR      |
| **Cluster**            | Nov10-A (first failure)            |
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
| **FAILED_TO_STORE** | 11:04:31.042 | DocumentId 217599                |
| Pod Starting        | 11:11:12.858 | `luz-jsonstore-77cfd496bc-q7m4r` |

**Gap Analysis:** Failure occurred **~7 minutes before** pod restart completed. This indicates the old pod had already terminated but the new pod had not yet started.

------------------------------------------------------------------------

### Detailed Timeline

|  |  |  |
|----|----|----|
| Time (UTC) | Service | Event |
| 11:04:31.042 | luz-eletter | Document storage initiated |
| 11:04:31.042 | luz-docs-batch | Attempted call to luz-jsonstore |
| 11:04:31.042 | luz-jsonstore | **Connection refused** - pod unavailable |
| 11:04:31.042 | luz-docs-batch | `DocumentException: jsonstore.service.unavailable` |
| 11:04:31.042 | luz-docs-view-controller-batch | Received HTTP 503 |
| 11:04:31.042 | luz-eletter | Marked as **FAILED_TO_STORE** |

------------------------------------------------------------------------

### Cluster Information

This case is part of the **November 10 Major Cluster** - the largest outage in the 30-day period.

|                    |                                 |
|--------------------|---------------------------------|
| Cluster            | Nov10-A + Nov10-B               |
| **Total Failures** | 6 documents                     |
| **Time Range**     | 11:04:31 - 11:11:21 (7 minutes) |
| **Duration**       | ~7 minutes                      |

#### Related Cases in Cluster

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
<td><p>**1**</p></td>
<td><p>**217599**</p></td>
<td><p>11:04:31.042</p></td>
<td><ul>
<li><p>(first)</p></li>
</ul></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>749107</p></td>
<td><p>11:04:37.232</p></td>
<td><p>6 seconds</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>588</p></td>
<td><p>11:10:52.743</p></td>
<td><p>6 min 15 sec</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>3507</p></td>
<td><p>11:10:56.477</p></td>
<td><p>4 seconds</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>589</p></td>
<td><p>11:11:11.550</p></td>
<td><p>15 seconds</p></td>
</tr>
<tr>
<td><p>6</p></td>
<td><p>590</p></td>
<td><p>11:11:21.765</p></td>
<td><p>10 seconds</p></td>
</tr>
</tbody>
</table>

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
2025-11-10 12:04:31,042 ERROR [DocumentStoreHandler] FAILED_TO_STORE
  documentId=217599
  errorCode=DOCUMENT_IMPORT_UNKNOWN_ERROR
```

#### luz-docs-batch Log

```
2025-11-10 12:04:31,042 ERROR [JsonStoreMongoService] updateOrRemoveMetadataFilterFields:
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
- [[Case Report - DocumentId 749107]]
- [[Case Report - DocumentId 222561]]
- [[Case Report - DocumentId 222042]]
- [[Case Report - DocumentId 1741]]
- [[Case Report - DocumentId 218735]]

%% ai-graph-end %%