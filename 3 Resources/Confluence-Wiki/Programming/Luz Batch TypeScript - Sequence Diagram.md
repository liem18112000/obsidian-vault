---
ai_hash: 0b36b37f9f7458df
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 14
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48719528005/Luz+Batch+TypeScript+-+Sequence+Diagram
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: Luz Batch TypeScript - Sequence Diagram
topic: programming
type: source
updated: 2025-10-08
---

# Luz Batch TypeScript - Sequence Diagram

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-10-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48719528005/Luz+Batch+TypeScript+-+Sequence+Diagram)
> Relevance 0.738 · topic `programming`

# Use cases


![[48719528005-luz-batching-typescript-usecases.png]]



# Case 1: Immediate Results + Client Polling


![[48719528005-Case_1_Immediate_Results_Client_Polling.png]]



<div id="expander-1692686064" class="expand-container conf-macro output-block" hasbody="true" macro-id="b2fb8131-77df-4d85-87e0-0b190f07c96d" macro-name="expand">

<div id="expander-control-1692686064" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Code Example</span>

</div>

<div id="expander-content-1692686064" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a6df8890-5d33-439c-9c40-3dfde92209e0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/**
 * Case 1: Immediate Results + Client Polling - Format 2 + Two-Phase Processing
 * 
 * 🎯 PURPOSE: Show developers exactly how to use the AsyncHttpProcessor library
 * 
 * 📋 WHAT YOU NEED TO IMPLEMENT:
 * 1. Extend AsyncHttpProcessor<YourRequestType, YourResultType>
 * 2. Implement processBatchItems() - Phase 1: Get raw results from third-party
 * 3. Implement mapBatchResults() - Phase 2: Map raw results to BatchItem[]
 * 4. Add public methods with inline validation for your controller to call
 * 
 * 📄 DTO REFERENCE: See order-processing.types.ts for OrderRequest structure
 * 
 * 🔄 LIBRARY FLOW (You don't implement this - library handles it):
 * Client → Controller → Service.submitOrder() → Library.processAsyncRequest() → HTTP 202
 * Background Phase 1: Library → Service.processBatchItems() → Library stores raw results
 * Background Phase 2: Library → Service.mapBatchResults() → Library stores final results
 * Polling: Client → Controller → Service.getOrderStatus() → Library.getRequestStatus()
 */

import { Injectable, Logger } from '@nestjs/common';
import { AsyncHttpProcessor } from '../../src/processors/http/async-http-processor.service';
import { BatchItem } from '../../src/types/async-batch-processor.types';
import { 
  OrderRequest, 
  OrderResult, 
  OrderStatus
} from './order-processing.types';

// ============================================================================
// 📝 STEP 1: EXTEND THE LIBRARY CLASS
// ============================================================================

@Injectable()
export class OrderProcessingService extends AsyncHttpProcessor<OrderRequest, any> {
  private readonly logger = new Logger(OrderProcessingService.name);

  constructor() {
    // 🔧 CONFIGURE THE LIBRARY - Simple inline 3-phase config
    const config = {
      // Global settings used across all phases
      global: {
        // 🗄️ STORAGE CONFIGURATION (EXAMPLE - NOT FINALIZED)
        // NOTE: The final storage implementation will depend on your chosen database/service:
        // - Could be PostgreSQL, MongoDB, Redis, DynamoDB, etc.
        // - Could be a dedicated microservice, cloud service, or embedded database
        // - This example shows the interface structure, not the final implementation
        storage: {
          baseUrl: 'https://your-luz-batching-service.com',  // Your storage service endpoint
          timeoutMs: 30000,                                   // Connection timeout
          retryConfig: {
            enabled: true,
            maxRetries: 3,
            retryDelayMs: 1000
          }
        },
        batchType: 'ORDER_PROCESSING',
        logLevel: 'info' as const
      },
      
      // Request submission phase (reserved for future features)
      requestSubmission: {},
      
      // Background processing phase
      backgroundProcessing: {
        targetBatchSize: 10,                    // How many requests to batch together
        maxBatchWaitMs: 5000,                   // Max wait before processing smaller batch
        mode: 'IMMEDIATE_RESULTS' as const,     // Process and store results immediately
        processingTimeoutMs: 300000             // 5 minute timeout for processing
      },
      
      // Result return phase  
      resultReturn: {
        method: 'POLLING' as const,             // Client polls for results
        polling: {
          enableStatusEndpoint: true,           // Enable status checking endpoint
          statusCacheMs: 5000                   // Cache status for 5 seconds
        }
      }
    };
    
    super(config, new Logger(OrderProcessingService.name));
  }

  // ============================================================================
  // 📝 STEP 2: ADD PUBLIC METHODS FOR YOUR CONTROLLER TO CALL
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Submit order for processing
   * 
   * This is what your controller calls. This method validates and enriches
   * the request data, then submits to the library for async processing.
   * 
   * 💡 VALIDATION: If validation fails, throws HTTP 400 error immediately.
   * If validation passes, returns { requestId } for client polling.
   */
  async submitOrder(orderRequest: OrderRequest): Promise<{ requestId: string }> {
    this.logger.log(`📥 Submitting order request: ${orderRequest.id}`);
    
    // 🔍 STEP 2.1: VALIDATE REQUEST (throw HTTP 400 errors for invalid data)
    if (!orderRequest.customerId) {
      throw new Error('Customer ID is required');
    }
    if (!orderRequest.items || orderRequest.items.length === 0) {
      throw new Error('Order must contain at least one item');
    }
    if (orderRequest.items.some(item => item.price <= 0)) {
      throw new Error('All items must have positive prices');
    }
    // Add more validation as needed...
    
    // 🔧 STEP 2.2: ENRICH REQUEST DATA (prepare for processing)
    const enrichedOrder = {
      // Include original data
      ...orderRequest,
      
      // Add computed/normalized fields (customize for your needs)
      normalizedCustomerId: orderRequest.customerId.toUpperCase(),
      normalizedCurrency: orderRequest.currency.toUpperCase(),
      processedTimestamp: new Date().toISOString(),
      
      // Add business context (customize for your needs)
      processingPriority: orderRequest.totalAmount > 1000 ? 'HIGH' : 'NORMAL',
      itemCount: orderRequest.items.length,
      
      // Add any fields your business logic needs...
    };
    
    // 🔵 CALL LIBRARY: Submit pre-validated data for async processing
    const result = await this.processAsyncRequest(enrichedOrder);
    
    this.logger.log(`✅ Order submitted with requestId: ${result.requestId}`);
    // 🔔 NEXT STEP: Library will batch requests and call processBatchItems() (Step 3)
    return result; // Returns { requestId: "abc123" } immediately
  }


  // ============================================================================
  // 📝 STEP 3: IMPLEMENT BUSINESS LOGIC (Called during background processing)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Phase 1 - Get raw results from third-party
   * 
   * ⚡ WHEN: Called later in background after HTTP 202 response sent
   *         Triggered when batch conditions are met:
   *         - Batch reaches targetBatchSize (10 requests in this config)
   *         - OR maxBatchWaitMs timeout expires (5000ms in this config)
   *         - OR library shutdown/flush is triggered
   * 
   * 🎯 PURPOSE: Heavy business logic, third-party API calls, database operations
   * 📋 YOUR JOB: Process the actual business logic and return raw results
   * 
   * 💡 IMPORTANT: items contains the validated/enriched data from submitOrder()
   *               This is the data you prepared in Step 2.1 validation + Step 2.2 enrichment
   */
  async processBatchItems(items: BatchItem[]): Promise<any> {
    this.logger.log(`🔄 Processing batch of ${items.length} validated orders`);
    
    // 🔄 PROCESS THE BATCH: Choose your approach based on your third-party APIs
    // 💡 NOTE: Each 'item' contains the validated/enriched data from submitOrder()
    //          This includes original request data + computed fields from Step 2.2
    //
    // 🔑 IMPORTANT: item.request = Business data (orderId from client)
    //              item.requestId = Library tracking ID (different thing!)
    //
    // 🎯 PROCESSING OPTIONS:
    // Option A: Bulk API - Send all items at once (no loop needed)
    //   const bulkResult = await this.thirdPartyAPI.processBulk(items.map(i => i.request));
    //
    // Option B: Individual processing - Loop through each order (example below)
    //   Use this when you need order-by-order processing or error handling
    
    // Extract pure business data from Format 2 BatchItem[]
    const businessData = items.map(item => item.request);
    
    // 🔄 CALL THIRD-PARTY API with clean business data
    const rawResults = await this.callThirdPartyAPI(businessData);
    
    this.logger.log(`✅ Step 3 complete: Got raw results from third-party`);
    
    // Return raw results - library stores immediately for data safety
    return rawResults;
  }

  // ============================================================================
  // 📝 STEP 4: IMPLEMENT MAPPING LOGIC (Phase 2 - Called during background processing)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Phase 2 - Map raw results to final BatchItem[]
   * 
   * ⚡ WHEN: Called after Phase 1 raw results are stored
   * 🎯 PURPOSE: Map raw third-party results back to original requests
   * 📋 YOUR JOB: Correlate raw results with requests and return Format 2 BatchItem[]
   * 
   * 💡 IMPORTANT: Library provides Format 2 structure with requestId and enriched data for correlation
   */
  async mapBatchResults(requests: BatchItem[], rawResults: any): Promise<BatchItem[]> {
    this.logger.log(`🔄 Step 4: Mapping ${rawResults.length} raw results`);
    
    // requests = [
    //   { requestId: "req_1", request: { orderId: "ORD_123", amount: 100, normalizedCustomerId: "CUST_456", processingPriority: "HIGH" } },
    //   { requestId: "req_2", request: { orderId: "ORD_456", amount: 200, normalizedCustomerId: "CUST_789", processingPriority: "NORMAL" } }
    // ]
    // rawResults = [
    //   { order_id: "ORD_456", status: "failed", reason: "insufficient_funds" },
    //   { order_id: "ORD_123", status: "completed", confirmation: "ABC123" }
    // ]
    
    // Simply add response to each request by correlating with raw results
    return requests.map(request => {
      // Find matching raw result by business correlation (orderId ↔ order_id)
      const matchingResult = rawResults.find(result => result.order_id === request.request.orderId);
      
      if (!matchingResult) {
        throw new Error(`No matching result found for orderId: ${request.request.orderId}`);
      }
      
      // Build final result from raw third-party response
      const finalResult: OrderResult = {
        id: request.request.orderId,
        status: matchingResult.status === 'completed' ? 'SUCCESS' : 'ERROR',
        processedAt: new Date(),
        totalAmount: request.request.totalAmount,
        currency: request.request.currency || 'USD',
        errorDetails: matchingResult.reason || undefined,
        validationResult: {
          isValid: matchingResult.status === 'completed',
          errors: matchingResult.status === 'completed' ? [] : [matchingResult.reason || 'Processing failed'],
          warnings: [],
          processedItems: []
        }
      };
      
      // Return the same BatchItem with response added
      return {
        ...request,           // Keep requestId and request as-is
        response: finalResult // Add the response
      };
    });
  }

  // ============================================================================
  // 📝 STEP 5: CLIENT POLLING - FINAL STEP (Returns results to client)
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Get order processing status - FINAL STEP
   * 
   * This is where clients actually retrieve the results from Step 4.
   * This is what your controller calls for client polling.
   * 
   * 💡 RETURNS RESULTS FROM STEP 4: The results you built in mapBatchResults()
   */
  async getOrderStatus(requestId: string): Promise<OrderStatus> {
    this.logger.debug(`📊 Getting status for request: ${requestId}`);
    
    // 🔵 CALL LIBRARY: This retrieves results stored from Step 4
    const status = await this.getRequestStatus(requestId);
    
    if (!status) {
      throw new Error(`Request ${requestId} not found`);
    }

    // 🔄 TRANSFORM: Convert library format to your API format
    // The 'result' field contains what you built in Step 4 (mapBatchResults)
    // 🔔 NEXT STEP: Return final results to client - processing complete!
    return {
      requestId,
      status: status.status as 'PENDING' | 'SUCCESS' | 'ERROR',
      result: status.result as OrderResult, // This is your result from Step 4
      errorDetails: status.errorDetails,
      createdAt: status.createdAt,
      updatedAt: status.updatedAt
    };
  }

  private async callThirdPartyAPI(businessData: any[]): Promise<any[]> {
    // 📝 SIMULATE THIRD-PARTY API CALL: Replace with your actual API
    this.logger.log(`🌐 Calling third-party API with ${businessData.length} orders`);
    
    // Simulate API delay
    await new Promise(resolve => setTimeout(resolve, 100));
    
    // Mock third-party response (out of order to test mapping)
    return businessData.map(data => ({
      order_id: data.orderId,  // Third-party uses different field name
      status: Math.random() > 0.2 ? 'completed' : 'failed',  // 80% success rate
      confirmation: 'CONF_' + Math.random().toString(36).substr(2, 9),
      reason: Math.random() > 0.2 ? undefined : 'insufficient_funds'
    }));
  }

  // ============================================================================
  // 📝 THAT'S IT! 🎉
  // ============================================================================
  
