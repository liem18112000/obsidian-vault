---
ai_hash: e3829cde0d267aa1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48743710783/Luz+Batch+TypeScript+-+Configuration
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: Luz Batch TypeScript - Configuration
topic: programming
type: source
updated: 2025-12-01
---

# Luz Batch TypeScript - Configuration

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-12-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48743710783/Luz+Batch+TypeScript+-+Configuration)
> Relevance 0.738 · topic `programming`

Complete reference for all configuration options available when extending `AsyncProcessor` from `@luz/luz-batch-typescript`.

------------------------------------------------------------------------

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="11e7633e-6674-4056-8205-fd2865f55ab9" macro-name="toc">

</div>

------------------------------------------------------------------------

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f08572b0-b59f-4d1d-9cc7-9840b9d38c56" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
AsyncProcessorConfig {
  mode: 'IMMEDIATE_RESULTS' | 'CALLBACK_RESULTS';
  batchType: string;
  storage: StorageConfig;
  targetBatchSize?: number;
  batchCheckIntervalMs?: number;
  enableDebugLogs?: boolean;
  logLevel?: 'error' | 'warn' | 'info' | 'debug';
  batchResultCheckLeaseDurationMs?: number;
  maxConcurrentBatchCreations?: number;
  batchQueryPageSize?: number;
}

StorageConfig {
  type: 'postgresql';
  url: string;
  options?: {
    connectionPoolSize?: number;
    connectionTimeoutMillis?: number;
    idleTimeoutMillis?: number;
  };
}
```

</div>

</div>

## Required vs Optional

### ✅ REQUIRED

You **MUST** provide these configuration options:

<div>

|  |  |  |
|----|----|----|
| Parameter | Type | Example |
| `mode` | `'IMMEDIATE_RESULTS'` or `'CALLBACK_RESULTS'` | `'IMMEDIATE_RESULTS'` |
| `batchType` | `string` | `'MY_SERVICE_BATCH'` |
| `storage.type` | `'postgresql'` | `'postgresql'` |
| `storage.url` | `string` | `'postgresql://postgres:postgres@localhost:5432/luz_batching'` |

</div>

### ⚙️ OPTIONAL

Everything else has sensible defaults and can be omitted:

<div>

|  |  |  |
|----|----|----|
| Parameter | Default | Description |
| `storage.options.*` |  | Connection pool settings |
| `targetBatchSize` | `10` | Items per batch |
| `batchCheckIntervalMs` | `5000` | Background operations interval (ms) |
| `errorHandling.*` |  | Retry and error handling |
| `enableDebugLogs` | `false` | Enable debug logging |
| `logLevel` | `'info'` | Log level |
| `batchResultCheckLeaseDurationMs` | `3000` | Duration of the lease in milliseconds for batch result checking. |
| `maxConcurrentBatchCreations` | `5` | Maximum number of batches created concurrently during batch creation. |
| `batchQueryPageSize` | `1000` | Number of batch items fetched per database query during pagination. |

</div>

------------------------------------------------------------------------

## Configuration Parameters

1.  `mode` (REQUIRED)  
    **Type:** `'IMMEDIATE_RESULTS'` \| `'CALLBACK_RESULTS'`

    **When to use** `IMMEDIATE_RESULTS`**:**

    - ✅ Your `processBatch()` method **produces final results immediately**

    - ✅ Third-party API returns results synchronously

    - ✅ Results are available right after processing

    - ✅ **Examples:** Case 1 - Sending emails, database operations, calling REST APIs that return immediately

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ea070cc0-8d4e-48eb-9863-1d4db7f5aa30" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    mode: 'IMMEDIATE_RESULTS'
    ```

    </div>

    </div>

    **When to use** `CALLBACK_RESULTS`**:**

    - ✅ Your `processBatch()` **submits jobs to a third-party** that processes asynchronously

    - ✅ Results come back later (not during `processBatch()` execution)

    - ✅ You need to implement `pollBatchResults()` to check for results

    - ✅ **Examples:** Case 3 - Payment processing with async confirmation, video transcoding, long-running ML jobs

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="aea6afac-a301-464d-bb14-46000a6f7bd7" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    mode: 'CALLBACK_RESULTS'

    // You must also implement:
    async pollBatchResults() {
      // Check third-party service for results
      // Return results when ready, null when not ready
    }
    ```

    </div>

    </div>

2.  `batchType` (REQUIRED)  
    **Type:** `string`

    **Must be unique!** This identifier is used to:

    - ✅ Filter requests in the database (only processes requests with matching `batchType`)

    - ✅ Organize batches by service type

    - ✅ Isolate different services using the same Postgresql database

    **⚠️ CRITICAL: If not unique, you will face conflicts:**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="87b2000c-d31b-4508-bb84-894be284b46b" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    // ❌ BAD: Two services with same batchType
    class NotificationService extends AsyncProcessor {
      constructor() {
        super({ batchType: 'MY_SERVICE', ... });  // Same name!
      }
    }

    class EmailService extends AsyncProcessor {
      constructor() {
        super({ batchType: 'MY_SERVICE', ... });  // Same name!
      }
    }
    // Result: Both services will try to process each other's requests!
    ```

    </div>

    </div>

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ac30f5cd-3a2c-4e70-acab-9550c8b67427" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    // ✅ GOOD: Unique batchType for each service
    class Case1ImmediateService extends AsyncProcessor {
      constructor() {
        super({ batchType: 'CASE1_IMMEDIATE_PROCESSING', ... });
      }
    }

    class Case3CallbackService extends AsyncProcessor {
      constructor() {
        super({ batchType: 'CASE3_CALLBACK_PROCESSING', ... });
      }
    }
    // Result: Each service processes only its own requests
    ```

    </div>

    </div>

    **Best practices:**

    - Use descriptive names: `'USER_NOTIFICATIONS'`, `'PAYMENT_PROCESSING'`, `'ORDER_FULFILLMENT'`

    - Use UPPER_SNAKE_CASE for consistency

    - Include environment if needed: `'PROD_EMAIL_SERVICE'`, `'STAGING_EMAIL_SERVICE'`

    - Keep it short but meaningful

3.  `storage.type` (REQUIRED)  
    **Type:** `'string'`

    Only support PostgreSQL currently.

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fe7a0d14-c439-420b-bbcb-7218454e1f7b" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    storage: {
      type: 'postgresql',
    }
    ```

    </div>

    </div>

