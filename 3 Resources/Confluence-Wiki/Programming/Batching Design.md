---
ai_hash: 0bfdf2d6735923f6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.4
entities: []
relevance: 0.766
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/49005101152/Batching+Design
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: Batching Design
topic: programming
type: source
updated: 2025-12-26
---

# Batching Design

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-12-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/49005101152/Batching+Design)
> Relevance 0.766 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="dec2ef7e-6a37-4b26-975f-f30821f99d5c" macro-name="toc">

</div>

## Typescript version

### Class diagram

<div id="expander-84923149" class="expand-container conf-macro output-block" hasbody="true" macro-id="ae3af4df-556d-460d-847b-e723b116dc5e" macro-name="expand">

<div id="expander-control-84923149" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">class diagram in mermaid format</span>

</div>

<div id="expander-content-84923149" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ea45e246-ecc7-449c-ab65-61cb41584179" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
%% ==================================================================================
%% BATCHING LIBRARY - COMPLETE CLASS DIAGRAM
%% For Java Implementation Reference
%% ==================================================================================
%% 
%% ARCHITECTURE OVERVIEW:
%% ----------------------
%% This library provides async batch processing with the following key features:
%% 1. Automatic batching of incoming requests
%% 2. Background processing with separate schedulers for creation and processing
%% 3. Multi-pod safe operations using database leases
%% 4. Support for both immediate results and async polling/webhook patterns
%% 5. Automatic retry and error recovery
%%
%% MAIN FLOWS:
%% -----------
%% Standard Batching: Request → Store Items → Create Batch → Process → Poll/Webhook → Complete → Notify
%% Direct Processing: Request → Process Immediately → Store Results → Notify
%%
%% STATE MACHINES:
%% ---------------
%% BatchState:     PENDING → PROCESSING → SUBMITTED → PROCESSED
%%                 PROCESSING → SUBMITTED_FAILED → PENDING (retry)
%% BatchItemState: PENDING → PROCESSING → PROCESSED
%% RequestState:   PENDING → PROCESSING → PROCESSED → NOTIFIED
%%
%% DATABASE TABLES:
%% ----------------
%% requests(id, tenant_id, metadata, state, notification_lease_expires_at, ...)
%% batches(id, batch_type, state, metadata, result_check_lease_expires_at, ...)
%% batch_items(id, original_payload, batch_type, state, request_id, batch_id, result, ...)
%%
%% ==================================================================================