  /*
   * 🎯 SUMMARY: What you implemented above:
   * 
   * 1️⃣ EXTENDED the library class: AsyncHttpProcessor<YourRequestType, YourResultType>
   * 2️⃣ ADDED public method submitOrder() with inline validation & enrichment
   * 3️⃣ IMPLEMENTED processBatchItems() - Phase 1: Get raw results from third-party
   * 4️⃣ IMPLEMENTED mapBatchResults() - Phase 2: Map raw results to Format 2 BatchItem[]
   * 5️⃣ IMPLEMENTED getOrderStatus() for client polling (returns final results)
   * 
   * 🔄 WHAT THE LIBRARY HANDLES FOR YOU:
   * - Batching requests together
   * - Two-phase processing orchestration
   * - Raw result storage (data safety)
   * - Final result storage and mapping
   * - Background processing orchestration
   * - Error handling and retries
   * - Status tracking and polling
   * 
   * 📋 WHAT YOUR CONTROLLER LOOKS LIKE:
   * 
   * @Controller('orders')
   * export class OrderController {
   *   constructor(private orderService: OrderProcessingService) {}
   * 
   *   @Post()
   *   async submitOrder(@Body() order: OrderRequest) {
   *     return this.orderService.submitOrder(order); // Returns { requestId }
   *   }
   * 
   *   @Get(':requestId/status')
   *   async getStatus(@Param('requestId') requestId: string) {
   *     return this.orderService.getOrderStatus(requestId); // Returns status + results
   *   }
   * }
   * 
   * 🚀 CLIENT USAGE FLOW:
   * 1. POST /orders → submitOrder() → Get { requestId } immediately (HTTP 202)
   * 2. GET /orders/{requestId}/status → getOrderStatus() → Poll for results
   * 3. When status = 'SUCCESS', final results from two-phase processing are available
   * 
   * 🔄 TWO-PHASE PROCESSING BENEFITS:
   * - ✅ Data Safety: Raw results stored immediately, never lost
   * - ✅ Retry Logic: Can retry mapping without re-calling expensive third-party APIs
   * - ✅ Debugging: Can inspect raw results in database for troubleshooting
   * - ✅ Error Recovery: Failed mapping doesn't lose third-party data
   * - ✅ Format 2: Clear separation between requestId and business data
   */
}

// ============================================================================
// 📝 THAT'S IT! 🎉 - Two-Phase Processing Complete
// ============================================================================

/*
 * 🎯 SUMMARY: What you implemented above:
 * 
 * 1️⃣ EXTENDED the library class: AsyncHttpProcessor<YourRequestType, YourResultType>
 * 2️⃣ ADDED public method submitOrder() with inline validation & enrichment
 * 3️⃣ IMPLEMENTED processBatchItems() - Phase 1: Get raw results from third-party
 * 4️⃣ IMPLEMENTED mapBatchResults() - Phase 2: Map raw results to Format 2 BatchItem[]
 * 5️⃣ IMPLEMENTED getOrderStatus() for client polling (returns final results)
 * 
 * 🔄 WHAT THE LIBRARY HANDLES FOR YOU:
 * - Batching requests together
 * - Two-phase processing orchestration
 * - Raw result storage (data safety)
 * - Final result storage and mapping
 * - Background processing orchestration
 * - Error handling and retries
 * - Status tracking and polling
 * 
 * 📋 WHAT YOUR CONTROLLER LOOKS LIKE:
 * 
 * @Controller('orders')
 * export class OrderController {
 *   constructor(private orderService: OrderProcessingService) {}
 * 
 *   @Post()
 *   async submitOrder(@Body() order: OrderRequest) {
 *     return this.orderService.submitOrder(order); // Returns { requestId }
 *   }
 * 
 *   @Get(':requestId/status')
 *   async getStatus(@Param('requestId') requestId: string) {
 *     return this.orderService.getOrderStatus(requestId); // Returns status + results
 *   }
 * }
 * 
 * 🚀 CLIENT USAGE FLOW:
 * 1. POST /orders → submitOrder() → Get { requestId } immediately (HTTP 202)
 * 2. GET /orders/{requestId}/status → getOrderStatus() → Poll for results
 * 3. When status = 'SUCCESS', final results from two-phase processing are available
 * 
 * 🔄 TWO-PHASE PROCESSING BENEFITS:
 * - ✅ Data Safety: Raw results stored immediately, never lost
 * - ✅ Retry Logic: Can retry mapping without re-calling expensive third-party APIs
 * - ✅ Debugging: Can inspect raw results in database for troubleshooting
 * - ✅ Error Recovery: Failed mapping doesn't lose third-party data
 * - ✅ Format 2: Clear separation between requestId and business data
 */
```

</div>

</div>

</div>

</div>

# Case 2: Immediate Results + Service Callback


![[48719528005-Case 2_Immediate Results _ Service Callback.png]]



<div id="expander-111907903" class="expand-container conf-macro output-block" hasbody="true" macro-id="4afabce9-2044-4b4c-bd38-0d90f9226fd0" macro-name="expand">

<div id="expander-control-111907903" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Code Example</span>

</div>

<div id="expander-content-111907903" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fbb4634f-aeba-4521-84fe-1e5e747b3066" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/**
 * Case 2: Immediate Results + Service Callback - Format 2 + Two-Phase Processing
 * 
 * 🎯 PURPOSE: Show developers exactly how to use the AsyncHttpProcessor library
 * 
 * 📋 WHAT YOU NEED TO IMPLEMENT:
 * 1. Extend AsyncHttpProcessor<YourRequestType, YourResultType>
 * 2. Implement processBatchItems() - Phase 1: Get raw results from third-party
 * 3. Implement mapBatchResults() - Phase 2: Map raw results to BatchItem[]
 * 4. Implement sendIndividualCallback() - for service callbacks
 * 5. Add public methods with inline validation for your controller to call
 * 
 * 🔄 LIBRARY FLOW (You don't implement this - library handles it):
 * Client → Controller → Service.sendNotification() → Library.processAsyncRequest() → HTTP 202
 * Background: Library → Service.processBatchItems() → Library stores raw results → Service.mapBatchResults() → Library stores final results
 * Callbacks: Library → Service.sendIndividualCallback() for each result
 */

import { Injectable, Logger } from '@nestjs/common';
import { AsyncHttpProcessor } from '../../src/processors/http/async-http-processor.service';
import { BatchItem } from '../../src/types/async-batch-processor.types';
import { 
  NotificationRequest, 
  NotificationResult, 
  NotificationStatus
} from './notification.types';

// ============================================================================
// 📝 STEP 1: EXTEND THE LIBRARY CLASS
// ============================================================================

@Injectable()
export class NotificationService extends AsyncHttpProcessor<NotificationRequest, any> {
  private readonly logger = new Logger(NotificationService.name);

  constructor() {
    // 🔧 CONFIGURE THE LIBRARY - Simple inline 3-phase config
    const config = {
      // Global settings used across all phases
      global: {
        // 🗄️ STORAGE CONFIGURATION (EXAMPLE - NOT FINALIZED)
        // NOTE: The final storage implementation will depend on your chosen database/service:
        // - Could be PostgreSQL, MongoDB, Redis, DynamoDB, etc.
        // - Could be a dedicated microservice, cloud service, or embedded database
        // - This example shows the interface structure, not the final implementation
        storage: {
          baseUrl: 'https://your-luz-batching-service.com',  // Your storage service endpoint
          timeoutMs: 30000,                                   // Connection timeout
          retryConfig: {
            enabled: true,
            maxRetries: 3,
            retryDelayMs: 1000
          }
        },
        batchType: 'NOTIFICATION_PROCESSING',
        logLevel: 'info' as const
      },
      
      // Request submission phase (reserved for future features)
      requestSubmission: {},
      
      // Background processing phase
      backgroundProcessing: {
        targetBatchSize: 5,                     // Smaller batches for faster callbacks
        maxBatchWaitMs: 3000,                   // Shorter wait for real-time notifications
        mode: 'IMMEDIATE_RESULTS' as const,     // Process and store results immediately
        processingTimeoutMs: 300000             // 5 minute timeout for processing
      },
      
      // Result return phase  
      resultReturn: {
        method: 'CALLBACK' as const,            // Service sends callbacks
        callback: {
          timeoutMs: 30000,                     // 30 second callback timeout
          retryOnTimeout: true                  // Retry callbacks on timeout
        },
        enableIndividualCallbacks: true         // Enable individual result callbacks
      }
    };
    
    super(config, new Logger(NotificationService.name));
  }

  // ============================================================================
  // 📝 STEP 2: ADD PUBLIC METHODS FOR YOUR CONTROLLER TO CALL
  // ============================================================================

  /**
   * 🟢 STEP 2: PUBLIC API - Submit notification for processing
   * 
   * This is what your controller calls. This method validates and enriches
   * the request data, then submits to the library for async processing.
   * 
   * 💡 VALIDATION: If validation fails, throws HTTP 400 error immediately.
   * If validation passes, returns { requestId } for tracking.
   */
  async sendNotification(notificationRequest: NotificationRequest): Promise<{ requestId: string }> {
    this.logger.log(`📥 Submitting notification request: ${notificationRequest.id}`);
    
    // 🔍 VALIDATE REQUEST (throw HTTP 400 errors for invalid data)
    if (!notificationRequest.userId) {
      throw new Error('User ID is required');
    }
    if (!notificationRequest.type) {
      throw new Error('Notification type is required');
    }
    if (!notificationRequest.message) {
      throw new Error('Message is required');
    }
    if (notificationRequest.message.length > 1000) {
      throw new Error(`Message too long: ${notificationRequest.message.length} > 1000`);
    }
    if (notificationRequest.title.length > 200) {
      throw new Error(`Title too long: ${notificationRequest.title.length} > 200`);
    }
    // Add more validation as needed...
    
    // 🔧 ENRICH REQUEST DATA (prepare for processing)
    const enrichedNotification = {
      notificationId: notificationRequest.id,
      userId: notificationRequest.userId,
      type: notificationRequest.type,
      title: notificationRequest.title,
      message: notificationRequest.message,
      priority: notificationRequest.priority,
      deliveryChannel: notificationRequest.priority === 'URGENT' 
        ? `urgent-${notificationRequest.type.toLowerCase()}` 
        : `standard-${notificationRequest.type.toLowerCase()}`,
      processedAt: new Date().toISOString()
      
      // Add any fields your business logic needs...
    };
    
    // 🔵 CALL LIBRARY: Submit pre-validated data for async processing
    const result = await this.processAsyncRequest(enrichedNotification);
    
    this.logger.log(`✅ Notification submitted with requestId: ${result.requestId}`);
    // 🔔 NEXT STEP: Library will batch requests and call processBatchItems() (Step 3)
    return result; // Returns { requestId: "abc123" } immediately
  }

  // ============================================================================
  // 📝 STEP 3: IMPLEMENT BUSINESS LOGIC (Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 3: Get raw results from third-party
   * 
   * ⚡ WHEN: Called later in background after HTTP 202 response sent
   *         Triggered when batch conditions are met:
   *         - Batch reaches targetBatchSize (5 requests in this config)
   *         - OR maxBatchWaitMs timeout expires (3000ms in this config)
   * 
   * 🎯 PURPOSE: Call third-party API and return raw results
   * 📋 YOUR JOB: Process the actual business logic and return raw results
   * 
   * 💡 NOTE: Items are already validated (inline validation was called first)
   */
  async processBatchItems(items: BatchItem[]): Promise<any> {
    this.logger.log(`🔄 Processing batch of ${items.length} validated notifications`);
    
    // Extract business data from BatchItem[] (Format 2)
    // items = [
    //   { requestId: "req_1", request: { notificationId: "NOTIF_123", userId: "USER_456", type: "EMAIL", ... } },
    //   { requestId: "req_2", request: { notificationId: "NOTIF_789", userId: "USER_012", type: "SMS", ... } }
    // ]
    const businessData = items.map(item => item.request);
    
    // 🏗️ ADD YOUR BUSINESS LOGIC HERE
    // Call third-party notification service
    // Example: Send to notification delivery service
    // const rawResults = await fetch('https://notification-api.com/send', {
    //   method: 'POST',
    //   headers: { 'Content-Type': 'application/json' },
    //   body: JSON.stringify(businessData)
    // });
    
    // 📝 SIMULATE PROCESSING: Replace with your actual logic
    await new Promise(resolve => setTimeout(resolve, 100)); // Simulate API call
    
    // Simulate third-party response (out of order!)
    const rawResults = businessData.map(data => ({
      notification_id: data.notificationId,  // Third-party uses different field name
      status: Math.random() > 0.2 ? 'delivered' : 'failed', // 80% success rate
      delivery_id: `DEL_${Math.random().toString(36).substr(2, 9)}`,
      error: Math.random() > 0.2 ? undefined : 'delivery_failed'
    })).reverse(); // Simulate out-of-order response
    
    this.logger.log(`✅ Step 3 complete: Got raw results from third-party`);
    return rawResults; // Library stores immediately for data safety
  }

  // ============================================================================
  // 📝 STEP 4: IMPLEMENT MAPPING LOGIC (Phase 2 - Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 4: Map raw results to BatchItem[]
   * 
   * ⚡ WHEN: Called by library after raw results are stored
   * 🎯 PURPOSE: Map third-party results back to original requests
   */
  async mapBatchResults(requests: BatchItem[], rawResults: any): Promise<BatchItem[]> {
    this.logger.log(`🔄 Step 4: Mapping ${rawResults.length} raw results`);
    
    // requests = [
    //   { requestId: "req_1", request: { notificationId: "NOTIF_123", userId: "USER_456", type: "EMAIL", ... } },
    //   { requestId: "req_2", request: { notificationId: "NOTIF_789", userId: "USER_012", type: "SMS", ... } }
    // ]
    // rawResults = [
    //   { notification_id: "NOTIF_789", status: "delivered", delivery_id: "DEL_456" },
    //   { notification_id: "NOTIF_123", status: "failed", error: "invalid_email" }
    // ]
    
    // Simply add response to each request by correlating with raw results
    return requests.map(request => {
      const matchingResult = rawResults.find(result => 
        result.notification_id === request.request.notificationId
      );
      
      if (!matchingResult) {
        throw new Error(`No matching result found for notificationId: ${request.request.notificationId}`);
      }
      
      const finalResult: NotificationResult = {
        id: request.request.notificationId,
        status: matchingResult.status === 'delivered' ? 'SUCCESS' : 'ERROR',
        processedAt: new Date(),
        deliveryResult: {
          isDelivered: matchingResult.status === 'delivered',
          deliveryMethod: request.request.type,
          deliveryId: matchingResult.delivery_id,
          deliveredAt: matchingResult.status === 'delivered' ? new Date() : undefined,
          errors: matchingResult.status === 'delivered' ? [] : [matchingResult.error || 'Delivery failed'],
          warnings: [],
          metadata: { rawResult: matchingResult }
        },
        errorDetails: matchingResult.error || undefined
      };
      
      return {
        ...request,           // Keep requestId and request as-is
        response: finalResult // Add the response
      };
    });
  }

  // ============================================================================
  // 📝 STEP 5: SERVICE CALLBACKS - FINAL STEP (Delivers results to external systems)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Send callback for each individual result - FINAL STEP
   * 
   * This is where results are actually delivered to external systems.
   * This is the library's callback mechanism for Case 2.
   * 
   * ⚡ WHEN: Called by library for EACH request/result pair after Step 4 completes
   *         Library maps each result from Step 4 back to its original client request
   *         Then triggers this method once per request with (result + originalRequest)
   * 🎯 PURPOSE: Deliver individual results via HTTP, PubSub, files, etc.
   * 📋 YOUR JOB: Implement your callback delivery logic (HTTP POST, message queue, etc.)
   */
  public async sendIndividualCallback(result: NotificationResult, originalRequest: NotificationRequest): Promise<void> {
    this.logger.log(`📤 Sending callback for notification ${result.id}`);
    
    try {
      // 🔧 ADD YOUR CALLBACK DELIVERY LOGIC HERE
      // This is where you send the result to external systems
      
      // Example: HTTP Callback
      // await fetch('https://your-webhook-endpoint.com/notifications', {
      //   method: 'POST',
      //   headers: { 'Content-Type': 'application/json' },
      //   body: JSON.stringify({
      //     notificationId: result.id,
      //     status: result.status,
      //     processedAt: result.processedAt,
      //     originalRequest: originalRequest
      //   })
      // });
      
      // Example: Message Queue
      // await this.messageQueue.publish('notification-results', {
      //   result,
      //   originalRequest
      // });
      
      // Example: File System
      // await fs.writeFile(`results/${result.id}.json`, JSON.stringify(result));
      
      // 📝 SIMULATE CALLBACK: Replace with your actual callback logic
      await new Promise(resolve => setTimeout(resolve, 50)); // Simulate callback delivery
      
      this.logger.log(`✅ Callback sent successfully for notification ${result.id}`);
      // 🔔 NEXT STEP: Processing complete - result delivered to external system!
      
    } catch (error) {
      this.logger.error(`❌ Failed to send callback for notification ${result.id}:`, error);
      // 💡 NOTE: Don't throw - callback failures shouldn't break the main flow
      // The library will handle retry logic if configured
    }
  }
}
```

