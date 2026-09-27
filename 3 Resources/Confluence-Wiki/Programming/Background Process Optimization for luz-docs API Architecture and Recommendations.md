---
title: "Background Process Optimization for luz-docs API: Architecture and Recommendations"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49306796036/Background+Process+Optimization+for+luz-docs+API+Architecture+and+Recommendations
space: "LUZ"
topic: programming
relevance: 0.921
depth: 3
updated: 2026-04-08
attachments: 0
tags:
  - confluence
  - programming
  - space/luz
---

# Background Process Optimization for luz-docs API: Architecture and Recommendations

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-04-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49306796036/Background+Process+Optimization+for+luz-docs+API+Architecture+and+Recommendations)
> Relevance 0.921 · topic `programming`

## 1. Overview of Current Architecture

Currently, the `luz-docs` API relies on a JAX-RS `ContainerRequestFilter` (`TriggerAsyncRequestFilter.java`) to intercept incoming HTTP requests and trigger multiple background processes.

When a request contains a `tenantId`, the filter sequentially fires multiple CDI asynchronous events (e.g., `fireAsync`). These events are picked up by respective job classes (e.g., `EnrichmentJob`), which use in-memory caches (like `DeleteFolderAndDocumentTimeCache`) to throttle their actual execution (e.g., ensuring a job only runs once a day or once every 5 minutes per tenant).

When allowed by the cache, the jobs execute within the Wildfly container using specific `ManagedExecutorService` thread pools configured in `thread-pool.xml`.

------------------------------------------------------------------------

## 2. Detailed Process Breakdown

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th><p>Job Name</p></th>
<th><p>Trigger</p></th>
<th><p>Execution Mechanism</p></th>
<th><p>Performance Impact</p></th>
<th><p>Recommendation</p></th>
</tr>
&#10;<tr>
<td><p><strong>Delete Folder &amp; Document Job</strong><br />
<code>DeleteFolderAndDocumentJob.java</code></p></td>
<td><p>JAX-RS Filter -&gt; <code>DeleteFolderAndDocumentJobEvent</code></p></td>
<td><p>Handled by the <code>deleteFolderAndDocumentJobRequestExecutor</code> thread pool. Checks cache (runs once/day/tenant). Queries DB for marked folders/documents and deletes metadata and GCS files.</p></td>
<td><p><strong>High</strong><br />
GCS deletion involves network I/O. Database batch deletions lock tables and drain DB connection pool during peak traffic.</p></td>
<td><p><strong>MOVE</strong><br />
Move out of the API lifecycle to a scheduled K8s CronJob.</p></td>
</tr>
<tr>
<td><p><strong>Re-run Enrichment Job</strong><br />
<code>EnrichmentJob.java</code></p></td>
<td><p>JAX-RS Filter -&gt; <code>RerunEnrichmentJobEvent</code></p></td>
<td><p>Handled by <code>enrichmentEveryRequest</code> pool. Throttled to 5 mins. Queries for docs needing enrichment, loops over them, blocking threads with a 20-min timeout <code>Future.get()</code>.</p></td>
<td><p><strong>Severe (Critical)</strong><br />
Blocks threads waiting on EJB background threads, causing thread exhaustion, CPU/Memory contention, and HTTP queuing.</p></td>
<td><p><strong>MOVE</strong><br />
Decouple immediately. Move to an event-driven queue (GCP Pub/Sub) or Cloud Tasks.</p></td>
</tr>
<tr>
<td><p><strong>Re-Enrich Docs Failed ≥ 3 Times</strong><br />
<code>ReEnrichForDocumentRetryFailedJob.java</code></p></td>
<td><p>JAX-RS Filter -&gt; <code>ReEnrichForDocumentRetryFailedJobEvent</code></p></td>
<td><p>Handled by <code>reEnrichForDocumentRetryFailedJobRequest</code>. Checks cache. Queries DB for failed enrichments and re-queues them.</p></td>
<td><p><strong>Medium</strong><br />
Adds unnecessary DB query load scanning for failed documents to standard API requests.</p></td>
<td><p><strong>MOVE</strong><br />
Process as a Dead Letter Queue (DLQ) task via scheduled background worker or Pub/Sub natively.</p></td>
</tr>
<tr>
<td><p><strong>Check TSA Documents Job</strong><br />
<code>CheckTSADocumentsJob.java</code></p></td>
<td><p>JAX-RS Filter -&gt; <code>CheckTSADocumentsJobEvent</code></p></td>
<td><p>Handled by <code>checkTSADocumentsJobRequest</code>. Throttled to once/day/tenant. Counts docs missing TSA, verifies random batch of 100 via cryptographic timestamps.</p></td>
<td><p><strong>Medium/High</strong><br />
Cryptographic verification is CPU-intensive. Initial count requires heavy DB aggregation.</p></td>
<td><p><strong>MOVE</strong><br />
Daily compliance/audit task. Move to a K8s CronJob.</p></td>
</tr>
<tr>
<td><p><strong>Delete Large Files Over Expire Date</strong><br />
<code>DeleteLargeFileDocumentJob.java</code></p></td>
<td><p>JAX-RS Filter -&gt; <code>DeleteLargeFileDocumentJobEvent</code></p></td>
<td><p>Handled by <code>deleteLargeFileDocumentJobRequest</code>. Throttled to once/day/tenant. Paginated while-loops to query expired files and soft-delete.</p></td>
<td><p><strong>Medium</strong><br />
Pagination loops inside a Wildfly thread hold database connections open for extended periods.</p></td>
<td><p><strong>MOVE</strong><br />
Batch cleanup operation suited for a scheduler. Move to a K8s CronJob.</p></td>
</tr>
<tr>
<td><p><strong>Register Tenant for Missing TSR Check</strong><br />
<code>RegisterTenantForCheckingTSRJob.java</code></p></td>
<td><p>JAX-RS Filter -&gt; <code>RegisterTenantForCheckingTSRJobEvent</code></p></td>
<td><p>Pushes a token/tenant mapping to <code>RegisterCheckingTSRCacheController</code>.</p></td>
<td><p><strong>Low</strong><br />
Fast cache update, but firing an async event on <em>every</em> API request adds JAX-RS filter overhead.</p></td>
<td><p><strong>MOVE/REFACTOR</strong><br />
Trigger only once on "Tenant Login" or manage via a separate sync worker.</p></td>
</tr>
</tbody>
</table>

