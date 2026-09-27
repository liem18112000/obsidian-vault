---
title: "Recipe: Typescript batching"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49033314460/Recipe+Typescript+batching
space: "LUZ"
topic: programming
relevance: 0.818
depth: 3
updated: 2026-01-13
attachments: 1
tags:
  - confluence
  - programming
  - space/luz
---

# Recipe: Typescript batching

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-01-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49033314460/Recipe+Typescript+batching)
> Relevance 0.818 · topic `programming`

A TypeScript library for batching async requests from multiple clients with PostgreSQL storage. This package allows your service to efficiently group individual requests into batches, process them with a third-party service, and return results to the original callers.

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="afab29e2-0139-48ea-9603-87bae7c8578f" macro-name="toc">

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

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c85685be-4117-474f-998f-973d730cc91a" macro-name="code" style="border-width: 1px;">

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


![[49033314460-image-20251231-081739.png]]



Then install the dependency by running `npm install`

------------------------------------------------------------------------

## Step-by-Step Integration Guide

### Step 1: Create Your Processor Class

Extend the `AsyncProcessor` base class with your input type:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3d1eafea-ff15-4050-a04a-3a052d234fb9" macro-name="code" style="border-width: 1px;">

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

### Step 2: Handle Client Requests

When your service receives requests from clients, pass the request data to the library to store by calling `processAsyncRequest` of the Processor you created in step 1:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8e1fc922-cde7-4ac6-a0bd-96a7528652be" macro-name="code" style="border-width: 1px;">

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

### Step 3: Implement the Required Methods

#### `processBatch()` - Required

Implement this method in your Processor class that you created in step 1.

This method is called when a batch is ready to be sent to your third-party service:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="01d296ba-c64d-4a3b-be78-bd6215e6dd14" macro-name="code" style="border-width: 1px;">

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

Implement this method in your Processor class that you created in step 1.

After `processBatch()` send the batches to the 3rd party service, we can start polling the 3rd party service for results of the batches.

Implement this `pollBatchResult()` method if your third-party service doesn't return results immediately during `processBatch()`:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="45baf0fb-215e-4c28-9155-5177c723f735" macro-name="code" style="border-width: 1px;">

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

Implement this method in your Processor class that you created in step 1.

Called when all items in a request are processed:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="69d82c19-758a-4ec0-8537-cfc634fa7e56" macro-name="code" style="border-width: 1px;">

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

### Step 4: Allow Clients to Check Status

When the clients of your service want to get the request status and content from your service, call the `getRequestStatus` method from your Processor class from step 1:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="46f074d4-ada7-4e87-be8b-c0eb5d1ee631" macro-name="code" style="border-width: 1px;">

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

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d310af2c-51d2-45dd-9b7d-fe2b8618c9b0" macro-name="code" style="border-width: 1px;">

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

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5e6939e4-e0d4-45dd-9440-f48a9cbcd261" macro-name="code" style="border-width: 1px;">

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

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="22318994-a0da-4830-aa03-7d71a782be05" macro-name="code" style="border-width: 1px;">

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

## Receiving Results via Webhook (Push from 3rd Party)

In some cases, the third-party service will actively send batch results back to your service via webhook/notification, rather than having you poll for results. The library provides the `updateBatchResult()` method to handle this scenario.

### When to Use This Pattern

- Third-party service sends results via webhook/callback

- Push-based result delivery instead of polling

- Real-time result processing without polling overhead

- Third-party provides a webhook URL configuration

### Implementation Steps

Add a webhook endpoint to your service that the third-party will call, and in that endpoint, call the `updateBatchResult()` method from the Processor you created in step 1:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a9184f0a-8355-4dd0-aa0e-f2b78004532f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Express.js example
app.post('/webhooks/batch-results', async (req, res) => {
  try {
    const { batchId, trackingId, results } = req.body;
    
    this.logger?.info(`Received webhook for batch ${batchId}`);

    // Validate the webhook (implement your security checks)
    if (!isValidWebhookSignature(req)) {
      return res.status(401).json({ error: 'Invalid signature' });
    }

    // Get the original batch items to map results
    const batchItems = await processor.queryBatchItems({ batchId });

    // Map third-party results to batch items
    const processedItems: BatchItem[] = batchItems.map((item, idx) => ({
      ...item,
      state: 'PROCESSED',
      result: results[idx]
    }));

    // Update the batch result using the library method
    await processor.updateBatchResult({
      batchId,
      isResultReady: true,
      results: processedItems,
      metadata: {
        trackingId,
        completedAt: new Date().toISOString(),
        receivedViaWebhook: true
      }
    });

    // Acknowledge receipt
    res.status(200).json({ status: 'success', batchId });

  } catch (error) {
    this.logger?.error('Webhook processing failed', { error });
    res.status(500).json({ error: error.message });
  }
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
| `pollBatchResult(batchId, metadata)` | ❌ No | Poll third-party for results (if not immediate). Not needed if using webhooks exclusively. |
| `onRequestCompleted(requestId)` | ❌ No | Handle notification when request completes |

</div>