</div>

</div>

</div>

</div>

# Case 3: Tracking ID + Service Polling + Client Polling


![[48719528005-Case 3_Tracking ID_Service Polling_Client Polling.png]]



<div id="expander-190243287" class="expand-container conf-macro output-block" hasbody="true" macro-id="c4886177-b5fd-420f-9bf1-6136e1fd3f7a" macro-name="expand">

<div id="expander-control-190243287" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Code Example</span>

</div>

<div id="expander-content-190243287" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8ee32d11-cc15-4ab4-92e7-92e9ed02cde6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/**
 * Case 3: Tracking ID + Service Polling + Client Polling - SIMPLIFIED EXAMPLE
 * 
 * 🎯 PURPOSE: Show developers exactly how to use the AsyncHttpProcessor library
 * 
 * 📋 WHAT YOU NEED TO IMPLEMENT:
 * 1. Extend AsyncHttpProcessor<YourRequestType, YourResultType>
 * 2. Implement processBatchItems() - Phase 1: Get raw results from third-party
 * 3. Implement mapBatchResults() - Phase 2: Map raw results to BatchItem[]
 * 4. Implement implementServiceManagedPolling() - for polling third-party for results
 * 5. Add public methods with inline validation for your controller to call
 * 
 * 📄 DTO REFERENCE: See document-processing.types.ts for DocumentRequest structure
 * 
 * 🔄 LIBRARY FLOW (You don't implement this - library handles it):
 * Client → Controller → Service.processDocument() → Library.processAsyncRequest() → HTTP 202
 * Background: Library → Service.processBatchItems() → Returns trackingId → Library triggers polling
 * Polling: Library → Service.implementServiceManagedPolling() → Get results from third-party
 * Client Polling: Client → Controller → Service.getDocumentStatus() → Library.getRequestStatus()
 */

import { Injectable, Logger } from '@nestjs/common';
import { AsyncHttpProcessor } from '../../src/processors/http/async-http-processor.service';
import { BatchItem } from '../../src/types/async-batch-processor.types';
import { 
  DocumentRequest, 
  DocumentResult, 
  DocumentStatus
} from './document-processing.types';
// ============================================================================
// 📝 STEP 1: EXTEND THE LIBRARY CLASS
// ============================================================================

@Injectable()
export class DocumentProcessingService extends AsyncHttpProcessor<DocumentRequest, any> {
  private readonly logger = new Logger(DocumentProcessingService.name);

  constructor() {
    // 🔧 CONFIGURE THE LIBRARY - Simple inline 3-phase config
    const config = {
      // Global settings used across all phases
      global: {
        // 🗄️ STORAGE CONFIGURATION (EXAMPLE - NOT FINALIZED)
        // NOTE: The final storage implementation will depend on your chosen database/service:
        // - Could be PostgreSQL, MongoDB, Redis, DynamoDB, etc.
        // - Could be a dedicated microservice, cloud service, or embedded database
        // - This example shows the interface structure, not the final implementation
        storage: {
          baseUrl: 'https://your-luz-batching-service.com',  // Your storage service endpoint
          timeoutMs: 45000,                                   // Longer timeout for third-party operations
          retryConfig: {
            enabled: true,
            maxRetries: 5,                                    // More retries for third-party reliability
            retryDelayMs: 2000
          }
        },
        batchType: 'DOCUMENT_PROCESSING',
        logLevel: 'debug' as const              // More verbose logging for third-party interactions
      },
      
      // Request submission phase (reserved for future features)
      requestSubmission: {},
      
      // Background processing phase
      backgroundProcessing: {
        targetBatchSize: 8,                     // Batch size optimized for third-party API limits
        maxBatchWaitMs: 4000,                   // Wait time before processing smaller batch
        mode: 'CALLBACK_RESULTS' as const,     // Results come from third-party via polling
        processingTimeoutMs: 600000             // 10 minute timeout for third-party processing
      },
      
      // Result return phase  
      resultReturn: {
        method: 'POLLING' as const,             // Client polls for results
        polling: {
          enableStatusEndpoint: false           // Service manages polling, not client
        }
      }
    };
    
    super(config, new Logger(DocumentProcessingService.name));
  }

  // ============================================================================
  // 📝 STEP 2: ADD PUBLIC METHODS FOR YOUR CONTROLLER TO CALL
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Submit document for processing
   * 
   * This is what your controller calls. This method validates and enriches
   * the request data, then submits to the library for async processing.
   * 
   * 💡 VALIDATION: If validation fails, throws HTTP 400 error immediately.
   * If validation passes, returns { requestId } for client polling.
   */
  async processDocument(documentRequest: DocumentRequest): Promise<{ requestId: string }> {
    this.logger.log(`📥 Submitting document request: ${documentRequest.id}`);
    
    // 🔍 STEP 2.1: VALIDATE REQUEST (throw HTTP 400 errors for invalid data)
    if (!documentRequest.documentId) {
      throw new Error('Document ID is required');
    }
    if (!documentRequest.documentType) {
      throw new Error('Document type is required');
    }
    if (!documentRequest.content && !documentRequest.fileUrl) {
      throw new Error('Either content or fileUrl is required');
    }
    // Add more validation as needed...
    
    // 🔧 STEP 2.2: ENRICH REQUEST DATA (prepare for processing)
    const enrichedDocument = {
      // Include original data
      ...documentRequest,
      
      // Add computed/normalized fields (customize for your needs)
      normalizedDocumentType: documentRequest.documentType.toUpperCase(),
      processingPriority: documentRequest.urgent ? 'HIGH' : 'NORMAL',
      submittedAt: new Date().toISOString(),
      
      // Add any fields your business logic needs...
    };
    
    // 🔵 CALL LIBRARY: Submit pre-validated data for async processing
    const result = await this.processAsyncRequest(enrichedDocument);
    
    this.logger.log(`✅ Document submitted with requestId: ${result.requestId}`);
    // 🔔 NEXT STEP: Library will batch requests and call processBatchItems() (Step 3)
    return result; // Returns { requestId: "abc123" } immediately
  }


