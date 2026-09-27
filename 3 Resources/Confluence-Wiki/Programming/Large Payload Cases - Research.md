---
ai_hash: 9a1e30f7b97587db
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 23
depth: 3
entities: []
relevance: 0.841
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48728768517/Large+Payload+Cases+-+Research
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: Large Payload Cases - Research
topic: programming
type: source
updated: 2025-10-10
---

# Large Payload Cases - Research

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-10-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48728768517/Large+Payload+Cases+-+Research)
> Relevance 0.841 · topic `programming`

# Overview


![[48728768517-image-20251008-064845.png]]



# **Use-cases**


![[48728768517-luz-batch-case-7-8.png]]



<div id="expander-1258466169" class="expand-container conf-macro output-block" hasbody="true" macro-id="09ebf357-f32c-4e43-8e91-922feeec2820" macro-name="expand">

<div id="expander-control-1258466169" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Other diagram</span>

</div>

<div id="expander-content-1258466169" class="expand-content expand-hidden">


![[48728768517-luz-batch-case-7-8-usercases-tree.png]]



</div>

</div>

# **Details**

## Case 7


![[48728768517-case7.png]]



# Case 7.1


![[48728768517-case 7.1.png]]



## Case 7.2


![[48728768517-case 7.2.png]]



# **Case 8.0**


![[48728768517-case 8.png]]



# **Case 8.1**


![[48728768517-Case 8.1.png]]



# **Case 8.2**


![[48728768517-Case 8.2.png]]



# **Detailed Options Breakdown**

#### **Case 7.0: Immediate Results + Service Callback (3 options)**

<div>

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| **Option** | **Storage** | **Return to Client** | **Status Tracking** | **Retry** | **Multi-Instance** | **Notes** |
| 1️⃣ Library+Memory | Memory | requestId (fast) | Library | No | Works | not make sense |
| 2️⃣ Memory only | Memory | requestId (fast) | No | No | Works | not make sense |
| 3️⃣ Synchronous | None | 200 results (waits) | No | No | Works | ✅ **Go with option 3, no need to use batching library in this case.** |

</div>

#### **Case 7.1: TrackingId + Polling + Service Callback (3 options)**

<div>

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Option</strong></p></th>
<th><p><strong>Storage</strong></p></th>
<th><p><strong>Return to Client</strong></p></th>
<th><p><strong>trackingId Storage</strong></p></th>
<th><p><strong>Retry Polling</strong></p></th>
<th><p><strong>Crash Recovery</strong></p></th>
<th><p><strong>Multi-Instance</strong></p></th>
<th><p><strong>Notes</strong></p></th>
</tr>
&#10;<tr>
<td><p>1️⃣ Library+Memory</p></td>
<td><p>Memory</p></td>
<td><p>requestId (fast)</p></td>
<td><p>Library (persisted)</p></td>
<td><p>Even after restart</p></td>
<td><p>No (payload lost)</p></td>
<td><p>Works</p></td>
<td><p>No reviewed</p></td>
</tr>
<tr>
<td><p>2️⃣ Memory only</p></td>
<td><p>Memory</p></td>
<td><p>requestId (fast)</p></td>
<td><p>Memory</p></td>
<td><p>While running</p></td>
<td><p>No</p></td>
<td><p>Works</p></td>
<td><p>No reviewed</p></td>
</tr>
<tr>
<td><p>3️⃣ Call 3rd-party first</p></td>
<td><p>Memory</p></td>
<td><p>trackingId (waits)</p></td>
<td><p>Library (persisted)</p></td>
<td><p>Even after restart</p></td>
<td><p>No (payload lost)</p></td>
<td><p>Works</p></td>
<td><p>✅ <strong>Go with option 3, store everything in 3 collections.</strong></p>
<p>And there is no request payload stored so the services need to ensure they do not need that for the mapping logic or they should store it into the metadata.</p></td>
</tr>
</tbody>
</table>

</div>

#### **Case 7.2: TrackingId + Webhook + Service Callback (4 options)**

