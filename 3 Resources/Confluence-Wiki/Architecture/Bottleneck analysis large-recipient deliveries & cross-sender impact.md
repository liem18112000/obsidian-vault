---
ai_hash: 9058d72f95146d4e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.72
entities: []
relevance: 0.777
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/49648402464/Bottleneck+analysis+large-recipient+deliveries+cross-sender+impact
space: HACKA
status: reference
tags:
- confluence
- architecture
- space/hacka
title: 'Bottleneck analysis: large-recipient deliveries & cross-sender impact'
topic: architecture
type: source
updated: 2026-08-07
---

# Bottleneck analysis: large-recipient deliveries & cross-sender impact

> [!info] Imported from Confluence
> Space **HACKA** · updated 2026-08-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/49648402464/Bottleneck+analysis+large-recipient+deliveries+cross-sender+impact)
> Relevance 0.777 · topic `architecture`

<div hasbody="true" macro-id="a64e012c-1e3a-4dde-bf1e-17413fcaa94a" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Follow-up analysis from the async-delivery load testing on `klara-performance` (2k/3k-recipient chunk tests, 2026-08-06/07). Answers three questions: what causes the bottleneck as recipient count grows, how it can affect other senders, and whether "unlimited" recipients per delivery is achievable. Related: [Support Kanton Bern sending 280k documents](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/49577754638/Support+Kanton+Bern+sending+280k+documents), Jira `LUZ-157031`.

</div>

</div>

## 1. What creates the bottleneck as recipient count grows?

Three distinct constraints stack on top of each other. None of them are about "too many recipients" in the abstract — each is a specific resource that a bigger request, or a bigger burst of smaller requests, consumes faster than it can be replenished.

### 1.1 Upload/creation time

The public create-delivery call does one synchronous multipart upload of every document to `luz-storage-batch`, inside `luz-eletter`. This time scales with document count:

<div>

|                                |                                          |
|--------------------------------|------------------------------------------|
| Chunk size                     | Storage-batch store time (measured)      |
| 2,000 docs (~48KB each)        | 89–272s (avg 168s)                       |
| 3,000 docs                     | ~150–210s (avg ~183s)                    |
| 8,000 docs (pre-GKE migration) | ~302s — caused the original NPE incident |

</div>

### 1.2 Timeout chain — still has an unresolved gap