  // ============================================================================
  // 📝 STEP 3: IMPLEMENT BUSINESS LOGIC (Called during background processing)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Process a batch of validated requests in the background
   * 
   * ⚡ WHEN: Called later in background after HTTP 202 response sent
   *         Triggered when batch conditions are met:
   *         - Batch reaches targetBatchSize (8 requests in this config)
   *         - OR maxBatchWaitMs timeout expires (4000ms in this config)
   *         - OR library shutdown/flush is triggered
   * 
   * 🎯 PURPOSE: Call third-party API and return raw results
   * 📋 YOUR JOB: Process the actual business logic and return raw results
   * 
   * 💡 NOTE: Items are already validated (inline validation was called first)
   */
  async processBatchItems(items: BatchItem[]): Promise<any> {
    this.logger.log(`🔄 Processing batch of ${items.length} validated documents`);
    
    // Extract business data from BatchItem[] (Format 2)
    // items = [
    //   { requestId: "req_1", request: { documentId: "DOC_123", content: "...", type: "PDF", ... } },
    //   { requestId: "req_2", request: { documentId: "DOC_456", content: "...", type: "DOCX", ... } }
    // ]
    const businessData = items.map(item => item.request);
    
    // 🏗️ ADD YOUR BUSINESS LOGIC HERE
    // Submit documents to third-party service
    // Example: Submit batch for document analysis
    // const trackingId = await this.documentAnalysisAPI.submitBatch({
    //   documents: businessData.map(data => ({
    //     documentId: data.documentId,
    //     content: data.content,
    //     type: data.type,
    //     analysisProfile: data.analysisProfile
    //   }))
    // });
    
    // 📝 SIMULATE THIRD-PARTY SUBMISSION: Replace with your actual API call
    const trackingId = `track_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
    await new Promise(resolve => setTimeout(resolve, 200)); // Simulate API call
    
    this.logger.log(`✅ Step 3 complete: Got tracking ID from third-party: ${trackingId}`);
    
    // Return tracking ID as raw result - library stores immediately for data safety
    // 🔔 NEXT STEP: Library triggers polling via implementServiceManagedPolling(), then calls mapBatchResults() (Step 4)
    return { trackingId };
  }


  // ============================================================================
  // 📝 STEP 4.5: IMPLEMENT POLLING LOGIC (Called by library after Step 4)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Poll third-party service for results using trackingId
   * 
   * ⚡ WHEN: Called by library after Step 4 returns { trackingId, requiresPolling: true }
   *         Library automatically triggers this based on your Step 4 response
   * 
   * 🎯 PURPOSE: Poll third-party service until results are ready
   * 📋 YOUR JOB: Implement polling logic and return final results when ready
   * 
   * 💡 NOTE: This is unique to Cases 3, 4 (service-managed polling patterns)
   * 
   * ⚡ THREADING: This method runs in a BACKGROUND THREAD - it does NOT block the main 
   * application thread. New client requests can still be processed while this polling 
   * operation runs. The library orchestrates this asynchronously.
   * 
   * 🏗️ ARCHITECTURAL CONSIDERATIONS:
   * 
   * 1. POLLING LIBRARY: The polling logic below (exponential backoff, retry logic, 
   *    timeout handling) could be abstracted into a separate polling library, but NOT this 
   *    batching library. This batching library focuses on request batching, while polling 
   *    patterns could be handled by a dedicated polling/retry library.
   * 
   * 2. PUSH vs PULL PATTERN: Current design uses PULL (library waits for results).
   *    A PUSH pattern would be more scalable:
   *    - Service starts polling in background (non-blocking)
   *    - Service calls library.submitPollingResults() when ready
   *    - Better resource usage (no waiting threads)
   *    - More complex error handling and coordination
   *    This could be a future improvement for high-scale scenarios.
   */
  public async implementServiceManagedPolling(
    batchId: string, 
    trackingId: string, 
    requestIds: string[]
  ): Promise<BulkProcessResult> {
    this.logger.log(`🔄 Starting polling for batch ${batchId} with trackingId: ${trackingId}`);
    
    // 🔧 CONFIGURE POLLING: Customize these for your third-party service
    const maxAttempts = 20;           // Max polling attempts
    const intervalMs = 5000;          // Poll every 5 seconds
    const backoffMultiplier = 1.2;    // Increase interval by 20% each attempt
    const maxBackoffMs = 30000;       // Max 30 seconds between polls
    
    let attempts = 0;
    let currentInterval = intervalMs;
    
    while (attempts < maxAttempts) {
      try {
        this.logger.debug(`📡 Polling attempt ${attempts + 1}/${maxAttempts} for trackingId: ${trackingId}`);
        
        // 🏗️ ADD YOUR POLLING LOGIC HERE
        // Poll your third-party document analysis service
        
        // Example: Poll third-party API
        // const pollingResult = await this.documentAnalysisAPI.getStatus(trackingId);
        
        // Example: Check if results are ready
        // if (pollingResult.status === 'COMPLETED') {
        //   // 💡 MULTIPLE RESULTS: Third-party typically returns results for entire batch
        //   const results: DocumentResult[] = pollingResult.documents.map(doc => ({
        //     id: doc.documentId, // 🔑 CRITICAL: Business ID from Step 4 - library uses this for mapping!
        //     status: doc.success ? 'SUCCESS' : 'ERROR',
        //     processedAt: new Date(),
        //     processingResult: {
        //       isProcessed: doc.success,
        //       metadata: doc.metadata || {},
        //       processingTime: doc.processingTimeMs || 0
        //     },
        //     analysisResults: doc.results || {},
        //     errorDetails: doc.error || undefined
        //   }));
        //   
        //   return { results }; // Library finds requestId that corresponds to each result.id
        // }
        
        // 📝 PLACEHOLDER: Replace with your actual polling implementation
        this.logger.debug(`📡 Polling third-party service with trackingId: ${trackingId}`);
        
        // Simulate completion after a few attempts for demo purposes
        if (attempts >= 2) {
          // 🔧 EXAMPLE RESULTS: Replace with your actual result processing
          // 
          // 🔑 CRITICAL MAPPING: Each result.id MUST match the business ID from Step 4
          // This mapping requirement can be standardized by the library in the future
          // 
          // 💡 MULTIPLE RESULTS: Step 4.5 typically returns results for ALL documents in the batch
          const results: DocumentResult[] = [
            {
              id: 'doc_123', // ⚠️ Business ID from Step 4 batch submission
              status: 'SUCCESS',
              processedAt: new Date(),
              processingResult: {
                isProcessed: true,
                metadata: { pages: 5, wordCount: 1234 },
                processingTime: 15000
              },
              analysisResults: { extractedText: 'Sample text...', confidence: 0.95 }
            },
            {
              id: 'doc_456', // ⚠️ Another business ID from Step 4 batch submission
              status: 'ERROR',
              processedAt: new Date(),
              processingResult: {
                isProcessed: false,
                metadata: {},
                processingTime: 0
              },
              errorDetails: 'Document format not supported'
            }
            // ... more results for other documents in the batch
          ];
          
          this.logger.log(`✅ Polling completed for batch ${batchId}`);
          
          // 📤 RETURN TO LIBRARY: Final results for storage and client polling
          // 
          // 🔄 NON-BLOCKING: This polling runs in a separate background thread/process.
          // The main application thread is NOT blocked - clients can still submit new requests.
          // Only this specific polling operation completes and returns results to the library.
          // 
          // 🗂️ LIBRARY MAPPING: Library uses result.id (business ID) to find the corresponding requestId.
          // This mapping logic can be improved later for better performance and reliability.
          // 🔔 NEXT STEP: Library will store results for client polling via getDocumentStatus() (Step 5)
          return { results };
        }
        
        // ⏳ RESULTS NOT READY: Wait and try again
        this.logger.debug(`⏳ Results not ready, waiting ${currentInterval}ms before next poll...`);
        await new Promise(resolve => setTimeout(resolve, currentInterval));
        
        // 📈 EXPONENTIAL BACKOFF: Gradually increase polling interval
        currentInterval = Math.min(currentInterval * backoffMultiplier, maxBackoffMs);
        attempts++;
        
      } catch (error) {
        this.logger.error(`❌ Polling attempt ${attempts + 1} failed:`, error);
        
        // Wait before retry
        await new Promise(resolve => setTimeout(resolve, currentInterval));
        currentInterval = Math.min(currentInterval * backoffMultiplier, maxBackoffMs);
        attempts++;
      }
    }
    
    // ⏰ POLLING TIMEOUT: Create error results for all requests
    this.logger.error(`⏰ Polling timeout for batch ${batchId} after ${attempts} attempts`);
    const errorResults: DocumentResult[] = requestIds.map(requestId => ({
      id: `unknown_${requestId}`, // We don't have the business ID here
      status: 'ERROR',
      processedAt: new Date(),
      processingResult: {
        isProcessed: false,
        metadata: {},
        processingTime: 0
      },
      errorDetails: 'Polling timeout - third-party service did not respond in time'
    }));
    
    return { results: errorResults };
  }

  // ============================================================================
  // 📝 STEP 4: IMPLEMENT MAPPING LOGIC (Phase 2 - Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 4: Map raw results to BatchItem[]
   * 
   * ⚡ WHEN: Called by library after raw results are stored
   * 🎯 PURPOSE: Map third-party results back to original requests
   */
  async mapBatchResults(requests: BatchItem[], rawResults: any): Promise<BatchItem[]> {
    this.logger.log(`🔄 Step 4: Mapping raw results with trackingId: ${rawResults.trackingId}`);
    
    // For Case 3, we need to trigger polling to get the actual results
    // The raw result contains the trackingId, now we need to poll for final results
    const trackingId = rawResults.trackingId;
    
    // Poll third-party service for final results
    const pollingResults = await this.implementServiceManagedPolling('batch_id', trackingId, requests.map(r => r.requestId));
    
    // Map polling results back to BatchItem[] format
    return requests.map(request => {
      const matchingResult = pollingResults.results.find(result => 
        result.documentId === request.request.documentId
      );
      
      if (!matchingResult) {
        throw new Error(`No matching result found for documentId: ${request.request.documentId}`);
      }
      
      return {
        ...request,           // Keep requestId and request as-is
        response: matchingResult // Add the response
      };
    });
  }

  // ============================================================================
  // 📝 STEP 5: CLIENT POLLING - FINAL STEP (Returns results to client)
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Get document processing status - FINAL STEP
   * 
   * This is where clients actually retrieve the results from Step 4.5 (polling).
   * This is what your controller calls for client polling.
   * 
   * 💡 RETURNS RESULTS FROM STEP 4.5: The results retrieved from third-party polling
   */
  async getDocumentStatus(requestId: string): Promise<DocumentStatus> {
    this.logger.debug(`📊 Getting status for request: ${requestId}`);
    
    // 🔵 CALL LIBRARY: This retrieves results stored from Step 4.5 (polling)
    const status = await this.getRequestStatus(requestId);
    
    if (!status) {
      throw new Error(`Request ${requestId} not found`);
    }

    // 🔄 TRANSFORM: Convert library format to your API format
    // The 'result' field contains what you processed in Step 4.5 (implementServiceManagedPolling)
    // 🔔 NEXT STEP: Return final results to client - processing complete!
    return {
      requestId,
      status: status.status as 'PENDING' | 'SUCCESS' | 'ERROR',
      result: status.result as DocumentResult, // This is your result from Step 4.5 polling
      errorDetails: status.errorDetails,
      createdAt: status.createdAt,
      updatedAt: status.updatedAt
    };
  }

  // ============================================================================
  // 📝 THAT'S IT! 🎉
  // ============================================================================
  
  /*
   * 🎯 SUMMARY: What you implemented above:
   * 
   * 1️⃣ EXTENDED the library class: AsyncHttpProcessor<YourRequestType, YourResultType>
   * 2️⃣ ADDED public methods for your controller to call (processDocument, getDocumentStatus)
   * 3️⃣ IMPLEMENTED inline validation and data preparation in public methods
   * 4️⃣ IMPLEMENTED processBatchItems() for third-party submission (returns trackingId)
   * 4️⃣.5 IMPLEMENTED implementServiceManagedPolling() for polling third-party results
   * 5️⃣ IMPLEMENTED getDocumentStatus() for client polling (returns final results)
   * 
   * 🔄 WHAT THE LIBRARY HANDLES FOR YOU:
   * - Batching requests together
   * - Storing requests and results
   * - Background processing orchestration
   * - Polling orchestration and scheduling
   * - Error handling and retries
   * - Status tracking and client polling
   * 
   * 📋 WHAT YOUR CONTROLLER LOOKS LIKE:
   * 
   * @Controller('documents')
   * export class DocumentController {
   *   constructor(private documentService: DocumentProcessingService) {}
   * 
   *   @Post()
   *   async processDocument(@Body() document: DocumentRequest) {
   *     return this.documentService.processDocument(document); // Returns { requestId }
   *   }
   * 
   *   @Get(':requestId/status')
   *   async getStatus(@Param('requestId') requestId: string) {
   *     return this.documentService.getDocumentStatus(requestId); // Returns status + results
   *   }
   * }
   * 
   * 🚀 CLIENT USAGE FLOW:
   * 1. POST /documents → processDocument() → Get { requestId } immediately (HTTP 202)
   * 2. Library processes in background → Submit to third-party → Get trackingId
   * 3. Library polls third-party → Get final results → Store them
   * 4. GET /documents/{requestId}/status → getDocumentStatus() → Poll for results
   * 5. When status = 'SUCCESS', results from third-party polling are available
   * 
   * 🔑 CASE 3 KEY FEATURES:
   * - Third-party submission with tracking ID
   * - Service-managed polling (you implement the polling logic)
   * - Client polling for final results
   * - Document-specific business logic and data transformation
   */
}
```

</div>

</div>

</div>

</div>

# Case 4: Tracking ID + Service Polling + Service Callback


![[48719528005-Case 4_Tracking ID_Service Polling_Service Callback.png]]



<div id="expander-1483415969" class="expand-container conf-macro output-block" hasbody="true" macro-id="56437c91-07f2-4128-802f-4006fbfa634c" macro-name="expand">

<div id="expander-control-1483415969" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Code Example</span>

</div>

<div id="expander-content-1483415969" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a79b5099-57ae-4411-9d2f-f278b17cb846" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/**
 * Case 4: Tracking ID + Service Polling + Service Callbacks - SIMPLIFIED EXAMPLE
 * 
 * 🎯 PURPOSE: Show developers exactly how to use the AsyncHttpProcessor library
 * 
 * 📋 WHAT YOU NEED TO IMPLEMENT:
 * 1. Extend AsyncHttpProcessor<YourRequestType, YourResultType>
 * 2. Implement processBatchItems() - Step 3: Get raw results from third-party
 * 3. Implement mapBatchResults() - Step 4: Map raw results to BatchItem[]
 * 4. Implement implementServiceManagedPolling() - for polling third-party for results
 * 5. Implement sendIndividualCallback() - for service callbacks after polling
 * 6. Add public methods with inline validation for your controller to call
 * 
 * 📄 DTO REFERENCE: See document-processing.types.ts for DocumentRequest structure
 * 
 * 🔄 LIBRARY FLOW (You don't implement this - library handles it):
 * Client → Controller → Service.processDocument() → Library.processAsyncRequest() → HTTP 202
 * Background: Library → Service.processBatchItems() → Returns trackingId → Library triggers polling
 * Polling: Library → Service.implementServiceManagedPolling() → Get results from third-party
 * Callbacks: Library → Service.sendIndividualCallback() for each result
 */

import { Injectable, Logger } from '@nestjs/common';
import { AsyncHttpProcessor } from '../../src/processors/http/async-http-processor.service';
import { BulkProcessResult, BatchItem } from '../../src/types/async-batch-processor.types';
import { 
  DocumentRequest, 
  DocumentResult, 
  DocumentStatus
} from './document-processing.types';

// ============================================================================
// 📝 STEP 1: EXTEND THE LIBRARY CLASS
// ============================================================================

@Injectable()
export class DocumentProcessingService extends AsyncHttpProcessor<DocumentRequest, any> {
  private readonly logger = new Logger(DocumentProcessingService.name);

  constructor() {
    // 🔧 CONFIGURE THE LIBRARY - Simple inline 3-phase config
    const config = {
      // Global settings used across all phases
      global: {
        // 🗄️ STORAGE CONFIGURATION (EXAMPLE - NOT FINALIZED)
        // NOTE: The final storage implementation will depend on your chosen database/service:
        // - Could be PostgreSQL, MongoDB, Redis, DynamoDB, etc.
        // - Could be a dedicated microservice, cloud service, or embedded database
        // - This example shows the interface structure, not the final implementation
        storage: {
          baseUrl: 'https://your-luz-batching-service.com',  // Your storage service endpoint
          timeoutMs: 30000,                                   // Connection timeout
          retryConfig: {
            enabled: true,
            maxRetries: 3,
            retryDelayMs: 1500
          }
        },
        batchType: 'DOCUMENT_PROCESSING',
        logLevel: 'info' as const
      },
      
      // Request submission phase (reserved for future features)
      requestSubmission: {},
      
      // Background processing phase
      backgroundProcessing: {
        targetBatchSize: 6,                     // Batch size for webhook processing
        maxBatchWaitMs: 3500,                   // Wait time optimized for webhook scenarios
        mode: 'CALLBACK_RESULTS' as const,     // Results come from third-party via webhooks
        processingTimeoutMs: 900000             // 15 minute timeout for webhook processing
      },
      
      // Result return phase  
      resultReturn: {
        method: 'CALLBACK' as const,            // Service sends callbacks
        callback: {
          timeoutMs: 60000,                     // Longer timeout for webhook processing
          retryOnTimeout: false                 // Don't retry webhooks automatically
        },
        enableIndividualCallbacks: true         // Enable individual result callbacks
      }
    };
    
    super(config, new Logger(DocumentProcessingService.name));
  }

  // ============================================================================
  // 📝 STEP 2: ADD PUBLIC METHODS FOR YOUR CONTROLLER TO CALL
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Submit document for processing
   * 
   * This is what your controller calls. The library handles the rest.
   * 
   * 💡 RESULT DEPENDS ON INLINE VALIDATION: If validation fails, this throws error.
   * If validation passes, returns { requestId } for tracking.
   */
  async processDocument(documentRequest: DocumentRequest): Promise<{ requestId: string }> {
    this.logger.log(`📥 Submitting document request: ${documentRequest.id}`);
    
    // 🔍 STEP 2.1: VALIDATE REQUEST (throw HTTP 400 errors for invalid data)
    if (!documentRequest.documentId) {
      throw new Error('Document ID is required');
    }
    if (!documentRequest.documentType) {
      throw new Error('Document type is required');
    }
    if (!documentRequest.content && !documentRequest.fileUrl) {
      throw new Error('Either content or fileUrl is required');
    }
    // Add more validation as needed...
    
    // 🔧 STEP 2.2: ENRICH REQUEST DATA (prepare for processing)
    const enrichedDocument = {
      // Include original data
      ...documentRequest,
      
      // Add computed/normalized fields (customize for your needs)
      normalizedDocumentType: documentRequest.documentType.toUpperCase(),
      processingPriority: documentRequest.urgent ? 'HIGH' : 'NORMAL',
      submittedAt: new Date().toISOString(),
      
      // Case 4 specific: Add callback configuration
      callbackConfig: {
        enableCallbacks: true,
        callbackUrl: process.env.CALLBACK_WEBHOOK_URL || 'https://your-app.com/api/callbacks',
        retryAttempts: 3
      },
      
      // Add any fields your business logic needs...
    };
    
    // 🔵 CALL LIBRARY: Submit pre-validated data for async processing
    const result = await this.processAsyncRequest(enrichedDocument);
    
    this.logger.log(`✅ Document submitted with requestId: ${result.requestId}`);
    // 🔔 NEXT STEP: Library will batch requests and call processBatchItems() (Step 3)
    return result; // Returns { requestId: "abc123" } immediately
  }


  // 💡 NOTE: No status polling method needed for Case 4
  // Results are automatically delivered via sendIndividualCallback() (Step 5.5)

  // ============================================================================
  // 📝 STEP 3: IMPLEMENT BUSINESS LOGIC (Called during background processing)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Process a batch of validated requests in the background
    const request = parsedItem.sourceData;
    
    try {
      // 🔍 STEP 3.1: ADD YOUR VALIDATION LOGIC HERE
      
      // 🔧 STEP 3.2: BUILD PROCESSED DATA (this goes to processBatchItems)
      // This is FILE TRANSFORMATION-SPECIFIC data - completely different from other cases
      // 
      // 💡 NOTE: This is just an EXAMPLE of file transformation business logic!
      // Each service/domain will have totally different processedData structures
      parsedItem.processedData = {
        // Include original data
        ...request,
        
        // FILE TRANSFORMATION: Simple transformation settings
        targetFormat: request.outputFormat || 'PDF',
        compressionMode: request.fileSize > 1000000 ? 'compress' : 'preserve',
        
        // FILE TRANSFORMATION: Output specifications  
        outputSpecs: {
          maxFileSize: 50000000, // 50MB limit
          quality: request.highQuality ? 95 : 75,
          addTimestamp: true
        },
        
        // FILE TRANSFORMATION: Callback delivery settings
        deliveryMethod: request.callbackUrl ? 'webhook' : 'email',
        webhookUrl: request.callbackUrl,
        
        // FILE TRANSFORMATION: Simple batch tracking
        transformationId: `transform_${Date.now()}_${Math.random().toString(36).substr(2, 5)}`,
        estimatedTime: request.fileSize > 5000000 ? '10-15 minutes' : '2-5 minutes'
      };
      
      // ✅ VALIDATION PASSED: Set to true so library knows request is valid
      parsedItem.isValid = true;
      // 🔔 NEXT STEP: Library will batch requests and call processBatchItems() (Step 4)
      
    } catch (error) {
      // ❌ VALIDATION FAILED: Set to false so library rejects request with HTTP 400
      parsedItem.isValid = false;
      parsedItem.error = error.message;
      // 🔔 NEXT STEP: Library will reject request with HTTP 400 error
    }
  }

  // ============================================================================
  // 📝 STEP 3: IMPLEMENT BUSINESS LOGIC (Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 3: Process a batch of validated requests in the background (Phase 1)
   * 
   * ⚡ WHEN: Called later in background after HTTP 202 response sent
   * 🎯 PURPOSE: Submit documents to third-party service and get tracking ID
   * 📋 YOUR JOB: Send documents to third-party and return raw results (tracking ID)
   */
  async processBatchItems(items: BatchItem[]): Promise<any> {
    this.logger.log(`🔄 Processing batch of ${items.length} validated documents`);
    
    // 💡 NOTE: items contains the validated/enriched data from processDocument()
    //          This is the data you prepared inline in processDocument() (Step 2.1 validation + Step 2.2 enrichment)
    //
    // 🔑 IMPORTANT: item.request.id = Business ID (documentId, etc.)
    //              requestId from Step 2 = Library tracking ID (different thing!)
    //
    // 🎯 PROCESSING OPTIONS:
    // Option A: Bulk API - Send all business data at once to third-party
    //   const allBusinessData = items.map(item => item.request);
    //   const trackingId = await this.documentConversionAPI.submitBulk(allBusinessData);
    //
    // Option B: Individual submission - Submit each document separately
    //   Use this when third-party doesn't support bulk operations
    
    try {
      // 🏗️ ADD YOUR BUSINESS LOGIC HERE
      // Submit documents to third-party service using business data from Step 2
      
      // Extract business data from BatchItem[] (Format 2)
      // items = [
      //   { requestId: "req_1", request: { id: "DOC_123", content: "...", fileType: "PDF", ... } },
      //   { requestId: "req_2", request: { id: "DOC_456", content: "...", fileType: "DOCX", ... } }
      // ]
      const businessData = items.map(item => item.request);
      
      // EXAMPLE: Prepare batch for third-party file transformation service
      const batchSubmission = {
        batchId: `transform_batch_${Date.now()}`,
        files: businessData.map(data => ({
          fileId: data.id, // Business ID from client request (NOT the requestId from Step 2)
          targetFormat: data.targetFormat || 'PDF',
          compressionMode: data.fileSize > 1000000 ? 'compress' : 'preserve',
          outputSpecs: {
            maxFileSize: 50000000, // 50MB limit
            quality: data.highQuality ? 95 : 75,
            addTimestamp: true
          },
          deliveryMethod: data.callbackUrl ? 'webhook' : 'email',
          webhookUrl: data.callbackUrl,
          content: data.content
        })),
        batchInfo: {
          transformationId: `transform_${Date.now()}_${Math.random().toString(36).substr(2, 5)}`,
          estimatedTime: businessData.some(d => d.fileSize > 5000000) ? '10-15 minutes' : '2-5 minutes'
        }
      };
      
      // 📝 SIMULATE THIRD-PARTY SUBMISSION: Replace with your actual API call
      // const trackingId = await this.fileTransformationAPI.submitBatch(batchSubmission);
      const trackingId = `transform_track_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
      await new Promise(resolve => setTimeout(resolve, 150)); // Simulate API call
      
      this.logger.log(`✅ Step 3 complete: Got tracking ID from third-party: ${trackingId}`);
      
      // Return tracking ID as raw result - library stores immediately for data safety
      // 🔔 NEXT STEP: Library triggers polling via implementServiceManagedPolling(), then calls mapBatchResults() (Step 4)
      return { trackingId };
      
    } catch (error) {
      this.logger.error(`❌ Failed to submit batch to third-party:`, error);
      throw error; // Let library handle the error
    }
  }

  // ============================================================================
  // 📝 STEP 4.5: IMPLEMENT POLLING LOGIC (Called by library after Step 4)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Poll third-party service for results using trackingId
   * 
   * ⚡ WHEN: Called by library after Step 4 returns { trackingId, requiresPolling: true }
   *         Library automatically triggers this based on your Step 4 response
   * 
   * 🎯 PURPOSE: Poll third-party service until results are ready
   * 📋 YOUR JOB: Implement polling logic and return final results when ready
   * 
   * 💡 NOTE: This is unique to Cases 3, 4 (service-managed polling patterns)
   * 
   * ⚡ THREADING: This method runs in a BACKGROUND THREAD - it does NOT block the main 
   * application thread. New client requests can still be processed while this polling 
   * operation runs. The library orchestrates this asynchronously.
   * 
   * 🏗️ ARCHITECTURAL CONSIDERATIONS:
   * 
   * 1. POLLING LIBRARY: The polling logic below (exponential backoff, retry logic, 
   *    timeout handling) could be abstracted into a separate polling library, but NOT this 
   *    batching library. This batching library focuses on request batching, while polling 
   *    patterns could be handled by a dedicated polling/retry library.
   * 
   * 2. PUSH vs PULL PATTERN: Current design uses PULL (library waits for results).
   *    A PUSH pattern would be more scalable:
   *    - Service starts polling in background (non-blocking)
   *    - Service calls library.submitPollingResults() when ready
   *    - Better resource usage (no waiting threads)
   *    - More complex error handling and coordination
   *    This could be a future improvement for high-scale scenarios.
   */
  public async implementServiceManagedPolling(
    batchId: string, 
    trackingId: string, 
    requestIds: string[]
  ): Promise<BulkProcessResult> {
    this.logger.log(`🔄 Starting polling for batch ${batchId} with trackingId: ${trackingId}`);
    
    // 🔧 CONFIGURE POLLING: Customize these for your third-party service
    const maxAttempts = 15;           // Max polling attempts
    const intervalMs = 4000;          // Poll every 4 seconds
    const backoffMultiplier = 1.3;    // Increase interval by 30% each attempt
    const maxBackoffMs = 25000;       // Max 25 seconds between polls
    
    let attempts = 0;
    let currentInterval = intervalMs;
    
    while (attempts < maxAttempts) {
      try {
        this.logger.debug(`📡 Polling attempt ${attempts + 1}/${maxAttempts} for trackingId: ${trackingId}`);
        
        // 🏗️ ADD YOUR POLLING LOGIC HERE
        // Poll your third-party document conversion service
        
        // Example: Poll third-party API
        // const pollingResult = await this.documentConversionAPI.getStatus(trackingId);
        
        // Example: Check if results are ready
        // if (pollingResult.status === 'COMPLETED') {
        //   // 💡 MULTIPLE RESULTS: Third-party typically returns results for entire batch
        //   const results: DocumentResult[] = pollingResult.documents.map(doc => ({
        //     id: doc.documentId, // 🔑 CRITICAL: Business ID from Step 4 - library uses this for mapping!
        //     status: doc.success ? 'SUCCESS' : 'ERROR',
        //     processedAt: new Date(),
        //     conversionResult: {
        //       outputUrl: doc.downloadUrl,
        //       outputFormat: doc.format,
        //       fileSize: doc.size,
        //       processingTime: doc.durationMs
        //     },
        //     errorDetails: doc.error || undefined
        //   }));
        //   
        //   return { results }; // Library triggers sendIndividualCallback() for each result
        // }
        
        // 📝 PLACEHOLDER: Replace with your actual polling implementation
        this.logger.debug(`📡 Polling third-party conversion service with trackingId: ${trackingId}`);
        
        // Simulate completion after a few attempts for demo purposes
        if (attempts >= 2) {
          // 🔧 EXAMPLE RESULTS: Replace with your actual result processing
          // 
          // 🔑 CRITICAL MAPPING: Each result.id MUST match the business ID from Step 4
          // This mapping requirement can be standardized by the library in the future
          // 
          // 💡 MULTIPLE RESULTS: Step 4.5 typically returns results for ALL documents in the batch
          const results: DocumentResult[] = [
            {
              id: 'doc_123', // ⚠️ Business ID from Step 4 batch submission
              status: 'SUCCESS',
              processedAt: new Date(),
              conversionResult: {
                outputUrl: 'https://storage.example.com/converted/doc_123.pdf',
                outputFormat: 'PDF',
                fileSize: 2048576,
                processingTime: 12000
              }
            },
            {
              id: 'doc_456', // ⚠️ Another business ID from Step 4 batch submission
              status: 'ERROR',
              processedAt: new Date(),
              conversionResult: {
                outputUrl: null,
                outputFormat: null,
                fileSize: 0,
                processingTime: 0
              },
              errorDetails: 'Unsupported file format for conversion'
            }
            // ... more results for other documents in the batch
          ];
          
          this.logger.log(`✅ Polling completed for batch ${batchId}`);
          
          // 📤 RETURN TO LIBRARY: Final results for storage and service callbacks
          // 
          // 🔄 NON-BLOCKING: This polling runs in a separate background thread/process.
          // The main application thread is NOT blocked - clients can still submit new requests.
          // Only this specific polling operation completes and returns results to the library.
          // 
          // 🗂️ LIBRARY MAPPING: Library uses result.id (business ID) to find the corresponding requestId.
          // This mapping logic can be improved later for better performance and reliability.
          // 
          // 🔔 NEXT STEP: Library will trigger sendIndividualCallback() for each result
          return { results };
        }
        
        // ⏳ RESULTS NOT READY: Wait and try again
        this.logger.debug(`⏳ Results not ready, waiting ${currentInterval}ms before next poll...`);
        await new Promise(resolve => setTimeout(resolve, currentInterval));
        
        // 📈 EXPONENTIAL BACKOFF: Gradually increase polling interval
        currentInterval = Math.min(currentInterval * backoffMultiplier, maxBackoffMs);
        attempts++;
        
      } catch (error) {
        this.logger.error(`❌ Polling attempt ${attempts + 1} failed:`, error);
        
        // Wait before retry
        await new Promise(resolve => setTimeout(resolve, currentInterval));
        currentInterval = Math.min(currentInterval * backoffMultiplier, maxBackoffMs);
        attempts++;
      }
    }
    
    // ⏰ POLLING TIMEOUT: Create error results for all requests
    this.logger.error(`⏰ Polling timeout for batch ${batchId} after ${attempts} attempts`);
    const errorResults: DocumentResult[] = requestIds.map(requestId => ({
      id: `unknown_${requestId}`, // We don't have the business ID here
      status: 'ERROR',
      processedAt: new Date(),
      conversionResult: {
        outputUrl: null,
        outputFormat: null,
        fileSize: 0,
        processingTime: 0
      },
      errorDetails: 'Polling timeout - third-party service did not respond in time'
    }));
    
    return { results: errorResults };
  }

  // ============================================================================
  // 📝 STEP 4: IMPLEMENT MAPPING LOGIC (Phase 2 - Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 4: Map raw results to BatchItem[]
   * 
   * ⚡ WHEN: Called by library after raw results are stored
   * 🎯 PURPOSE: Map third-party results back to original requests
   */
  async mapBatchResults(requests: BatchItem[], rawResults: any): Promise<BatchItem[]> {
    this.logger.log(`🔄 Step 4: Mapping raw results with trackingId: ${rawResults.trackingId}`);
    
    // For Case 4, we need to trigger polling to get the actual results
    // The raw result contains the trackingId, now we need to poll for final results
    const trackingId = rawResults.trackingId;
    
    // Poll third-party service for final results
    const pollingResults = await this.implementServiceManagedPolling('batch_id', trackingId, requests.map(r => r.requestId));
    
    // Map polling results back to BatchItem[] format
    return requests.map(request => {
      const matchingResult = pollingResults.results.find(result => 
        result.id === request.request.id
      );
      
      if (!matchingResult) {
        throw new Error(`No matching result found for documentId: ${request.request.id}`);
      }
      
      return {
        ...request,           // Keep requestId and request as-is
        response: matchingResult // Add the response
      };
    });
  }

  // ============================================================================
  // 📝 STEP 5.5: SERVICE CALLBACKS - FINAL STEP (Delivers results to external systems)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Send callback for each individual result - FINAL STEP
   * 
   * This is where results are actually delivered to external systems after polling completes.
   * This is the library's callback mechanism for Case 4.
   * 
   * ⚡ WHEN: Called by library for EACH request/result pair after Step 4.5 completes
   *         Library maps each result from Step 4.5 back to its original client request
   *         Then triggers this method once per request with (result + originalRequest)
   * 🎯 PURPOSE: Deliver individual results via HTTP, PubSub, files, etc.
   * 📋 YOUR JOB: Implement your callback delivery logic (HTTP POST, message queue, etc.)
   */
  public async sendIndividualCallback(result: DocumentResult, originalRequest: DocumentRequest): Promise<void> {
    this.logger.log(`📤 Sending callback for document ${result.id}`);
    
    try {
      // 🔧 ADD YOUR CALLBACK DELIVERY LOGIC HERE
      // This is where you send the result to external systems
      
      // Example: HTTP Callback to client's webhook
      // await this.httpClient.post(originalRequest.callbackUrl || 'https://default-webhook.com', {
      //   documentId: result.id,
      //   status: result.status,
      //   downloadUrl: result.conversionResult?.outputUrl,
      //   outputFormat: result.conversionResult?.outputFormat,
      //   fileSize: result.conversionResult?.fileSize,
      //   processingTime: result.conversionResult?.processingTime,
      //   processedAt: result.processedAt,
      //   originalRequest: originalRequest
      // });
      
      // Example: Email Notification
      // if (originalRequest.notificationEmail) {
      //   await this.emailService.sendProcessingComplete({
      //     to: originalRequest.notificationEmail,
      //     documentId: result.id,
      //     status: result.status,
      //     downloadUrl: result.conversionResult?.outputUrl
      //   });
      // }
      
      // Example: Message Queue
      // await this.messageQueue.publish('document-processing-results', {
      //   result,
      //   originalRequest
      // });
      
      // 📝 SIMULATE CALLBACK: Replace with your actual callback logic
      await new Promise(resolve => setTimeout(resolve, 100)); // Simulate callback delivery
      
      this.logger.log(`✅ Callback sent successfully for document ${result.id}`);
      // 🔔 NEXT STEP: Processing complete - result delivered to external system!
      
    } catch (error) {
      this.logger.error(`❌ Failed to send callback for document ${result.id}:`, error);
      // 💡 NOTE: Don't throw - callback failures shouldn't break the main flow
      // The library will handle retry logic if configured
    }
  }

  // ============================================================================
  // 📝 THAT'S IT! 🎉
  // ============================================================================
  
  /*
   * 🎯 SUMMARY: What you implemented above:
   * 
   * 1️⃣ EXTENDED the library class: AsyncHttpProcessor<YourRequestType, YourResultType>
   * 2️⃣ ADDED public methods for your controller to call (processDocument)
   * 3️⃣ IMPLEMENTED inline validation and data preparation in public methods
   * 4️⃣ IMPLEMENTED processBatchItems() for third-party submission (returns trackingId)
   * 4️⃣.5 IMPLEMENTED implementServiceManagedPolling() for polling third-party results
   * 5️⃣.5 IMPLEMENTED sendIndividualCallback() for service callbacks (delivers results)
   * 
   * 🔄 WHAT THE LIBRARY HANDLES FOR YOU:
   * - Batching requests together
   * - Storing requests and results
   * - Background processing orchestration
   * - Polling orchestration and scheduling
   * - Callback triggering and management
   * - Error handling and retries
   * - Status tracking
   * 
   * 📋 WHAT YOUR CONTROLLER LOOKS LIKE:
   * 
   * @Controller('documents')
   * export class DocumentController {
   *   constructor(private documentService: DocumentProcessingService) {}
   * 
   *   @Post()
   *   async processDocument(@Body() document: DocumentRequest) {
   *     return this.documentService.processDocument(document); // Returns { requestId }
   *   }
   * }
   * 
   * 🚀 CLIENT USAGE FLOW:
   * 1. POST /documents → processDocument() → Get { requestId } immediately (HTTP 202)
   * 2. Library processes in background → Submit to third-party → Get trackingId
   * 3. Library polls third-party → Get final results → Store them
   * 4. Library triggers sendIndividualCallback() for each result → Results delivered
   * 5. No client polling needed - results pushed to external systems automatically
   * 
   * 🔑 CASE 4 KEY FEATURES:
   * - Third-party submission with tracking ID
   * - Service-managed polling (you implement the polling logic)
   * - Service callbacks for result delivery (no client polling needed)
   * - Document-specific business logic and data transformation
   */
}
```

</div>

</div>

</div>

</div>

# Case 5: Tracking ID + Third-party Webhook + Service Callback


![[48719528005-Case 5_Tracking ID_Third-party Webhook_Service Callback.png]]



<div id="expander-931229612" class="expand-container conf-macro output-block" hasbody="true" macro-id="68ed6acd-f554-4801-803e-2db80829de2f" macro-name="expand">

<div id="expander-control-931229612" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Code Example</span>

</div>

<div id="expander-content-931229612" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7828e58f-17ca-4db3-b781-ef1a29f4d020" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/**
 * Case 5: Tracking ID + Webhook + Service Callback - SIMPLIFIED EXAMPLE
 * 
 * 🎯 PURPOSE: Show developers exactly how to use the AsyncHttpProcessor library
 * 
 * 📋 WHAT YOU NEED TO IMPLEMENT:
 * 1. Extend AsyncHttpProcessor<YourRequestType, YourResultType>
 * 2. Implement processBatchItems() - Step 3: Get raw results from third-party
 * 3. Implement mapBatchResults() - Step 4: Process webhook data and map to BatchItem[]
 * 4. Implement storeWebhookData() - for storing webhook data from third-party
 * 5. Implement sendIndividualCallback() - for service callbacks
 * 6. Add public methods with inline validation for your controller to call
 * 
 * 📄 DTO REFERENCE: See document-processing.types.ts for DocumentRequest structure
 * 
 * 🔄 LIBRARY FLOW (You don't implement this - library handles it):
 * Client → Controller → Service.processDocument() → Library.processAsyncRequest() → HTTP 202
 * Background: Library → Service.processBatchItems() → Returns trackingId → Library waits for webhook
 * Webhook: Controller → Service.storeWebhookData() → Library → Service.mapBatchResults()
 * Callbacks: Library → Service.sendIndividualCallback() for each result
 */

import { Injectable, Logger } from '@nestjs/common';
import { AsyncHttpProcessor } from '../../src/processors/http/async-http-processor.service';
import { BulkProcessResult, BatchItem } from '../../src/types/async-batch-processor.types';
import { 
  DocumentRequest, 
  DocumentResult
} from './document-processing.types';

// ============================================================================
// 📝 STEP 1: EXTEND THE LIBRARY CLASS
// ============================================================================

@Injectable()
export class DocumentProcessingService extends AsyncHttpProcessor<DocumentRequest, any> {
  private readonly logger = new Logger(DocumentProcessingService.name);
  
  // 🔧 SERVICE-LEVEL WEBHOOK CONFIGURATION (NOT library config)
  // This is where webhook settings belong - managed by the service, not the library
  private readonly webhookConfig = {
    callbackUrl: process.env.WEBHOOK_CALLBACK_URL || 'https://your-app.com/api/webhooks/documents',
    secret: process.env.WEBHOOK_SECRET || 'your-webhook-secret',
    timeout: 30000, // 30 seconds for webhook calls
    retryAttempts: 3
  };

  constructor() {
    // 🔧 CONFIGURE THE LIBRARY - Simple inline 3-phase config
    const config = {
      // Global settings used across all phases
      global: {
        // 🗄️ STORAGE CONFIGURATION (EXAMPLE - NOT FINALIZED)
        // NOTE: The final storage implementation will depend on your chosen database/service:
        // - Could be PostgreSQL, MongoDB, Redis, DynamoDB, etc.
        // - Could be a dedicated microservice, cloud service, or embedded database
        // - This example shows the interface structure, not the final implementation
        storage: {
          baseUrl: 'https://your-luz-batching-service.com',  // Your storage service endpoint
          timeoutMs: 30000,                                   // Connection timeout
          retryConfig: {
            enabled: true,
            maxRetries: 3,
            retryDelayMs: 1500
          }
        },
        batchType: 'DOCUMENT_PROCESSING',
        logLevel: 'info' as const
      },
      
      // Request submission phase (reserved for future features)
      requestSubmission: {},
      
      // Background processing phase
      backgroundProcessing: {
        targetBatchSize: 4,                     // Smaller batches for webhook processing
        maxBatchWaitMs: 2500,                   // Shorter wait time for real-time processing
        mode: 'CALLBACK_RESULTS' as const,     // Results come from third-party via webhook
        processingTimeoutMs: 900000             // 15 minute timeout for webhook processing
      },
      
      // Result return phase  
      resultReturn: {
        method: 'CALLBACK' as const,            // Service sends callbacks
        callback: {
          timeoutMs: 60000,                     // Timeout for webhook processing
          retryOnTimeout: false                 // Don't retry webhooks automatically
        },
        enableIndividualCallbacks: true         // Enable individual result callbacks
      }
    };
    
    super(config, new Logger(DocumentProcessingService.name));
  }

  // ============================================================================
  // 📝 STEP 2: ADD PUBLIC METHODS FOR YOUR CONTROLLER TO CALL
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Submit document for processing
   * 
   * This is what your controller calls. The library handles the rest.
   * 
   * 💡 RESULT DEPENDS ON INLINE VALIDATION: If validation fails, this throws error.
   * If validation passes, returns { requestId } for tracking.
   */
  async processDocument(documentRequest: DocumentRequest): Promise<{ requestId: string }> {
    this.logger.log(`📥 Submitting document request: ${documentRequest.id}`);
    
    // 🔍 STEP 2.1: VALIDATE REQUEST (throw HTTP 400 errors for invalid data)
    if (!documentRequest.documentId) {
      throw new Error('Document ID is required');
    }
    if (!documentRequest.documentType) {
      throw new Error('Document type is required');
    }
    if (!documentRequest.content && !documentRequest.fileUrl) {
      throw new Error('Either content or fileUrl is required');
    }
    // Add more validation as needed...
    
    // 🔧 STEP 2.2: ENRICH REQUEST DATA (prepare for processing)
    const enrichedDocument = {
      // Include original data
      ...documentRequest,
      
      // Add computed/normalized fields (customize for your needs)
      normalizedDocumentType: documentRequest.documentType.toUpperCase(),
      processingPriority: documentRequest.urgent ? 'HIGH' : 'NORMAL',
      submittedAt: new Date().toISOString(),
      
      
      // Add any fields your business logic needs...
    };
    
    // 🔵 CALL LIBRARY: Submit pre-validated data for async processing
    const result = await this.processAsyncRequest(enrichedDocument);
    
    this.logger.log(`✅ Document submitted with requestId: ${result.requestId}`);
    // 🔔 NEXT STEP: Library will batch requests and call processBatchItems() (Step 3)
    return result; // Returns { requestId: "abc123" } immediately
  }


  // ============================================================================
  // 📝 STEP 3: IMPLEMENT BUSINESS LOGIC (Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 3: Process a batch of validated requests in the background (Phase 1)
   * 
   * ⚡ WHEN: Called later in background after HTTP 202 response sent
   *         Triggered when batch conditions are met:
   *         - Batch reaches targetBatchSize (4 requests in this config)
   *         - OR maxBatchWaitMs timeout expires (2500ms in this config)
   *         - OR library shutdown/flush is triggered
   * 
   * 🎯 PURPOSE: Submit documents to third-party service and get tracking ID
   * 📋 YOUR JOB: Send documents to third-party and return raw results (tracking ID)
   * 
   * 💡 NOTE: Items are already validated (inline validation was called first)
   */
  async processBatchItems(items: BatchItem[]): Promise<any> {
    this.logger.log(`🔄 Processing batch of ${items.length} validated documents`);
    
    // 💡 NOTE: items contains the validated/enriched data from processDocument()
    //          This is the data you prepared inline in processDocument() (Step 2.1 validation + Step 2.2 enrichment)
    //
    // 🔑 IMPORTANT: item.request.id = Business ID (documentId, etc.)
    //              requestId from Step 2 = Library tracking ID (different thing!)
    //
    // 🎯 PROCESSING OPTIONS:
    // Option A: Bulk API - Send all business data at once to third-party
    //   const allBusinessData = items.map(item => item.request);
    //   const trackingId = await this.documentWebhookAPI.submitBatch(allBusinessData);
    //
    // Option B: Individual submission - Submit each document separately
    //   Use this when third-party doesn't support bulk operations

    try {
      // 🏗️ ADD YOUR BUSINESS LOGIC HERE
      // Submit documents to third-party service using business data from Step 2
      
      // Extract business data from BatchItem[] (Format 2)
      // items = [
      //   { requestId: "req_1", request: { id: "DOC_123", content: "...", documentType: "PDF", ... } },
      //   { requestId: "req_2", request: { id: "DOC_456", content: "...", documentType: "DOCX", ... } }
      // ]
      const businessData = items.map(item => item.request);
      
      // EXAMPLE: Prepare batch for third-party document webhook service
      const batchSubmission = {
        batchId: `doc_webhook_${Date.now()}`,
        documents: businessData.map(data => ({
          documentId: data.id, // Business ID from client request (NOT the requestId from Step 2)
          type: data.documentType || data.normalizedDocumentType,
          mode: data.processingMode || 'STANDARD',
          metadata: {
            priority: data.processingPriority,
            submittedAt: data.submittedAt,
            urgent: data.urgent
          },
          callbackConfig: {
            enableCallbacks: true,
            webhookUrl: process.env.WEBHOOK_CALLBACK_URL || 'https://your-app.com/api/webhooks',
            retryAttempts: 3
          },
          content: data.content
        })),
        webhookUrl: process.env.WEBHOOK_CALLBACK_URL || 'https://your-app.com/api/webhooks', // Where third-party will send results
        submissionInfo: {
          batchSize: businessData.length,
          estimatedTime: businessData.some(d => d.fileSize > 5000000) ? '10-15 minutes' : '2-5 minutes'
        }
      };
      
      // 📝 SIMULATE THIRD-PARTY SUBMISSION: Replace with your actual API call
      // const trackingId = await this.documentWebhookAPI.submitBatch(batchSubmission);
      const trackingId = `webhook_track_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
      await new Promise(resolve => setTimeout(resolve, 100)); // Simulate API call
      
      this.logger.log(`✅ Step 3 complete: Got tracking ID from third-party: ${trackingId}`);
      
      // 📤 RETURN TO LIBRARY: trackingId as raw result - library stores immediately for data safety
      // 🔑 CASE 5 PATTERN: Webhook pattern - results will come later via webhook processing
      // Results will come later via webhook → mapBatchResults()
      // 🔔 NEXT STEP: Library waits for webhook callback, then triggers mapBatchResults() (Step 4)
      return { trackingId };
      
    } catch (error) {
      this.logger.error(`❌ Failed to submit batch to third-party:`, error);
      throw error; // Let library handle the error
    }
  }


  // ============================================================================
  // 📝 STEP 4.5: IMPLEMENT WEBHOOK PROCESSING (Called when webhook received)
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Store webhook data from third-party
   * 
   * This is what your webhook controller calls when third-party sends results.
   * Follows the Controller → Service → Library pattern.
   * 
   * ⚡ WHEN: Called by your controller when third-party sends webhook
   * 🎯 PURPOSE: Store raw webhook data and trigger background processing
   * 📋 YOUR JOB: Act as service layer wrapper for webhook storage
   */
  async storeWebhookData(batchId: string, webhookPayload: any): Promise<void> {
    this.logger.log(`📥 Storing webhook data for batch: ${batchId}`);
    
    // 🔵 CALL LIBRARY: Store raw webhook data (fast storage for data safety)
    await this.storeWebhookCallbackData(batchId, webhookPayload);
    
    this.logger.log(`✅ Webhook data stored for batch: ${batchId}`);
    // 🔔 NEXT STEP: Controller returns HTTP 200 OK to third-party, then library calls mapBatchResults()
  }


  // ============================================================================
  // 📝 STEP 4: IMPLEMENT MAPPING LOGIC (Phase 2 - Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 4: Map raw results to BatchItem[]
   * 
   * ⚡ WHEN: Called by library after webhook data has been received and stored
   * 🎯 PURPOSE: Process webhook data and map results back to original requests
   */
  async mapBatchResults(requests: BatchItem[], rawResults: any): Promise<BatchItem[]> {
    this.logger.log(`🔄 Step 4: Processing webhook data and mapping to BatchItem[]`);
    
    // rawResults contains the webhook data stored by storeWebhookData()
    // Process the webhook data directly here
    const webhookData = rawResults.webhookData || rawResults;
    
    try {
      // 🏗️ ADD YOUR WEBHOOK PROCESSING LOGIC HERE
      // Parse the webhook data from third-party service
      
      // EXAMPLE: Process webhook data structure
      const webhookResults = webhookData.results || webhookData.documents || [];
      
      // EXAMPLE: Map webhook results to library format and then to BatchItem[]
      return requests.map(request => {
        const webhookResult = webhookResults.find(result => 
          (result.documentId || result.id) === request.request.id
        );
        
        if (!webhookResult) {
          throw new Error(`No matching webhook result found for documentId: ${request.request.id}`);
        }
        
        // Create the final result object
        const finalResult = {
          id: request.request.id,
          status: webhookResult.success ? 'SUCCESS' : 'ERROR',
          processedAt: new Date(),
          result: webhookResult.success ? {
            processedContent: webhookResult.processedContent || 'Webhook processing completed',
            analysisResults: webhookResult.analysis || {},
            processingTime: webhookResult.processingTimeMs || 0,
            confidence: webhookResult.confidence || 1.0
          } : undefined,
          errorDetails: webhookResult.success ? undefined : webhookResult.error || 'Webhook processing failed'
        };
        
        return {
          ...request,           // Keep requestId and request as-is
          response: finalResult // Add the processed response
        };
      });
      
    } catch (error) {
      this.logger.error(`❌ Error processing webhook data:`, error);
      
      // Return error results for all requests
      return requests.map(request => ({
        ...request,
        response: {
          id: request.request.id,
          status: 'ERROR',
          processedAt: new Date(),
          errorDetails: `Webhook processing failed: ${error.message}`
        }
      }));
    }
  }

  // ============================================================================
  // 📝 STEP 5: SERVICE CALLBACKS - FINAL STEP (Delivers results to external systems)
  // ============================================================================

  /**
   * 🔵 LIBRARY CALLS THIS: Send callback for each individual result - FINAL STEP
   * 
   * This is where results are actually delivered to external systems.
   * This is the library's callback mechanism for Case 5.
   * 
   * ⚡ WHEN: Called by library for EACH request/result pair after Step 4.5 completes
   *         Library maps each result from Step 4.5 back to its original client request
   *         Then triggers this method once per request with (result + originalRequest)
   * 🎯 PURPOSE: Deliver individual results via HTTP, PubSub, files, etc.
   * 📋 YOUR JOB: Implement your callback delivery logic (HTTP POST, message queue, etc.)
   */
  public async sendIndividualCallback(result: DocumentResult, originalRequest: DocumentRequest): Promise<void> {
    this.logger.log(`📤 Sending callback for document ${result.id}`);
    
    try {
      // 🔧 ADD YOUR CALLBACK DELIVERY LOGIC HERE
      // This is where you send the result to external systems
      
      // Example: HTTP Callback
      // await this.httpClient.post('https://your-webhook-endpoint.com/documents', {
      //   documentId: result.id,
      //   status: result.status,
      //   processedAt: result.processedAt,
      //   originalRequest: originalRequest
      // });
      
      // Example: Message Queue
      // await this.messageQueue.publish('document-results', {
      //   result,
      //   originalRequest
      // });
      
      // Example: File System
      // await this.fileSystem.writeResult(`results/${result.id}.json`, result);
      
      // 📝 SIMULATE CALLBACK: Replace with your actual callback logic
      await new Promise(resolve => setTimeout(resolve, 50)); // Simulate callback delivery
      
      this.logger.log(`✅ Callback sent successfully for document ${result.id}`);
      // 🔔 NEXT STEP: Processing complete - result delivered to external system!
      
    } catch (error) {
      this.logger.error(`❌ Failed to send callback for document ${result.id}:`, error);
      // 💡 NOTE: Don't throw - callback failures shouldn't break the main flow
      // The library will handle retry logic if configured
    }
  }


  // ============================================================================
  // 📝 THAT'S IT! 🎉
  // ============================================================================
  
  /*
   * 🎯 SUMMARY: What you implemented above:
   * 
   * 1️⃣ EXTENDED the library class: AsyncHttpProcessor<YourRequestType, YourResultType>
   * 2️⃣ ADDED public method for your controller to call (processDocument)
   * 3️⃣ IMPLEMENTED inline validation and data preparation in public methods
   * 4️⃣ IMPLEMENTED processBatchItems() for third-party submission (returns trackingId)
   * 4️⃣.5 IMPLEMENTED webhook processing (storeWebhookData + handleBatchCallback)
   * 5️⃣ IMPLEMENTED sendIndividualCallback() for service callbacks (delivers results)
   * 
   * 🔄 WHAT THE LIBRARY HANDLES FOR YOU:
   * - Batching requests together
   * - Storing requests and results
   * - Background processing orchestration
   * - Webhook orchestration and data storage
   * - Error handling and retries
   * - Callback triggering and management
   * 
   * 📋 WHAT YOUR CONTROLLER LOOKS LIKE:
   * 
   * @Controller('documents')
   * export class DocumentController {
   *   constructor(private documentService: DocumentProcessingService) {}
   * 
   *   @Post()
   *   async processDocument(@Body() document: DocumentRequest) {
   *     return this.documentService.processDocument(document); // Returns { requestId }
   *   }
   * 
   *   @Post('webhook/callback')
   *   async handleWebhook(@Body() webhookData: any, @Query('batchId') batchId: string) {
   *     await this.documentService.storeWebhookData(batchId, webhookData);
   *     return { status: 'success' }; // Fast response to third-party
   *   }
   * }
   * 
   * 🚀 CLIENT USAGE FLOW:
   * 1. POST /documents → processDocument() → Get { requestId } immediately (HTTP 202)
   * 2. Library processes in background → Submit to third-party → Get trackingId
   * 3. Third-party calls webhook → storeWebhookData() → Background processing
   * 4. Library processes webhook → Results delivered via sendIndividualCallback()
   * 5. No polling needed - results pushed to external systems automatically
   * 
   * 🔑 CASE 5 KEY FEATURES:
   * - Third-party submission with tracking ID
   * - Webhook-based result delivery (no polling)
   * - Service callbacks for result distribution
   * - Document-specific business logic and data transformation
   * - Fast webhook response (store first, process in background)
   */
}
```

</div>

</div>

</div>

</div>

# Case 6: Tracking ID + Third-party Webhook + Client Polling


![[48719528005-Case 6_Tracking ID_Third-party Webhook_Client Polling.png]]



<div id="expander-760162283" class="expand-container conf-macro output-block" hasbody="true" macro-id="8c80cfc5-919a-4b69-b7c5-8037d2af3436" macro-name="expand">

<div id="expander-control-760162283" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Code Example</span>

</div>

<div id="expander-content-760162283" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4357f98c-89ca-4d31-b798-9fc764b88079" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/**
 * Case 6: Tracking ID + Webhook + Client Polling - SIMPLIFIED EXAMPLE
 * 
 * 🎯 PURPOSE: Show developers exactly how to use the AsyncHttpProcessor library
 * 
 * 📋 WHAT YOU NEED TO IMPLEMENT:
 * 1. Extend AsyncHttpProcessor<YourRequestType, YourResultType>
 * 2. Implement processBatchItems() - Step 3: Get raw results from third-party
 * 3. Implement mapBatchResults() - Step 4: Process webhook data and map to BatchItem[]
 * 4. Implement storeWebhookData() - for storing webhook data from third-party
 * 5. Add public methods with inline validation for your controller to call
 * 
 * 📄 DTO REFERENCE: See document-processing.types.ts for DocumentRequest structure
 * 
 * 🔄 LIBRARY FLOW (You don't implement this - library handles it):
 * Client → Controller → Service.processDocument() → Library.processAsyncRequest() → HTTP 202
 * Background: Library → Service.processBatchItems() → Returns trackingId → Library waits for webhook
 * Webhook: Controller → Service.storeWebhookData() → Library → Service.mapBatchResults()
 * Polling: Client → Controller → Service.getRequestStatus() → Library.getRequestStatus()
 */

import { Injectable, Logger } from '@nestjs/common';
import { AsyncHttpProcessor } from '../../src/processors/http/async-http-processor.service';
import { BulkProcessResult, BatchItem } from '../../src/types/async-batch-processor.types';
import { 
  DocumentRequest, 
  DocumentResult
} from './document-processing.types';

// ============================================================================
// 📝 STEP 1: EXTEND THE LIBRARY CLASS
// ============================================================================

@Injectable()
export class DocumentProcessingService extends AsyncHttpProcessor<DocumentRequest, any> {
  private readonly logger = new Logger(DocumentProcessingService.name);
  
  // 🔧 SERVICE-LEVEL WEBHOOK CONFIGURATION (NOT library config)
  // This is where webhook settings belong - managed by the service, not the library
  private readonly webhookConfig = {
    callbackUrl: process.env.WEBHOOK_CALLBACK_URL || 'https://your-app.com/api/webhooks/documents',
    secret: process.env.WEBHOOK_SECRET || 'your-webhook-secret',
    timeout: 30000, // 30 seconds for webhook calls
    retryAttempts: 3
  };
  
  // 🔧 SERVICE-LEVEL CLIENT POLLING CONFIGURATION
  // This is for client polling behavior - can be used by controllers/clients
  private readonly clientPollingConfig = {
    pollingInterval: 5000, // 5 seconds between polls
    maxPollingAttempts: 120, // 10 minutes total (120 * 5s)
    exponentialBackoff: false
  };

  constructor() {
    // 🔧 CONFIGURE THE LIBRARY - Simple inline 3-phase config
    const config = {
      // Global settings used across all phases
      global: {
        // 🗄️ STORAGE CONFIGURATION (EXAMPLE - NOT FINALIZED)
        // NOTE: The final storage implementation will depend on your chosen database/service:
        // - Could be PostgreSQL, MongoDB, Redis, DynamoDB, etc.
        // - Could be a dedicated microservice, cloud service, or embedded database
        // - This example shows the interface structure, not the final implementation
        storage: {
          baseUrl: 'https://your-luz-batching-service.com',  // Your storage service endpoint
          timeoutMs: 15000,                                   // Faster storage operations
          retryConfig: {
            enabled: true,
            maxRetries: 2,                                    // Fewer retries for speed
            retryDelayMs: 500
          }
        },
        batchType: 'DOCUMENT_PROCESSING',
        logLevel: 'info' as const
      },
      
      // Request submission phase (reserved for future features)
      requestSubmission: {},
      
      // Background processing phase
      backgroundProcessing: {
        targetBatchSize: 3,                     // Small batches for real-time processing
        maxBatchWaitMs: 2000,                   // Quick processing for client polling
        mode: 'CALLBACK_RESULTS' as const,     // Results come from third-party via webhook
        processingTimeoutMs: 900000             // 15 minute timeout for webhook processing
      },
      
      // Result return phase  
      resultReturn: {
        method: 'POLLING' as const,             // Client polls for results
        polling: {
          enableStatusEndpoint: true,           // Enable status checking endpoint
          statusCacheMs: 5000                   // Cache status for 5 seconds
        },
        enableIndividualCallbacks: false       // Case 6: Client polling - no service callbacks
      }
    };
    
    super(config, new Logger(DocumentProcessingService.name));
  }

  // ============================================================================
  // 📝 STEP 2: ADD PUBLIC METHODS FOR YOUR CONTROLLER TO CALL
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Submit document for processing
   * 
   * This is what your controller calls. The library handles the rest.
   * 
   * 💡 RESULT DEPENDS ON INLINE VALIDATION: If validation fails, this throws error.
   * If validation passes, returns { requestId } for client polling.
   */
  async processDocument(documentRequest: DocumentRequest): Promise<{ requestId: string }> {
    this.logger.log(`📥 Submitting document request: ${documentRequest.id}`);
    
    // 🔍 STEP 2.1: VALIDATE REQUEST (throw HTTP 400 errors for invalid data)
    if (!documentRequest.documentId) {
      throw new Error('Document ID is required');
    }
    if (!documentRequest.documentType) {
      throw new Error('Document type is required');
    }
    if (!documentRequest.content && !documentRequest.fileUrl) {
      throw new Error('Either content or fileUrl is required');
    }
    // Add more validation as needed...
    
    // 🔧 STEP 2.2: ENRICH REQUEST DATA (prepare for processing)
    const enrichedDocument = {
      // Include original data
      ...documentRequest,
      
      // Add computed/normalized fields (customize for your needs)
      normalizedDocumentType: documentRequest.documentType.toUpperCase(),
      processingPriority: documentRequest.urgent ? 'HIGH' : 'NORMAL',
      submittedAt: new Date().toISOString(),
      
      
      // Add any fields your business logic needs...
    };
    
    // 🔵 CALL LIBRARY: Submit pre-validated data for async processing
    const result = await this.processAsyncRequest(enrichedDocument);
    
    this.logger.log(`✅ Document submitted with requestId: ${result.requestId}`);
    // 🔔 NEXT STEP: Library will batch requests and call processBatchItems() (Step 3)
    return result; // Returns { requestId: "abc123" } immediately
  }



  // ============================================================================
  // 📝 STEP 3: IMPLEMENT BUSINESS LOGIC (Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 3: Process a batch of validated requests in the background (Phase 1)
   * 
   * ⚡ WHEN: Called later in background after HTTP 202 response sent
   *         Triggered when batch conditions are met:
   *         - Batch reaches targetBatchSize (3 requests in this config)
   *         - OR maxBatchWaitMs timeout expires (2000ms in this config)
   *         - OR library shutdown/flush is triggered
   * 
   * 🎯 PURPOSE: Submit documents to third-party service and get tracking ID
   * 📋 YOUR JOB: Send documents to third-party and return raw results (tracking ID)
   * 
   * 💡 NOTE: Items are already validated (inline validation was called first)
   */
  async processBatchItems(items: BatchItem[]): Promise<any> {
    this.logger.log(`🔄 Processing batch of ${items.length} validated documents`);
    
    // 💡 NOTE: items contains the validated/enriched data from processDocument()
    //          This is the data you prepared inline in processDocument() (Step 2.1 validation + Step 2.2 enrichment)
    //
    // 🔑 IMPORTANT: item.request.id = Business ID (documentId, etc.)
    //              requestId from Step 2 = Library tracking ID (different thing!)
    //
    // 🎯 PROCESSING OPTIONS:
    // Option A: Bulk API - Send all business data at once to third-party
    //   const allBusinessData = items.map(item => item.request);
    //   const trackingId = await this.documentWebhookAPI.submitBatch(allBusinessData);
    //
    // Option B: Individual submission - Submit each document separately
    //   Use this when third-party doesn't support bulk operations

    try {
      // 🏗️ ADD YOUR BUSINESS LOGIC HERE
      // Submit documents to third-party service using business data from Step 2
      
      // Extract business data from BatchItem[] (Format 2)
      // items = [
      //   { requestId: "req_1", request: { id: "DOC_123", content: "...", documentType: "PDF", ... } },
      //   { requestId: "req_2", request: { id: "DOC_456", content: "...", documentType: "DOCX", ... } }
      // ]
      const businessData = items.map(item => item.request);
      
      // EXAMPLE: Prepare batch for third-party document webhook service
      const batchSubmission = {
        batchId: `doc_webhook_${Date.now()}`,
        documents: businessData.map(data => ({
          documentId: data.id, // Business ID from client request (NOT the requestId from Step 2)
          format: data.documentType?.toUpperCase() || 'UNKNOWN',
          level: data.priority === 'URGENT' ? 'priority' : 'standard',
          metadata: {
            documentSize: data.content?.length || 0,
            expectedProcessingTime: data.priority === 'URGENT' ? 90000 : 240000, // 1.5min vs 4min
            contentFormat: data.contentType || 'application/pdf',
            languageHint: data.language || 'auto-detect'
          },
          webhookConfig: {
            callbackUrl: data.callbackUrl || process.env.WEBHOOK_CALLBACK_URL || 'https://your-app.com/api/webhooks',
            retryCount: data.priority === 'URGENT' ? 3 : 2,
            timeoutMs: 30000
          },
          content: data.content
        })),
        webhookUrl: process.env.WEBHOOK_CALLBACK_URL || 'https://your-app.com/api/webhooks', // Where third-party will send results
        processingContext: {
          batchSize: businessData.length,
          submittedAt: new Date().toISOString()
        }
      };
      
      // 📝 SIMULATE THIRD-PARTY SUBMISSION: Replace with your actual API call
      // const trackingId = await this.documentWebhookAPI.submitBatch(batchSubmission);
      const trackingId = `webhook_track_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
      await new Promise(resolve => setTimeout(resolve, 80)); // Simulate API call
      
      this.logger.log(`✅ Step 3 complete: Got tracking ID from third-party: ${trackingId}`);
      
      // 📤 RETURN TO LIBRARY: trackingId as raw result - library stores immediately for data safety
      // 🔑 CASE 6 PATTERN: Webhook pattern - results will come later via webhook processing
      // Results will come later via webhook → mapBatchResults()
      // 🔔 NEXT STEP: Library waits for webhook callback, then triggers mapBatchResults() (Step 4)
      return { trackingId };
      
    } catch (error) {
      this.logger.error(`❌ Failed to submit batch to third-party:`, error);
      throw error; // Let library handle the error
    }
  }

  // ============================================================================
  // 📝 STEP 4.5: IMPLEMENT WEBHOOK PROCESSING (Called when webhook received)
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Store webhook data from third-party
   * 
   * This is what your webhook controller calls when third-party sends results.
   * Follows the Controller → Service → Library pattern.
   * 
   * ⚡ WHEN: Called by your controller when third-party sends webhook
   * 🎯 PURPOSE: Store raw webhook data and trigger background processing
   * 📋 YOUR JOB: Act as service layer wrapper for webhook storage
   */
  async storeWebhookData(batchId: string, webhookPayload: any): Promise<void> {
    this.logger.log(`📥 Storing webhook data for batch: ${batchId}`);
    
    // 🔵 CALL LIBRARY: Store raw webhook data (fast storage for data safety)
    await this.storeWebhookCallbackData(batchId, webhookPayload);
    
    this.logger.log(`✅ Webhook data stored for batch: ${batchId}`);
    // 🔔 NEXT STEP: Controller returns HTTP 200 OK to third-party, then library calls mapBatchResults()
  }

  // ============================================================================
  // 📝 STEP 4: IMPLEMENT MAPPING LOGIC (Phase 2 - Called during background processing)
  // ============================================================================

  /**
   * 🔵 STEP 4: Map raw results to BatchItem[]
   * 
   * ⚡ WHEN: Called by library after webhook data has been received and stored
   * 🎯 PURPOSE: Process webhook data and map results back to original requests
   */
  async mapBatchResults(requests: BatchItem[], rawResults: any): Promise<BatchItem[]> {
    this.logger.log(`🔄 Step 4: Processing webhook data and mapping to BatchItem[]`);
    
    // rawResults contains the webhook data stored by storeWebhookData()
    // Process the webhook data directly here
    const webhookData = rawResults.webhookData || rawResults;
    
    try {
      // 🏗️ ADD YOUR WEBHOOK PROCESSING LOGIC HERE
      // Parse the webhook data from third-party service
      
      // EXAMPLE: Process webhook data structure
      const webhookResults = webhookData.results || webhookData.documents || [];
      
      // EXAMPLE: Map webhook results to library format and then to BatchItem[]
      return requests.map(request => {
        const webhookResult = webhookResults.find(result => 
          (result.documentId || result.id) === request.request.id
        );
        
        if (!webhookResult) {
          throw new Error(`No matching webhook result found for documentId: ${request.request.id}`);
        }
        
        // Create the final result object
        const finalResult = {
          id: request.request.id,
          status: webhookResult.success ? 'SUCCESS' : 'ERROR',
          processedAt: new Date(),
          result: webhookResult.success ? {
            processedContent: webhookResult.processedContent || 'Webhook processing completed',
            analysisResults: webhookResult.analysis || {},
            processingTime: webhookResult.processingTimeMs || 0,
            confidence: webhookResult.confidence || 1.0
          } : undefined,
          errorDetails: webhookResult.success ? undefined : webhookResult.error || 'Webhook processing failed'
        };
        
        return {
          ...request,           // Keep requestId and request as-is
          response: finalResult // Add the processed response
        };
      });
      
    } catch (error) {
      this.logger.error(`❌ Error processing webhook data:`, error);
      
      // Return error results for all requests
      return requests.map(request => ({
        ...request,
        response: {
          id: request.request.id,
          status: 'ERROR',
          processedAt: new Date(),
          errorDetails: `Webhook processing failed: ${error.message}`
        }
      }));
    }
  }

  // ============================================================================
  // 📝 STEP 5: CLIENT POLLING - FINAL STEP (Returns results to client)
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Get document processing status - FINAL STEP
   * 
   * This is where clients actually retrieve the results from Step 4.5 (webhook processing).
   * This is what your controller calls for client polling.
   * 
   * 💡 RETURNS RESULTS FROM STEP 4.5: The results processed from webhook
   */
  async getRequestStatus(requestId: string): Promise<any> {
    this.logger.debug(`📊 Getting status for request: ${requestId}`);
    
    // 🔵 CALL LIBRARY: This retrieves results stored from Step 4.5 (webhook processing)
    const status = await super.getRequestStatus(requestId);
    
    if (!status) {
      throw new Error(`Request ${requestId} not found`);
    }

    // 🔄 TRANSFORM: Convert library format to your API format
    // The 'result' field contains what you processed in Step 4 (mapBatchResults)
    // 🔔 NEXT STEP: Return final results to client - processing complete!
    return {
      requestId,
      status: status.status,
      result: status.result, // This is your result from Step 4 webhook processing
      errorDetails: status.errorDetails,
      createdAt: status.createdAt,
      updatedAt: status.updatedAt
    };
  }

  // ============================================================================
  // 📝 THAT'S IT! 🎉
  // ============================================================================
  
  /*
   * 🎯 SUMMARY: What you implemented above:
   * 
   * 1️⃣ EXTENDED the library class: AsyncHttpProcessor<YourRequestType, YourResultType>
   * 2️⃣ ADDED public method for your controller to call (processDocument)
   * 3️⃣ IMPLEMENTED inline validation and data preparation in public methods
   * 4️⃣ IMPLEMENTED processBatchItems() for third-party submission (returns trackingId)
   * 4️⃣.5 IMPLEMENTED webhook processing (storeWebhookData + handleBatchCallback)
   * 5️⃣ IMPLEMENTED getRequestStatus() for client polling (returns final results)
   * 
   * 🔄 WHAT THE LIBRARY HANDLES FOR YOU:
   * - Batching requests together
   * - Storing requests and results
   * - Background processing orchestration
   * - Webhook orchestration and data storage
   * - Error handling and retries
   * - Status tracking and client polling
   * 
   * 📋 WHAT YOUR CONTROLLER LOOKS LIKE:
   * 
   * @Controller('documents')
   * export class DocumentController {
   *   constructor(private documentService: DocumentProcessingService) {}
   * 
   *   @Post()
   *   async processDocument(@Body() document: DocumentRequest) {
   *     return this.documentService.processDocument(document); // Returns { requestId }
   *   }
   * 
   *   @Post('webhook/callback')
   *   async handleWebhook(@Body() webhookData: any, @Query('batchId') batchId: string) {
   *     await this.documentService.storeWebhookData(batchId, webhookData);
   *     return { status: 'success' }; // Fast response to third-party
   *   }
   * 
   *   @Get(':requestId/status')
   *   async getStatus(@Param('requestId') requestId: string) {
   *     return this.documentService.getRequestStatus(requestId); // Returns status + results
   *   }
   * }
   * 
   * 🚀 CLIENT USAGE FLOW:
   * 1. POST /documents → processDocument() → Get { requestId } immediately (HTTP 202)
   * 2. Library processes in background → Submit to third-party → Get trackingId
   * 3. Third-party calls webhook → storeWebhookData() → Background processing
   * 4. Library processes webhook → Results stored for client polling
   * 5. GET /documents/{requestId}/status → getRequestStatus() → Poll for results
   * 6. When status = 'SUCCESS', results from webhook processing are available
   * 
   * 🔑 CASE 6 KEY FEATURES:
   * - Third-party submission with tracking ID
   * - Webhook-based result delivery (no polling third-party)
   * - Client polling for final results
   * - Document-specific business logic and data transformation
   * - Fast webhook response (store first, process in background)
   */

  // ============================================================================
  // 📝 CONFIGURATION ACCESS METHODS
  // ============================================================================

  /**
   * 🟢 YOUR PUBLIC API: Get client polling configuration
   * 
   * ⚡ WHEN: Called by controllers or clients that need polling settings
   * 🎯 PURPOSE: Provide consistent polling configuration across your app
   */
  getClientPollingConfig() {
    return {
      ...this.clientPollingConfig,
      totalTimeoutMs: this.clientPollingConfig.pollingInterval * this.clientPollingConfig.maxPollingAttempts
    };
  }
}
```

</div>

</div>

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Recipe Luz Batch TypeScript]]
- [[Recipe Typescript batching]]
- [[Batch Processor Library - NodeJS]]
- [[Batching Design]]
- [[Luz Batch TypeScript - Configuration]]

%% ai-graph-end %%