</div>

------------------------------------------------------------------------

## 3. Proposed Architectural Design

To ensure the `luz-docs` API remains highly available, performant, and horizontally scalable, the background processing logic must be entirely decoupled from the synchronous HTTP request path.

### Step 1: Remove JAX-RS Filter Triggers

- **Action:** Completely remove `TriggerAsyncRequestFilter.java`.

- **Replace with:** <a href="https://axonivy.atlassian.net/wiki/x/AQBWMAs" data-card-appearance="inline" data-local-id="bd8d49073a3b" rel="nofollow">https://axonivy.atlassian.net/wiki/x/AQBWMAs</a>

- **Benefit:** Instantly improves P99 API latency, reduces Wildfly thread pool creation overhead, and prevents sudden database connection spikes caused by normal user traffic.

### Step 2: Implement Kubernetes CronJobs for Scheduled Tasks

Time-based database cleanups and daily audits should be executed by GKE native schedulers.

- **Targeted Jobs:**

  - `DeleteFolderAndDocumentJob` (Nightly)

  - `DeleteLargeFileDocumentJob` (Nightly)

  - `CheckTSADocumentsJob` (Nightly)

  - `ReEnrichForDocumentRetryFailedJob` (Hourly/Daily)

- **Implementation:** Package the execution logic into a command-line runner (or utilize a lightweight Spring Boot / Quarkus command-mode application) using the same codebase/image. Deploy as `CronJob` resources in GKE. They will spin up, do the batch work, and terminate, keeping the API container completely free of background noise.

### Step 3: Implement Event-Driven Workers using GCP Pub/Sub

For tasks that need immediate, asynchronous processing without waiting for a daily cron schedule.

- **Targeted Jobs:** `EnrichmentJob` (Standard document processing).

- **Implementation:**

  1.  When a document is uploaded/modified via the API, the API publishes a message to a `luz-docs-enrichment-topic` on GCP Pub/Sub. The API immediately returns `202 Accepted` to the user.

  2.  Create a dedicated Kubernetes Deployment (`luz-docs-worker`). This deployment subscribes to the Pub/Sub topic and processes the enrichment.

  3.  **Scalability:** The worker pods can scale horizontally using KEDA based on the Pub/Sub queue depth, completely independent of the API traffic.

### Step 4: Utilize Google Cloud Tasks (Optional/Alternative for Enrichment)

<a href="https://docs.cloud.google.com/tasks/docs/dual-overview" class="external-link" rel="nofollow">Understand Cloud Tasks  |  Google Cloud Documentation</a>

<a href="https://docs.cloud.google.com/tasks/docs/comp-pub-sub" class="external-link" rel="nofollow">Choose Cloud Tasks or Pub/Sub  |  Google Cloud Documentation</a>

If the AI Analysis service has strict rate limits, **Google Cloud Tasks** is highly recommended over standard Pub/Sub. Cloud Tasks natively supports:

- Configurable dispatch rates (e.g., max 50 requests per second).

- Exponential backoff and retry rules.

- Deduplication of requests.

## Conclusion

The current architecture conflates serving user API requests with heavy background data management. By migrating to **K8s CronJobs** for maintenance and **GCP Pub/Sub Worker Pods** for event-driven logic, the `luz-docs` API will achieve higher throughput, lower latency, and better resilience against memory and thread pool exhaustion.
