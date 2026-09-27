---
ai_hash: 8505b999f1850e6c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.818
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48752394379/Recipe+Luz+Batch+TypeScript
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: 'Recipe: Luz Batch TypeScript'
topic: programming
type: source
updated: 2026-01-30
---

# Recipe: Luz Batch TypeScript

> [!info] Imported from Confluence
> Space **FUT** · updated 2026-01-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48752394379/Recipe+Luz+Batch+TypeScript)
> Relevance 0.818 · topic `programming`

A TypeScript library for batching async requests from multiple clients with PostgreSQL storage. This package allows your service to efficiently group individual requests into batches, process them with a third-party service, and return results to the original callers.

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="3e97531f-0a33-4ac0-b364-60fe41766708" macro-name="toc">

</div>

------------------------------------------------------------------------

## Overview

When your service receives many individual requests (e.g., 10 HTTP requests, each containing 50 addresses), this library:

1.  **Stores** all incoming items in a PostgreSQL database

2.  **Groups** pending items into batches when a threshold is reached

3.  **Sends** batches to your third-party service (you implement this logic)

4.  **Retrieves** results from the third-party (either immediately or via polling)

5.  **Notifies** your service when original requests are complete

### Key Features

- **Multi-tenant support**: Different host projects share the same database, differentiated by `batchType`

- **Automatic batching**: Internal schedulers poll for pending items and create batches

- **Flexible result handling**: Supports both immediate results and async polling

- **Request tracking**: Clients receive a tracking ID to check their request status

- **Notification system**: Your service is notified when requests complete

------------------------------------------------------------------------

## Installation

Follow this page [Recipe: NPM private package](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48954736685/Recipe+NPM+private+package) to make sure you setup access to our private npm repository.

Then add the library to your `package.json`:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b1b532b7-b01e-4061-9ab8-73aec2eb7cc0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "dependencies": {
    "@luz/luz-batch-typescript": "^0.0.1", // This is just sample version. Please use the latest one.
  }
}
```

</div>

</div>

To find the latest version, run `npm view @luz/luz-batch-typescript`


![[48752394379-image-20251231-081739.png]]



Then install the dependency by running `npm install`

------------------------------------------------------------------------

## Step-by-Step Integration Guide

### Step 1: Create Your Processor Class

Extend the `AsyncProcessor` base class with your input type:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="63ad92cb-3ed4-40b6-bf09-0a4be68346a4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import { 
  AsyncProcessor, 
  AsyncProcessorConfig,
  BatchRequestData,
  BatchProcessingResult,
  BatchItem 
} from '@luz/luz-batch-typescript';

// Define your input type
interface AddressInput {
  street: string;
  city: string;
  zipCode: string;
}


export class AddressValidationProcessor extends AsyncProcessor<AddressInput> {

  constructor() {
    // Define your batching configuration
    const config: AsyncProcessorConfig = {
      // Required
      batchType: 'MY_SERVICE_TYPE',           // Unique identifier for your service
      storage: {
        type: 'postgresql',
        url: process.env.DATABASE_URL!,       // PostgreSQL connection string
        options: {
          connectionPoolSize: 10,             // Optional: default 10
          connectionTimeoutMillis: 30000,     // Optional: default 30000
          idleTimeoutMillis: 10000,           // Optional: default 10000
          }
      },
      
      // Batching behavior
      targetBatchSize: 100,                   // Items per batch (default: 100)
      batchCheckIntervalMs: 5000,             // Base scheduler interval (default: 5000)
      
      // Advanced scheduler intervals (optional)
      batchCreationIntervalMs: 5000,          // How often to check for items to batch
      batchProcessingIntervalMs: 5000,        // How often to process pending batches
  
      // Performance tuning (optional)
      maxConcurrentBatchCreations: 5,         // Parallel batch creations
      batchQueryPageSize: 1000,               // Items per DB query page
      queryRequestLimit: 1000,                // Max requests per query
  
      // Lease durations for distributed systems (optional)
      batchResultCheckLeaseDurationMs: 30000, // Lease for polling results
      notificationLeaseDurationMs: 30000,     // Lease for notifications
  
      // Logging
      logLevel: 'info'                        // 'error' | 'warn' | 'info' | 'debug'
        };
    
    super(config);
    this.start();
  }

  // ... implement required methods, see step 2
}
```

</div>

</div>

### Step 2: Implement the Required Methods

#### `processBatch()` - Required