Three timeouts are stacked in series; the shortest one wins:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5abc9bae-ecb5-4a06-848e-3d5e6be2c653" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
client -> Kong proxy (3600s) -> GKE Gateway backend for luz-eletter (GCPBackendPolicy) -> luz-eletter -> luz-storage-batch (MP-REST readTimeout)
```

</div>

</div>

- App-level `LUZ_STORAGE_BATCH_SERVICE_KEY_MP_REST_READTIMEOUT`: raised from 300s default to 900s.

- Gateway-level `GCPBackendPolicy` (`kubernetes-overlays/env-performance/infra-gateway/k8s.yaml`, resource `infra-gateway-policy` targeting the `luz-eletter` Service): still `timeoutSec: 300` — **never updated to match the 900s app-level value**. A request the app would happily wait out can still be hard-killed at the Gateway. Our 2k test already saw a 317.8s response succeed once (before this Gateway policy existed); the same response today would be killed at 300s.

### 1.3 Two more serialized per-item pipelines, after creation succeeds

A "created" delivery isn't a "delivered" one. Two more stages process it asynchronously, each doing one call per item on its own Pub/Sub queue:

- **Per document** → `luz-docs` import. `StoringDocumentProcessing` → `DocumentSenderFolderService.processDocumentStoring` → `SenderDocumentStorageService.storeDocumentInSentFolder` makes one REST import call per document, moving it from the temp storage-batch path into the sender's permanent folder. Gated by a 15-minute `STORING_IN_SENDER` timeout.

- **Per recipient** → digital dispatch. `DigitalChannelDispatcher` does a synchronous DB insert (recipient tracking) plus a matching + send call, per recipient, per document. No batching in either path.

## 2. How could this affect other senders negatively?

Routing is threshold-based: `DispatcherSelector.isUsingSmallDispatcher()` sends any delivery with **more than 100 total recipients** (`MAX_NO_RECIPIENT_FOR_SMALL_DISPATCHER=100` in performance/prod/dev/test; 10 in swissdec) to a separate `luz-eletter-large-dispatcher` deployment, with its own dedicated Pub/Sub subscriptions for the core pipeline (document-sending, recipient-sending, delivery-processing, delivery-status — same topics, distinct `-large-` subscriptions). Both dispatcher deployments have identical HPA configuration (min 1 / max 3 replicas, same backlog/CPU/memory targets).

### What is genuinely isolated

Typical small-delivery traffic (prod baseline: median 1 recipient, p95 = 7) never touches the large dispatcher's queues or pods. One sender's single delivery over 100 recipients cannot directly queue-block a small delivery from another tenant.

### What is not isolated

- **Same database, same infrastructure.** Both dispatcher deployments write to the same Postgres instance and tables — shared connection pool and I/O capacity mean load or contention on one path can still affect the other, independent of any specific locking mechanism.

- **Create-time path is shared regardless of size.** The Gateway and `luz-storage-batch` upload aren't threshold-routed; every sender's create call competes for the same capacity before size-based routing ever applies.

- **Some queues aren't split by size at all.** In this environment's config, `message-business-status` and `delivery-failure-notification` subscriptions are only wired up for the large dispatcher (small dispatcher's config has them blank) — not confirmed as intentional, flagged for follow-up.

- **Other senders in the same bucket share fate.** Any delivery over 100 recipients — including a legitimate one-off business delivery, not just a bulk/test send — lands in the same large-dispatcher pool as every other such sender.

## 3. Is there a way to make it work with "unlimited" recipients?

No — "unlimited" isn't achievable. There is a real physical ceiling downstream (postal/print-house logistics, SMS/email provider rate limits, digital match+send capacity) that no internal architecture change removes. But today's *practical* ceiling is set by fixable, artificial constraints, not physical ones.

### Recommended changes, in order of leverage

1.  **Chunked/multi-part upload API against one delivery ID.** Replace the single unbounded multipart call with a bounded-size, multi-call pattern (client sends documents in fixed-size batches against one delivery, finalizes when done) — similar to S3 multipart upload. Each call stays fully synchronous and durable exactly as today (a 201-equivalent response still only means "these documents are safely stored"), so no new polling burden and no change to the delivery guarantee. This removes the direct coupling between total document count and a single call's duration, which is what collides with the Gateway timeout today.

2.  **Batch the per-item downstream calls.** Both the luz-docs import (one call per document) and the recipient-tracking DB insert (one row per recipient) are currently serial, per-item operations. Batching these removes the O(N) per-item cost that scales with recipient count.

3.  **Scale drain capacity to match arrival rate.** Both dispatchers cap at 3 replicas today. Raise `maxReplicas`, and scale the DB connection pool and luz-docs backend capacity in step — turning a fixed ceiling into something that scales horizontally with load.

4.  **Fix the two known live bugs**, since both currently masquerade as capacity problems:

    - Gateway `GCPBackendPolicy` timeout (300s) vs app-level readTimeout (900s) mismatch — update the policy to match.

    - `DocumentStorageBatchUtils.storeDocuments` logs `result.toString()` before its null check, causing an opaque NPE (instead of a clean retryable error) whenever storage-batch has any hiccup, and risking orphaned documents.

Doing all four gets the system to scale to whatever volume any real customer plausibly needs. What can't be engineered away: the actual send step — digital match+send capacity, SMS/email provider rate limits, physical mail/print-house throughput. Raising those means bigger provider contracts or capacity, not code changes.

## Supporting data — our 3k-chunk test (2026-08-06/07)

*Note:* the setup is different from the 2k-chunk test that no peak hour simulation in this 3k-chunk test.

<div>

|  |  |
|----|----|
| Metric | Value |
| Chunks created | 39 / 39 (100%, 0 creation failures) |
| Total documents submitted | 117,000 (3,000/chunk) |
| Submission window | 2 hours (sequential, one sender) |
| Peak backlog (large recipient-sending queue) | 35,176 messages |
| Time to fully drain backlog | ~6 hours after submission ended |
| Final outcome | 117,000 / 117,000 delivered (100%), 0 failed documents at any point |

</div>

Full per-delivery timing and status data available in the automation repo (`big-deliveries/run_3k_2h_test.py`, `check_ack_rate.py`, `compare_queues_history.py`) for reproduction.

%% ai-graph-start %%

**Related notes:**
- [[Timeouts stack in series and the shortest wins; audit the whole chain]]
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]
- [[If upstream holds memory until you ack, your write latency is their OOM risk]]
- [[Invoice Run, ePost backend storage]]
- [[Performance Analysis and Proposed Solutions]]

%% ai-graph-end %%