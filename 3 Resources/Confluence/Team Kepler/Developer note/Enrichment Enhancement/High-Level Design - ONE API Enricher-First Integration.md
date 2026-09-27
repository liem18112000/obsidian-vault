---
ai_hash: 48c39dd9537e105c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48999890945'
confluence_path: Team Kepler > Developer note > Enrichment Enhancement
created: 2025-12-23
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- enricher
title: 'High-Level Design: ONE API Enricher-First Integration'
type: source
updated: 2025-12-29
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48999890945/High-Level+Design+ONE+API+Enricher-First+Integration
---

# High-Level Design: ONE API Enricher-First Integration

*Confluence source · Team Kepler › Developer note › Enrichment Enhancement · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48999890945/High-Level+Design+ONE+API+Enricher-First+Integration) · updated 2025-12-29*

> [!info]
>
>
>
>
>
>

------------------------------------------------------------------------

### 1. Overview

This document describes the high-level design for integrating ONE API with luz-docs to support the "enricher-first" flow for marketing letters. The solution ensures that document enrichment completes before letter delivery while maintaining high priority processing.

------------------------------------------------------------------------

### 2. Architecture Diagram

![[image-20251229-085718.png]]

### 3. Process Flows

#### 3.1 Document Creation Process

```
ONE API → POST /documents?isEnricherFirst=true → luz-docs
                              ↓
                    Save metadata to Mongo
                              ↓
                    Upload file to GCS
                              ↓
                    Return HTTP 201
                              ↓
                    Trigger enrichment event (async)
```

**Key Points:**

- New query parameter: `isEnricherFirst: bool` (optional, default: `false`)

- Document creation returns immediately with HTTP 201

- Enrichment is triggered asynchronously after response

#### 3.2 Enrich Trigger Process (Priority Handling)

```
                    isEnricherFirst?
                    /            \
                 True            False
                  ↓                ↓
         Set priority         Set priority
          to URGENT            by origin
              ↓                    ↓
        URGENT threads       DEFAULT threads
        (faster, more        (slower, fewer
         resources)           resources)
```

**Key Points:**

- `isEnricherFirst=true` guarantees URGENT priority

- URGENT thread pool processes documents faster with more resources

- DEFAULT thread pool has lower priority and fewer resources

#### 3.3 Enrichment Execution Flow

```
Trigger Enrich → Enrich Phase 1 + Phase 2
                          ↓
                   hasException?
                   /          \
                True          False
                 ↓              ↓
          isEnricherFirst?    isDocumentEnriched = True
          /          \                  ↓
       True         False        isEnricherFirst?
         ↓            ↓          /            \
    Actively       Normal     True          False
    Trigger        retry        ↓              ↓
    Retry          flow    Publish         (no event)
                           COMPLETED
```

#### 3.4 Retry and Failure Handling

```
Scheduled Retry Job runs periodically
              ↓
       Retry Exceed?
       /          \
    True         False
      ↓            ↓
isEnricherFirst?  Continue retry
    /      \      (increment counter)
 True     False
   ↓        ↓
Publish   (no event,
FAILED    silent failure)
```

------------------------------------------------------------------------

### 4. Published Events

#### 4.1 Event Structure

Events are published to Google Pub/Sub topic: `luz-docs-enrichment-status`

```
{
  "documentId": "string",
  "tenantId": "string",
  "status": "COMPLETED" | "FAILED",
  "isEnriched": true | false,
  "timestamp": "ISO-8601 timestamp",
  "retryCount": 0,
  "errorMessage": "string (only for FAILED status)"
}
```

#### 4.2 Event Scenarios

|                       |           |            |                          |
|-----------------------|-----------|------------|--------------------------|
| Scenario              | Status    | isEnriched | When Published           |
| Enrichment successful | COMPLETED | true       | After Phase 2 completes  |
| Max retries exceeded  | FAILED    | false      | When retry count \>= max |

------------------------------------------------------------------------

### 5. ONE API Integration Requirements

#### 5.1 API Call Changes

ONE API **MUST** add the new query parameter when creating marketing letter documents:

```
POST /luz_docs/api/tenants/{tenantId}/documents?isEnricherFirst=true
Content-Type: multipart/form-data
[existing request body unchanged]
```

**Important:**

- Parameter is optional (default: `false`)

- Only set to `true` for marketing letters that require enrichment before delivery

- Document creation response (HTTP 201) returns immediately - do NOT wait for enrichment

#### 5.2 Pub/Sub Subscription

ONE API **MUST** create a subscription to receive enrichment status events:

|                   |                                                   |
|-------------------|---------------------------------------------------|
| Configuration     | Value                                             |
| Topic             | `luz-docs-enrichment-status`                      |
| Subscription Name | `oneapi-enrichment-status-sub` (suggested)        |
| Message Ordering  | Enabled (ordering key: `{tenantId}-{documentId}`) |

#### 5.3 Event Handling Logic

ONE API **MUST** implement the following event handling:

```
On receiving enrichment status event:
    ├── If status == "COMPLETED":
    │       → Proceed with marketing letter delivery
    │       → Document is enriched and ready
    │
    └── If status == "FAILED":
            → Handle failure gracefully
            → Options:
                a) Deliver without enrichment data
                b) Notify operations team
                c) Retry document creation
                d) Cancel delivery
```

#### 5.4 Timeout Handling

ONE API **SHOULD** implement a timeout mechanism:

```
After creating document with isEnricherFirst=true:
    ├── Start timeout timer (suggested: 5-10 minutes)
    ├── Wait for Pub/Sub event
    │
    ├── If COMPLETED received before timeout:
    │       → Proceed with delivery
    │
    ├── If FAILED received before timeout:
    │       → Handle failure
    │
    └── If timeout reached with no event:
            → Query document status via API (fallback)
            → Or proceed with graceful degradation
```

------------------------------------------------------------------------

### 6. Sequence Diagram

```
ONE API              luz-docs              Pub/Sub            ONE API
   │                    │                     │              (subscriber)
   │                    │                     │                   │
   │ POST /documents    │                     │                   │
   │ ?isEnricherFirst   │                     │                   │
   │ =true              │                     │                   │
   │───────────────────>│                     │                   │
   │                    │                     │                   │
   │                    │ Save to Mongo       │                   │
   │                    │ Upload to GCS       │                   │
   │                    │                     │                   │
   │    HTTP 201        │                     │                   │
   │<───────────────────│                     │                   │
   │                    │                     │                   │
   │                    │ Async: Enrich       │                   │
   │                    │ (URGENT priority)   │                   │
   │                    │         .           │                   │
   │                    │         .           │                   │
   │                    │         .           │                   │
   │                    │                     │                   │
   │                    │ Publish COMPLETED   │                   │
   │                    │────────────────────>│                   │
   │                    │                     │                   │
   │                    │                     │ Deliver event     │
   │                    │                     │──────────────────>│
   │                    │                     │                   │
   │                    │                     │     Proceed with  │
   │                    │                     │     letter        │
   │                    │                     │     delivery      │
   │                    │                     │                   │
```

------------------------------------------------------------------------

### 7. Summary of ONE API Adaptations

#### 7.1 Must Have (Critical)

|  |  |  |
|----|----|----|
| \# | Adaptation | Description |
| 1 | Add query parameter | Include `isEnricherFirst=true` in POST /documents calls for marketing letters |
| 2 | Create Pub/Sub subscription | Subscribe to `luz-docs-enrichment-status` topic |
| 3 | Handle COMPLETED event | Proceed with delivery when enrichment succeeds |
| 4 | Handle FAILED event | Gracefully handle enrichment failures |

#### 7.2 Should Have (Recommended)

|     |                    |                                               |
|-----|--------------------|-----------------------------------------------|
| \#  | Adaptation         | Description                                   |
| 5   | Implement timeout  | Don't wait indefinitely for enrichment events |
| 6   | Add monitoring     | Track enrichment latency and failure rates    |
| 7   | Implement fallback | Query API if no event received within timeout |

#### 7.3 Nice to Have (Optional)

|     |                 |                                                |
|-----|-----------------|------------------------------------------------|
| \#  | Adaptation      | Description                                    |
| 8   | Retry mechanism | Retry document creation on persistent failures |
| 9   | Circuit breaker | Protect against luz-docs service issues        |

------------------------------------------------------------------------

### 8. Error Handling Matrix

|  |  |  |
|----|----|----|
| Scenario | luz-docs Behavior | ONE API Expected Action |
| Enrichment succeeds | Publish COMPLETED | Proceed with delivery |
| Enrichment fails (retries exhausted) | Publish FAILED | Handle gracefully |
| Document creation fails | Return HTTP 4xx/5xx | Retry or report error |
| Pub/Sub publish fails | Log error, no event | Use timeout + fallback |
| No event received | N/A | Query API after timeout |

%% ai-graph-start %%

**Related notes:**
- [[Express an ordering requirement as queue priority, not as a synchronous wait]]
- [[Adapt to support ONE API - Enricher first delivery]]
- [[Duplicate of Adapt to support ONE API - Enricher first delivery]]
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]
- [[Invoice Run, ePost backend storage]]

%% ai-graph-end %%