This method is called when a batch is ready to be sent to your third-party service:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1a5d451c-ceaf-4bb5-908c-afc0a031b562" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
async processBatch(batch: BatchRequestData): Promise<BatchProcessingResult> {
  const batchId = batch.items[0].batchId!;
  
  this.logger?.info(`Processing batch ${batchId} with ${batch.items.length} items`);

  try {
    // Extract your data from batch items
    const addresses = batch.items.map(item => item.originalPayload as AddressInput);

    // Send to your third-party service
    const response = await this.externalAPI.submitBatch(addresses);

    // OPTION A: Third-party returns results immediately
    if (response.results) {
      const processedItems: BatchItem[] = batch.items.map((item, idx) => ({
        ...item,
        state: 'PROCESSED',
        result: response.results[idx]
      }));

      return {
        batchId,
        isResultReady: true,
        results: processedItems
      };
    }

    // OPTION B: Third-party returns a tracking ID for later retrieval
    return {
      batchId,
      isResultReady: false,
      metadata: {
        trackingId: response.trackingId,
        submittedAt: new Date().toISOString()
      }
    };

  } catch (error) {
    this.logger?.error(`Batch ${batchId} submission failed`, { error });
    
    // Return failure state - batch will be retried
    return {
      batchId,
      isResultReady: false,
      metadata: { error: error.message, failedAt: new Date().toISOString() }
    };
  }
}
```

</div>

</div>

#### `pollBatchResult()` - Optional

Implement this if your third-party service doesn't return results immediately:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="753f12f4-0037-432e-885c-25d7d048545a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
async pollBatchResult(batchId: string, metadata?: any): Promise<BatchProcessingResult | null> {
  const trackingId = metadata?.trackingId;
  
  if (!trackingId) {
    this.logger?.warn(`Batch ${batchId} missing tracking ID`);
    return null;
  }

  try {
    // Check if results are ready
    const response = await this.externalAPI.getResults(trackingId);

    if (!response || !response.isComplete) {
      // Not ready yet - return null to try again later
      return null;
    }

    // Get original batch items to map results
    const batchItems = await this.queryBatchItems({ batchId });

    // Map results to batch items
    const processedItems: BatchItem[] = batchItems.map((item, idx) => ({
      ...item,
      state: 'PROCESSED',
      result: response.results[idx]
    }));

    return {
      batchId,
      isResultReady: true,
      results: processedItems,
      metadata: {
        ...metadata,
        completedAt: new Date().toISOString()
      }
    };

  } catch (error) {
    this.logger?.error(`Error polling batch ${batchId}`, { error });
    return null; // Will retry on next poll cycle
  }
}
```

</div>

</div>

#### `onRequestCompleted()` - Optional

Called when all items in a request are processed:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f3413db-6e64-4af1-bf8d-17c2b6f9d60c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
async onRequestCompleted(requestId: string): Promise<void> {
  this.logger?.info(`Request ${requestId} completed, notifying client`);

  try {
    // Get request details including callback URL from metadata
    const request = await this.getRequestStatus(requestId);
    
    const callbackUrl = request.metadata?.callbackUrl;
    if (callbackUrl) {
      // Send webhook notification
      await fetch(callbackUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          requestId,
          state: 'COMPLETED',
          results: request.results
        })
      });
    }
  } catch (error) {
    this.logger?.error(`Failed to notify client for request ${requestId}`, { error });
    // Throwing an error here will keep the request in PROCESSED state for retry
    throw error;
  }
}
```

</div>

</div>

### Step 3: Handle Client Requests

When your service receives requests from clients:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c4d89245-81ef-4c36-b0f0-da43026a184d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Express.js example
app.post('/validate-addresses', async (req, res) => {
  try {
    const addresses = req.body; // Array of addresses from client

    // Submit to batching library
    const { clientRequestId, totalItems } = await processor.processAsyncRequest(
      addresses,           // Data to process
      false,              // directProcess = false (use batching)
      req.user?.tenantId, // Optional tenant ID
      {                   // Optional metadata
        callbackUrl: req.body.callbackUrl,
        clientInfo: req.headers['user-agent']
      }
    );

    // Return tracking ID to client
    res.status(202).json({
      requestId: clientRequestId,
      totalItems,
      message: 'Request submitted. Use the request ID to check status.'
    });

  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

</div>

</div>

### Step 4: Allow Clients to Check Status

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9fc68a7e-546c-4228-abcd-99fff784595e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
app.get('/requests/:requestId', async (req, res) => {
  try {
    const { requestId } = req.params;

    const status = await processor.getRequestStatus(requestId);

    res.json({
      requestId,
      state: status.state,
      results: status.results,        // Only populated when PROCESSED or INFORMED
      metadata: status.metadata
    });

  } catch (error) {
    if (error.code === 'HTTP_NOT_FOUND') {
      res.status(404).json({ error: 'Request not found' });
    } else {
      res.status(500).json({ error: error.message });
    }
  }
});
```

</div>

</div>

------------------------------------------------------------------------