4.  `storage.url` (REQUIRED)  
    **Type:** `string`

    The database connection string.

    **Examples:**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="700e8c11-8f84-4453-94f0-972ee29f6cf6" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@localhost:${POSTGRES_PORT}/luz_batching
    ```

    </div>

    </div>

    **⚠️ Important:**

    - Ensure PostgreSQL is running and accessible

    - Use authentication in production

    - Store credentials in environment variables, not in code

    - Test connection before deploying

5.  `storage.options.connectionPoolSize` (OPTIONAL)  
    **Type:** `number`  
    **Default:** `100`  
    Maximum connections in pool

6.  `storage.options.connectTimeoutMs` (OPTIONAL)  
    **Type:** `number`  
    **Default:** `10000`  
    Connection timeout

7.  `storage.options.idleTimeoutMillis` (OPTIONAL)  
    **Type:** `number`  
    **Default:** `30000`  
    Idle connection timeout in pool

8.  `targetBatchSize` (OPTIONAL)  
    **Type:** `number`  
    **Default:** `100`

    How many items to include in each batch when calling `processBatch()`.

    **Choose based on:**

    - **API rate limits:** If calling external APIs, match their rate limits

    - **Processing time:** Larger batches = longer processing time per batch

    - **Memory usage:** Larger batches = more memory used

    - **Latency requirements:** Smaller batches = faster individual request processing

    **⚠️ Note:** This is a **target**, not a hard limit. The library will:

    - Create a batch when `targetBatchSize` is reached

    - OR create a batch when oldest pending item exceeds the interval time

    - The actual batch size may be less than target

9.  `batchCheckIntervalMs` (OPTIONAL)  
    **Type:** `number`  
    **Default:** `5000` (5 seconds)

    **⭐ CRITICAL UNDERSTANDING:** This is the **SINGLE interval that controls ALL background operations**.

    **This ONE interval controls ALL of these operations**// Inside BackgroundBatchProcessor (library code)

10. `enableDebugLogs` (OPTIONAL)  
    **Type:** `boolean`  
    **Default:** `false`

    Enable detailed debug logging from the library.

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9fbcf9d2-7873-4357-88c9-83c13c15f287" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    enableDebugLogs: true  // Enables verbose logging
    ```

    </div>

    </div>

    **When to enable:**

    - ✅ Development and debugging

    - ✅ Troubleshooting issues

    - ✅ Understanding library behavior

    **When to disable:**

    - ✅ Production (use `logLevel` instead)

    - ✅ Performance-sensitive environments

    - ✅ Clean logs needed

11. `logLevel` (OPTIONAL)  
    **Type:** `'error'` \| `'warn'` \| `'info'` \| `'debug'`  
    **Default:** `'info'`

    Set the logging verbosity level.

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a6932101-20c1-4081-9470-00890f842f40" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    logLevel: 'debug'  // Most verbose
    logLevel: 'info'   // Recommended for production
    logLevel: 'warn'   // Only warnings and errors
    logLevel: 'error'  // Only errors
    ```

    </div>

    </div>

    **Log levels:**

    - `'error'` - Only errors (least verbose)

    - `'warn'` - Warnings and errors

    - `'info'` - General information (recommended for production)

    - `'debug'` - Detailed debugging information (recommended for development)

12. `batchResultCheckLeaseDurationMs` (OPTIONAL)  
    **Type:** `number`  
    **Default:** `3000`  
    Duration of the lease in milliseconds for batch result checking.

13. `maxConcurrentBatchCreations` (OPTIONAL)  
    **Type:** `number`  
    **Default:** `5`  
    Maximum number of batches created concurrently during batch creation.

14. `batchQueryPageSize` (OPTIONAL)  
    **Type:** `number`  
    **Default:** `1000`  
    Number of batch items fetched per database query during pagination.

%% ai-graph-start %%

**Related notes:**
- [[Recipe Luz Batch TypeScript]]
- [[Recipe Typescript batching]]
- [[Luz Batch TypeScript - Sequence Diagram]]
- [[Batch Processor Library - NodeJS]]
- [[Batching Design]]

%% ai-graph-end %%