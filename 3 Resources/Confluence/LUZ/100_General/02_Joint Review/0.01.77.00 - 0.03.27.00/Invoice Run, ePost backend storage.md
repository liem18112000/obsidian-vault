---
ai_hash: 2df8c04139c3bf76
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49032822826'
confluence_path: LUZ Home > 100_General > 02_Joint Review > 0.01.77.00 - 0.03.27.00
  > Joint review 0.03.12.00 (30.12.2025 - 12.01.2026 )
created: 2026-01-12
entities: []
source: Confluence · LUZ - LUZ
status: reference
tags:
- confluence
- invoice-run
- epost
title: Invoice Run, ePost backend storage
type: source
updated: 2026-01-12
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49032822826/Invoice+Run+ePost+backend+storage
---

# Invoice Run, ePost backend storage

*Confluence source · LUZ Home › 100_General › 02_Joint Review › 0.01.77.00 - 0.03.27.00 › Joint review 0.03.12.00 (30.12.2025 - 12.01.2026 ) · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49032822826/Invoice+Run+ePost+backend+storage) · updated 2026-01-12*

------------------------------------------------------------------------

## Invoice Run V2: Distributed Rate Limiter

### **The Problem:**

- When Luz-store in process of running invoice, there were several issue due to the the dependent services (luz-finance, …) cannot handle the traffic

- So, Luz-store and dependent services cannot be scale to multiple pods

### **The Solution:**

- We put a Distributed Rate Limiter (using strategy of Token Bucket with Redis) to make sure the dependent services not getting overloaded while running invoice run.

### **The Result:**

- Luz-store can be scaled up.

- Invoice run are stabilized

### The technical points:

![[image-20260112-050227.png]]

1.  Message Queue - Invoice messages arrive from PubSub

2.  Receive & Parse - System receives and parses the invoice message

3.  Identify Type - Determines if message requires rate limiting:

- Calculate Invoice

- Charge & Create PDF

- Other (no rate limit)

4.  Rate Limit Check - If required, checks Redis token bucket:

- Capacity: 60 requests

- Refill: 60 per minute

- Shared across all pod instances

5.  Decision:

- Permit Granted → Process invoice → Complete

- Permit Denied → Requeue message for retry later

------------------------------------------------------------------------

## Document Enrichment: Urgent Priority for Enrichment

![[image-20260112-072744.png]]

### The Problem

- Currently, the ONE API has a special delivery flow for marketing letters that requires the enrichment process to be completed **before** the letter is delivered to the recipient.

- However, luz-docs is not currently designed for this behavior. In luz-docs, enrichment is not a mandatory finish step for document creation.

### The Solution

- To support this marketing letter use case, ONE API and luz-docs need to be aligned so that delivery is not failed due to enrichment process.

- Introduce the enrichment priority to control the order of the enrichment 

### The technical Points

![[image-20260112-073308.png]]

- **Document Creation Process:** The flow begins when ONE API sends a POST request to create a document. A new optional parameter called enrichPriority (an Enum type) has been added to the existing parameters, with DEFAULT as the default value. The system saves the document metadata to MongoDB, uploads the file to Google Cloud Storage, and returns HTTP 201 to the client.

- **Enrich Trigger Process:** After the document is created, an asynchronous enrichment event is triggered. The system checks if the document is marked as enricher-first. If true, the priority is set to URGENT and the document is processed using a dedicated URGENT thread pool with more threads for faster processing. If false, the priority is determined by the document origin and processed using the DEFAULT thread pool with fewer threads.

- **Enrich Process:** The enrichment runs in two phases. Phase 02 executes the Document Discover Enricher first. Phase 01 then runs multiple enrichers including Thumbnail Enricher, Content Type Enricher, Timestamp Enricher, and Email Enricher. Both phases include exception handling with a retry mechanism. When an exception occurs, the system checks if the retry limit has been exceeded. If not exceeded, the enricher retries immediately. If exceeded, the system adds a new row to the Enrichment Status table to record the failure.

- **Status Polling:** After enrichment completes or fails, the status is updated in the Enricher Status database. ONE API can then poll this status to track whether the document has been successfully enriched.

------------------------------------------------------------------------

## Vault Proxy

### The Problem

- **Critical**: luz-cache is shared by all LUZ services with fixed Memorystore capacity, and **luz-vault heavily relies on** it to stay healthy → under heavy load from multiple services, the **Memorystore ran out of capacity and became unavailable** on the last Nov 27, causing luz-vault to become stressed and go down.

- luz-vault clients contain duplicated Authentication and Caching code.

### The Solution

Vault Proxy

- Ensures **consistent API responses and latency** between Before and After

- **Eliminate dependency** on and **reduce HTTP traffic** to **luz-cache** and the shared memory region in **Memorystore.**

- luz-vault-proxy is a GKE deployment with image **startup within ~1s** and **supports Horizontal Pod Autoscaling** → **more stable and reliable** during sudden request spikes.

- With **Auto Login & Caching** features allows **simplification of code across modules**.

### The technical points

![[6-20260112-041000.png]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]
- [[Luz eLetter dispatch and enrichment re-run both funnel through luz-jsonstore into Mongo]]
- [[High-Level Design - ONE API Enricher-First Integration]]
- [[Invoice Run & luz-docs Archive Improvements]]
- [[New architecture for documentStatistic]]

%% ai-graph-end %%