## Handling Large Requests (Direct Processing)

For requests with a large number of items, you may want to bypass the batching queue and process them directly. This avoids storing all individual items in the database.

### When to Use Direct Processing

- Request contains too many items (e.g., 10,000+ addresses)

### How It Works

1.  Pass `directProcess: true` to `processAsyncRequest()`

2.  The library creates a request and batch record, but **does not store individual items**

3.  Your `processBatch()` is called immediately with the items in memory

4.  When polling for results, the host project retrieves results directly from the third-party

5.  When the client checks status, the host project fetches results on-demand

### Implementation

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1727f35e-7a35-45c8-bea6-57e9f3a32fd4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// In your request handler
const itemCount = addresses.length;
const LARGE_REQUEST_THRESHOLD = 1000;

const isLargeRequest = itemCount >= LARGE_REQUEST_THRESHOLD;

const { clientRequestId, totalItems } = await processor.processAsyncRequest(
  addresses,
  isLargeRequest,  // directProcess = true for large requests
  tenantId,
  { 
    callbackUrl,
    isLargeRequest  // Store this flag for later
  }
);
```

</div>

</div>

### Retrieving Results for Large Requests

Since items aren't stored, override result handling:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fecd6f64-e0cd-4ba6-a570-0f6e80f14930" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// In your processor class
async pollBatchResult(batchId: string, metadata?: any): Promise<BatchProcessingResult | null> {
  const trackingId = metadata?.trackingId;
  const isLargeRequest = metadata?.isLargeRequest;

  const response = await this.externalAPI.getResults(trackingId);
  
  if (!response?.isComplete) return null;

  if (isLargeRequest) {
    // For large requests, don't store results in DB
    // Just mark the batch as processed
    return {
      batchId,
      isResultReady: true,
      results: [], // Empty - results not stored
      metadata: { ...metadata, completedAt: new Date().toISOString() }
    };
  }

  // Normal flow - store results
  // ...
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c7eb3c14-b6fa-41c8-bf2d-1f4361bfecf9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// In your status endpoint
app.get('/requests/:requestId', async (req, res) => {
  const status = await processor.getRequestStatus(requestId);
  
  // Check if this was a large request
  if (status.metadata?.isLargeRequest && status.state === 'PROCESSED') {
    // Fetch results directly from third-party
    const batchIds = await processor.getBatchIds(requestId);
    const batch = (await processor.queryBatches({ batchId: batchIds[0] }))[0];
    
    const results = await this.externalAPI.getResults(batch.metadata.trackingId);
    
    return res.json({
      requestId,
      state: status.state,
      results: results.data
    });
  }

  // Normal flow
  res.json({ requestId, state: status.state, results: status.results });
});
```

</div>

</div>

------------------------------------------------------------------------

### Methods to Implement

<div>

|  |  |  |
|----|----|----|
| **Method** | **Required** | **Description** |
| `processBatch(batch)` | ✅ Yes | Send batch to third-party, return result or tracking info |
| `pollBatchResult(batchId, metadata)` | ❌ No | Poll third-party for results (if not immediate) |
| `onRequestCompleted(requestId)` | ❌ No | Handle notification when request completes |

</div>

This is root page which includes recipes for all support cases as sub-pages.

There are 2 categories

- One is standard flows where all related information is storing in database of the library and would bundle up with other requests for processing effectively.

- One is direct flows where each request will be process individually and and directly. Please note that in the direct flow only part of information is storing in database, this part is metadata part not request payload.

**When to use standard flows and when to use direct flows?**

The direct flows should be used for cases where request already has large number of item e.g. 1000 items. Otherwise, use the standard flows.

## Library repository

<a href="https://bitbucket.org/axonivy-prod/luz_batch_typescript/src/master/" class="external-link" data-card-appearance="inline" data-local-id="b6b7958121c5" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_batch_typescript/src/master/</a>

## Creating tables in database

If your service/module is the first service/module to use this library. Then, it’s required to create required tables and their indexes. This can be done using luzfin_script. Then, please merge the branch below into master

<a href="https://bitbucket.org/axonivy-prod/luzfin_scripts/branch/future/LUZ-135270/add-script-to-create-database-of-batching" class="external-link" data-card-appearance="inline" data-local-id="82d7755ddd8d" rel="nofollow">https://bitbucket.org/axonivy-prod/luzfin_scripts/branch/future/LUZ-135270/add-script-to-create-database-of-batching</a>

%% ai-graph-start %%

**Related notes:**
- [[Recipe Typescript batching]]
- [[Luz Batch TypeScript - Sequence Diagram]]
- [[Batch Processor Library - NodeJS]]
- [[Luz Batch TypeScript - Configuration]]
- [[Batching Design]]

%% ai-graph-end %%