<div>

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 12%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Option</strong></p></th>
<th><p><strong>Storage</strong></p></th>
<th><p><strong>Return to Client</strong></p></th>
<th><p><strong>trackingId Storage</strong></p></th>
<th><p><strong>Multi-Instance</strong></p></th>
<th><p><strong>Crash Recovery</strong></p></th>
<th><p><strong>Production Ready</strong></p></th>
<th><p><strong>Notes</strong></p></th>
</tr>
&#10;<tr>
<td><p>1️⃣ Library+Memory</p></td>
<td><p>Memory</p></td>
<td><p>requestId (fast)</p></td>
<td><p>Library</p></td>
<td><p><strong>BROKEN</strong></p></td>
<td><p>No</p></td>
<td><p>No</p></td>
<td><p>No reviewed</p></td>
</tr>
<tr>
<td><p>2️⃣ Memory only</p></td>
<td><p>Memory</p></td>
<td><p>requestId (fast)</p></td>
<td><p>Memory</p></td>
<td><p><strong>BROKEN</strong></p></td>
<td><p>No</p></td>
<td><p>No</p></td>
<td><p>No reviewed</p></td>
</tr>
<tr>
<td><p>3️⃣ Service Storage</p></td>
<td><p>DB or FileStore/GCS</p></td>
<td><p>requestId (fast)</p></td>
<td><p>Storage</p></td>
<td><p><strong>WORKS</strong></p></td>
<td><p>Yes</p></td>
<td><p><strong>YES</strong></p></td>
<td><p>No reviewed</p></td>
</tr>
<tr>
<td><p>4️⃣ Call 3rd-party first</p></td>
<td><p>Memory</p></td>
<td><p>trackingId (waits)</p></td>
<td><p>Library</p></td>
<td><p><strong>BROKEN</strong></p></td>
<td><p>No</p></td>
<td><p>No</p></td>
<td><p>✅<strong>Option 4, ensure service only return response to third-party only when service sent data to client.</strong></p>
<p>And there is no request payload stored so the services need to ensure they do not need that for the mapping logic, or they should store it into the metadata.</p></td>
</tr>
</tbody>
</table>

</div>

#### **Case 8.0: Immediate Results + Client Polling (5 options)**

<div>

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| **Option** | **Storage** | **Return to Client** | **Results Storage** | **Multi-Instance** | **Use When** | **Notes** |
| 1️⃣ Library+Memory | Memory | requestId (fast) | Memory | **BROKEN** | Never | No reviewed |
| 2️⃣ Memory only | Memory | requestId (fast) | Memory | **BROKEN** | Single instance only | No reviewed |
| 3️⃣ Service Storage | DB or FileStore/GCS | requestId (fast) | Storage | **WORKS** | **Production (async)** | No reviewed |
| 4️⃣ Poll 3rd-party every time | None | requestId (fast) | N/A (always fetch) | Works | Performance/cost issues | not make sense |
| 5️⃣ Synchronous | None | 200 results (waits) | N/A | Works | **3rd-party is fast** | ✅ **Go with option 5, no need to use batching library in this case.** |

</div>

#### **Case 8.1: TrackingId + Polling + Client Polling (4 options)**

<div>

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| **Option** | **Storage** | **Return to Client** | **trackingId Storage** | **Multi-Instance** | **Use When** | **Notes** |
| 1️⃣ Library+Memory | Memory | requestId (fast) | Library | **BROKEN** | Never | No reviewed |
| 2️⃣ Memory only | Memory | requestId (fast) | Memory | **BROKEN** | Single instance only | No reviewed |
| 3️⃣ Service Storage | DB or FileStore/GCS | requestId (fast) | Storage | **WORKS** | **Production** | No reviewed |
| 4️⃣ Call 3rd-party first | None | trackingId (waits) | N/A | Works | Performance/cost issues | not make sense |
| 5️⃣Call 3rd-party first (but with library) |  |  |  |  |  | ✅ **Go with option 5, need library, add or update the code/database to distinguish cases: small and large payload.** |

</div>

#### **Case 8.2: TrackingId + Webhook + Client Polling (3 options)**

**Notes:** this case does not make sense; service should go with case 7.2 for this usecase.

<div>

|  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|
| **Option** | **Storage** | **Return to Client** | **Webhook Issue** | **Client Poll Issue** | **Production Ready** | **Notes** |
| 1️⃣ Library+Memory | Memory | requestId (fast) | Wrong pod | Wrong pod | **DOUBLE BROKEN** | not make sense |
| 2️⃣ Memory only | Memory | requestId (fast) | Wrong pod | Wrong pod | **DOUBLE BROKEN** | not make sense |
| 3️⃣ Service Storage | DB or FileStore/GCS | requestId (fast) | Works | Works | **YES** | not make sense |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Batch Processor Library - NodeJS]]
- [[Score async API designs on crash recovery and multi-instance, not latency]]
- [[Batching Design]]
- [[Luz Batch TypeScript - Sequence Diagram]]
- [[Performance pain points]]

%% ai-graph-end %%