classDiagram

    %% ====================================================================================
    %% CORE PROCESSOR - Abstract Base Class (Services Must Extend This)
    %% ====================================================================================

    class AsyncProcessor {
        <<abstract>>
        -BackgroundBatchProcessor backgroundProcessor
        -boolean isProcessing
        -AsyncBatchErrorHandler errorHandler
        +AsyncProcessorConfig config
        #AsyncLogger logger
        #BatchingDAO batchingDAO

        +start() void
        +stop() void
        +processAsyncRequest(sourceData, directProcess, tenantId, metadata) ClientRequestResult
        +processBatch(batch) BatchProcessingResult
        +pollBatchResult(batchId, metadata) BatchProcessingResult
        +onRequestCompleted(requestId) void
        +updateBatchResult(results) void
        +queryBatches(criteria) List~Batch~
        +getBatchStatus(batchId) BatchState
        +getRequestStatus(requestId) RequestStatusResponse
        +queryBatchItems(criteria) List~BatchItem~
        +getBatchIds(requestId) List~String~
        +getRequestIds(batchId) List~String~
    }

    note for AsyncProcessor "processBatch() = ABSTRACT (must implement)\npollBatchResult() = OPTIONAL hook\nonRequestCompleted() = OPTIONAL hook"

    %% ====================================================================================
    %% BACKGROUND PROCESSING COORDINATOR
    %% ====================================================================================

    class BackgroundBatchProcessor {
        -boolean isRunning
        -AsyncProcessor processor
        -BatchingDAO batchingDAO
        -AsyncLogger logger
        -BatchCreationScheduler creationScheduler
        -BatchProcessingScheduler processingScheduler

        +start() void
        +stop() void
        +triggerImmediateProcessing() void
        +triggerImmediateBatchCreation() void
        +triggerImmediateBatchProcessing() void
        +storeBatchResult(batchResult) void
    }

    %% ====================================================================================
    %% SCHEDULER - BATCH CREATION
    %% ====================================================================================

    class BatchCreationScheduler {
        -boolean isRunning
        -boolean isProcessing
        -ScheduledExecutorService scheduler
        -AsyncProcessor processor
        -BatchingDAO batchingDAO
        -AsyncLogger logger
        -long intervalMs
        -int maxConcurrentBatchCreations
        -int batchQueryPageSize

        +start() void
        +stop() void
        +triggerImmediate() void
        -runCycle() void
        -createBatches(batchType, targetBatchSize) List~String~
        -createSingleBatch(batchType, batch) String
        -recoverStuckBatches(timeoutMs) void
    }

    %% ====================================================================================
    %% SCHEDULER - BATCH PROCESSING
    %% ====================================================================================

    class BatchProcessingScheduler {
        -boolean isRunning
        -boolean isProcessing
        -ScheduledExecutorService scheduler
        -AsyncProcessor processor
        -BatchingDAO batchingDAO
        -AsyncLogger logger
        -AsyncBatchErrorHandler errorHandler
        -long intervalMs

        +start() void
        +stop() void
        +triggerImmediate() void
        +storeBatchResult(batchResult) void
        -runCycle() void
        -processBatches(batchType) void
        -processBatch(batch) BatchProcessingResult
        -pollBatchResults() void
        -completeRequests() void
        -notifyClients() void
        -recoverExpiredLeases() void
    }

    %% ====================================================================================
    %% DATA ACCESS LAYER - INTERFACE
    %% ====================================================================================

    class BatchingDAO {
        <<interface>>
        +connect() void
        +disconnect() void
        +beginTransaction() TransactionContext
        +commitTransaction(context) void
        +rollbackTransaction(context) void
        +createRequest(tenantId, metadata, initialState, context) String
        +getRequest(requestId, context) Request
        +updateRequestState(requestId, state, context) void
        +updateRequestsState(requestIds, state, context) int
        +queryRequests(criteria, context) List~Request~
        +queryRequestsByBatchType(batchType, state, context) List~Request~
        +claimRequestForNotification(requestId, leaseDurationMs) boolean
        +clearNotificationLease(requestId) void
        +storeBatchItems(items, context) List~String~
        +queryBatchItems(criteria, context) List~BatchItem~
        +countBatchItems(criteria, context) int
        +updateBatchItems(items, currentState, context) int
        +createBatch(batchType, metadata, initialState, context) String
        +queryBatches(criteria, context) List~Batch~
        +updateBatch(batchId, updates, currentState, context) void
        +getBatchStatus(batchId, context) BatchState
        +deleteBatch(batchId, context) void
        +claimBatchForResultCheck(batchId, leaseDurationMs) boolean
        +clearResultCheckLease(batchId) void
    }

    %% ====================================================================================
    %% DATA ACCESS LAYER - POSTGRESQL IMPLEMENTATION
    %% ====================================================================================

    class PostgreSQLBatchingDAO {
        -DataSource dataSource
        -AsyncLogger logger
        -boolean isConnected
        +connect() void
        +disconnect() void
        -createTablesAndIndexes() void
        -executeQuery(query, values, context) ResultSet
        -ensureConnected() void
    }

    note for PostgreSQLBatchingDAO "Implements all BatchingDAO methods\nManages 3 tables: requests, batches, batch_items"

    class TransactionContext {
        +Connection connection
        +boolean isActive
    }

    %% ====================================================================================
    %% ERROR HANDLING
    %% ====================================================================================

    class AsyncBatchErrorHandler {
        -AsyncLogger logger
        +handleError(error, context) AsyncBatchError
        +isRetryableError(error) boolean
        +getRetryDelay(error, attempt, maxDelayMs) long
    }

    class ErrorFactory {
        <<utility>>
        +createError(code, message, context, cause) AsyncBatchError
        +dbConnectionFailed(cause, context) AsyncBatchError
        +dbQueryFailed(operation, cause, context) AsyncBatchError
        +transactionFailed(operation, cause, context) AsyncBatchError
        +recordNotFound(type, id, context) AsyncBatchError
        +emptyRequest(context) AsyncBatchError
        +invalidState(state, expected, context) AsyncBatchError
    }

    %% ====================================================================================
    %% STATE MACHINE
    %% ====================================================================================

    class BatchStateMachine {
        <<utility>>
        -Map~BatchState, Set~ TRANSITIONS
        +isValidTransition(current, next) boolean
        +validateTransition(current, next, batchId) void
        +getAllowedNextStates(current) List~BatchState~
        +isTerminalState(state) boolean
        +getStandardInitialState() BatchState
        +getDirectProcessingInitialState() BatchState
    }

    %% ====================================================================================
    %% LOGGING INTERFACE
    %% ====================================================================================

    class AsyncLogger {
        <<interface>>
        +error(message, args) void
        +warn(message, args) void
        +info(message, args) void
        +debug(message, args) void
    }

    class ConsoleLogger {
        -LogLevel logLevel
        +error(message, args) void
        +warn(message, args) void
        +info(message, args) void
        +debug(message, args) void
    }

    %% ====================================================================================
    %% CONFIGURATION
    %% ====================================================================================

    class AsyncProcessorConfig {
        +ProcessingMode mode
        +String batchType
        +StorageConfig storage
        +int targetBatchSize
        +long batchCheckIntervalMs
        +LogLevel logLevel
        +long batchResultCheckLeaseDurationMs
        +long notificationLeaseDurationMs
        +int maxConcurrentBatchCreations
        +int batchQueryPageSize
        +long batchCreationIntervalMs
        +long batchProcessingIntervalMs
    }

    class StorageConfig {
        +StorageType type
        +String url
        +int connectionPoolSize
        +long connectionTimeoutMillis
        +long idleTimeoutMillis
    }

    %% ====================================================================================
    %% DOMAIN ENTITIES
    %% ====================================================================================

    class Request {
        +String id
        +String tenantId
        +Object metadata
        +RequestState state
        +Instant notificationLeaseExpiresAt
        +Instant submittedAt
        +Instant completedAt
        +Instant createdAt
        +Instant updatedAt
    }

    class Batch {
        +String id
        +String batchType
        +BatchState state
        +Object metadata
        +Instant resultCheckLeaseExpiresAt
        +Instant createdAt
        +Instant updatedAt
    }

    class BatchItem {
        +String id
        +Object originalPayload
        +String batchType
        +BatchItemState state
        +String requestId
        +String batchId
        +Object result
        +Instant createdAt
        +Instant updatedAt
    }

    %% ====================================================================================
    %% DATA TRANSFER OBJECTS
    %% ====================================================================================

    class BatchRequestData {
        +List~BatchItem~ items
        +Object metadata
    }

    class BatchProcessingResult {
        +String batchId
        +List~BatchItem~ results
        +Object metadata
        +boolean isResultReady
    }

    class ClientRequestResult {
        +String clientRequestId
        +int totalItems
    }

    class RequestStatusResponse {
        +RequestState state
        +String tenantId
        +Object metadata
        +List~Object~ results
    }

    class AsyncBatchError {
        +String name
        +String message
        +ErrorCode code
        +boolean isRetryable
        +Throwable cause
        +Object context
    }

    %% ====================================================================================
    %% QUERY CRITERIA
    %% ====================================================================================

    class BatchItemQueryCriteria {
        +String batchType
        +BatchItemState state
        +String requestId
        +String batchId
        +Integer limit
    }

    class BatchCriteria {
        +String batchId
        +String batchType
        +BatchState state
        +Integer limit
        +Instant createdBefore
        +Instant createdAfter
    }

    %% ====================================================================================
    %% ENUMERATIONS
    %% ====================================================================================

    class BatchState {
        <<enumeration>>
        PENDING
        PROCESSING
        SUBMITTED
        SUBMITTED_FAILED
        PROCESSED
    }

    class BatchItemState {
        <<enumeration>>
        PENDING
        PROCESSING
        PROCESSED
    }

    class RequestState {
        <<enumeration>>
        PENDING
        PROCESSING
        PROCESSED
        NOTIFIED
    }

    class ProcessingMode {
        <<enumeration>>
        IMMEDIATE_RESULTS
        CALLBACK_RESULTS
    }

    class StorageType {
        <<enumeration>>
        POSTGRESQL
    }

    class LogLevel {
        <<enumeration>>
        ERROR
        WARN
        INFO
        DEBUG
    }

    class ErrorCode {
        <<enumeration>>
        NETWORK_ERROR
        HTTP_CLIENT_ERROR
        HTTP_SERVER_ERROR
        DB_CONNECTION_ERROR
        DB_QUERY_ERROR
        DB_TRANSACTION_ERROR
        VALIDATION_ERROR
        PROCESSING_ERROR
        CONFIG_ERROR
        TIMEOUT_ERROR
        UNKNOWN_ERROR
    }

    %% ====================================================================================
    %% RELATIONSHIPS - COMPOSITION
    %% ====================================================================================

    AsyncProcessor *-- BackgroundBatchProcessor : owns
    AsyncProcessor *-- AsyncBatchErrorHandler : owns
    AsyncProcessor o-- BatchingDAO : uses
    AsyncProcessor o-- AsyncProcessorConfig : configured by
    AsyncProcessor o-- AsyncLogger : uses

    BackgroundBatchProcessor *-- BatchCreationScheduler : owns
    BackgroundBatchProcessor *-- BatchProcessingScheduler : owns

    BatchCreationScheduler o-- BatchingDAO : uses
    BatchCreationScheduler o-- BatchStateMachine : uses
    BatchCreationScheduler ..> AsyncProcessor : calls processBatch

    BatchProcessingScheduler o-- BatchingDAO : uses
    BatchProcessingScheduler o-- BatchStateMachine : uses
    BatchProcessingScheduler o-- AsyncBatchErrorHandler : uses
    BatchProcessingScheduler ..> AsyncProcessor : calls processBatch/pollBatchResult/onRequestCompleted

    %% ====================================================================================
    %% RELATIONSHIPS - INTERFACE IMPLEMENTATION
    %% ====================================================================================

    PostgreSQLBatchingDAO ..|> BatchingDAO : implements
    ConsoleLogger ..|> AsyncLogger : implements

    %% ====================================================================================
    %% RELATIONSHIPS - CREATION
    %% ====================================================================================

    PostgreSQLBatchingDAO ..> TransactionContext : creates
    AsyncBatchErrorHandler ..> ErrorFactory : uses
    ErrorFactory ..> AsyncBatchError : creates

    %% ====================================================================================
    %% RELATIONSHIPS - DATA MANAGEMENT
    %% ====================================================================================

    AsyncProcessorConfig *-- StorageConfig : contains

    BatchingDAO ..> Request : manages
    BatchingDAO ..> Batch : manages
    BatchingDAO ..> BatchItem : manages

    Request --> RequestState : has state
    Batch --> BatchState : has state
    BatchItem --> BatchItemState : has state

    BatchItem --> Request : belongs to
    BatchItem --> Batch : belongs to

    BatchStateMachine ..> BatchState : validates transitions

    AsyncProcessor ..> BatchProcessingResult : returns from processBatch
    AsyncProcessor ..> BatchRequestData : passes to processBatch
    AsyncProcessor ..> RequestStatusResponse : returns from getRequestStatus
    AsyncProcessor ..> ClientRequestResult : returns from processAsyncRequest
```

</div>

</div>

</div>

</div>


![[49005101152-batching-typescript-class-diagram.png]]

%% ai-graph-start %%

**Related notes:**
- [[Batch Processor Library - NodeJS]]
- [[Luz Batch TypeScript - Sequence Diagram]]
- [[Recipe Typescript batching]]
- [[Recipe Luz Batch TypeScript]]
- [[Concurrency Design Patterns]]

%% ai-graph-end %%