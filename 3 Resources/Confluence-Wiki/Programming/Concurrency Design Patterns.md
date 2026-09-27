---
ai_hash: 4494b7a2ed322bd2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 13
depth: 3
entities: []
relevance: 0.906
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47091122608/Concurrency+Design+Patterns
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Concurrency Design Patterns
topic: programming
type: source
updated: 2022-09-30
---

# Concurrency Design Patterns

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-09-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47091122608/Concurrency+Design+Patterns)
> Relevance 0.906 · topic `programming`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">


![[47091122608-condespat2.png]]



## Improvement 0: Use threadpools non-blocking

When we use thread pools we must make sure that the thread scheduling it is not blocked by waiting for its completion this usually happens by either calling `CompletableFuture::join` or `CompletatbleFuture::get`. Below two examples demonstrating the problem:

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

**AddressNormalizerService.java** (Original)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3072e5e1-4a00-4a91-bf33-f75931603f09" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public List<AddressForBatchNormalize> normalizeParallelAddress(List<AddressForBatchNormalize> addresses, AddressKind type) throws ExecutionException, InterruptedException {
        AbstractAsyncExecutor executor;
        if (addresses.size() <= maximumSizeToRunWithParallelInSmallPool) {
            executor = smallRequestParallelExecutor;
        } else {
            executor = parallelExecutor;
        }

        CompletableFuture<List<AddressForBatchNormalize>> future = executor.supply(() -> {
            NormalizeParallelRequest normalizeParallelRequest = AddressConverter.convertToNormalizeParallelRequest(addresses, type);
            long current = System.currentTimeMillis();
            NormalizeParallelResponse normalizeParallelResponse = eireneRestClient.normalizeParallel(normalizeParallelRequest);
            LOGGER.log(Level.INFO, "EIRENE took {0}ms to parallel normalize", System.currentTimeMillis() - current);
            return AddressConverter.fromNormalizeParallelResponseBackToAddresses(normalizeParallelResponse);
        });
        return future.get();
    }
```

</div>

</div>

Here we see that directly after scheduling the task, we call `ComletableFuture::get` which will make the main thread (which might be an I/O thread) block and wait for the completion of the scheduled task. As the custom thread-pools are usually smaller than the default worker thread pool we introduced a new bottle-neck.

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

**DocumentDeliveryService.java** (Original)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="762aaaa6-9a8c-43ff-b23e-777d0a65010d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public CompletableFuture<Void> importDocumentAsync(String tenantId, List<DocumentDataCarrier> documentDataCarriers, Token token, long deliveryId,
                                                       DeliveryProcessingStatusTracking deliveryProcessingStatusTracking,
                                                       DeliveryContext deliveryContext) {
        return deliveryRunningExecutor.run(() -> importDocument(tenantId, token, documentDataCarriers, deliveryContext, deliveryProcessingStatusTracking))
                .exceptionally(throwable -> {
                    Logger.getLogger(DocumentDeliveryService.class.getName()).log(Level.SEVERE,
                            loggerMessageProducer.from(tenantId, deliveryId, "Unexpected error deliver the delivery async. Suspending the delivery and retry later."), throwable);
                    // Mark the delivery as SUSPENDED to retry
                    // TODO - dvdat: Revert the ORIGIN checking after applying long-term solution
                    if (Objects.nonNull(deliveryProcessingStatusTracking)) {
                        deliveryProcessingStatusTrackingService.suspendTrackingDeliveryProcessingStatus(deliveryId);
                    }

                    return null;
                });
    }
```

</div>

</div>

Here we simply schedule the thread to the executor and then return without waiting. The exception handling is correctly implemented by using the `ComletableFuture::exceptionally` method.

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

### Detailed Explanation

<div class="iframe">

<div id="player">

</div>

<div class="player-unavailable">

# Đã xảy ra lỗi.

<div class="submessage">

Không thể chạy JavaScript.

</div>

</div>

</div>

Sourec code: <a href="https://bitbucket.org/axonivy-prod/demo-thread-pools/src" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/demo-thread-pools/src</a>

### More Examples

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

**BatchNormalizeService.java** (Original)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2d9ceba6-7a87-4c09-8e49-45e62cdaf011" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public void scanBatchProcessingAndCheck() {
        // Find all the Recipient Matching have normalizing status
        List<RecipientMatchingEntity> entitiesTobeScanned = recipientMatchingDao.findNormalizingRecipientMatching(MAX_SUPPORT_PROCESS_BATCH);
        entitiesTobeScanned.stream()
                .map(record -> executor.run(() -> processNormalizedForRecipientMatch(record)))
                .forEach(CompletableFuture::join);
        executor.run(() -> matchingRunService.processWaitForMatchingRecipientBatch());
    }
```

</div>

</div>

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

(Improved)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="27e9770a-dd37-4624-86ae-06f635427997" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public void scanBatchProcessingAndCheck() {
        // Find all the Recipient Matching have normalizing status
        List<RecipientMatchingEntity> entitiesTobeScanned = recipientMatchingDao.findNormalizingRecipientMatching(MAX_SUPPORT_PROCESS_BATCH);
        CompletableFuture<?>[] futures = entitiesTobeScanned.stream()
                .map(record -> executor.run(() -> processNormalizedForRecipientMatch(record)))
                .toArray(i -> new CompletableFuture<?>[i]);
        CompletableFuture.allOf(futures).thenRunAsync(
                () -> matchingRunService.processWaitForMatchingRecipientBatch(), 
                executor.getExecutorService());
    }
```

</div>

</div>

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

**IdentityMatchingService.java** (Original)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ea39d4c9-edad-4260-a0ae-3e7f02151954" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public void matchingAsynchronous(List<RecipientRequestBatch> recipientRequestBatches) {
  AbstractAsyncExecutor executor = getAppropriatePool(recipientRequestBatches);
  List<CompletableFuture<Void>> identityMatchingFutures = matchingAsynchronousWithExecutorAndLimit(executor,
      recipientRequestBatches, MATCHING_BATCH_LIMIT_PER_QUERY);
  identityMatchingFutures.forEach(CompletableFuture::join);
  recipientRequestBatches.stream().map(RecipientRequestBatch::getMatchingRunId).distinct()
      .collect(Collectors.toList()).forEach(matchingRunId -> matchingRunResultBuilder.updateMatchingRunStatusIfAllFinished(matchingRunId));
}
```

</div>

</div>

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

(Improved)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="50bb4e73-b58a-4d4b-8a3e-b0db2460b4a7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public void matchingAsynchronous(List<RecipientRequestBatch> recipientRequestBatches) {
  AbstractAsyncExecutor executor = getAppropriatePool(recipientRequestBatches);
  List<CompletableFuture<Void>> identityMatchingFutures = matchingAsynchronousWithExecutorAndLimit(executor,
      recipientRequestBatches, MATCHING_BATCH_LIMIT_PER_QUERY);
  CompletableFuture<Void> allMatches = CompletableFuture.allOf(identityMatchingFutures.toArray(new CompletableFuture<?>[identityMatchingFutures.size()]));
  allMatches.thenRunAsync(() -> {
    recipientRequestBatches.stream()
        .map(RecipientRequestBatch::getMatchingRunId)
        .distinct()
        .forEach(matchingRunId -> matchingRunResultBuilder.updateMatchingRunStatusIfAllFinished(matchingRunId));
  }, executor.getExecutorService()); // or maybe another thread pool here
}
```

</div>

</div>

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

**PhysicalDocumentSenderService.java** (Original)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6e90fc0b-ea75-4025-9ff3-46eca6dd6e34" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public CompletableFuture<Void> sendDocument(String senderTenantId, Token senderToken, List<DocumentDataCarrier> physicalDocuments, DeliveryContext deliveryContext) {
    CompletableFuture<?>[] futures = physicalDocuments.stream().map(documentDataCarrier ->
            documentSendingExecutor
                    .run(() -> deliverForEachDocument(senderToken, documentDataCarrier, deliveryContext))
                  Noo  .thenRun(() -> documentPostDeliveringUpdater
                            .updateDocumentAfterFinishingEachDocument(senderTenantId, senderToken, deliveryContext, documentDataCarrier))
                    .thenRun(() -> documentPostDeliveringUpdater
                            .deleteDocumentRecipientTrackingsAfterFinishingEachDocument(senderTenantId, senderToken, deliveryContext, documentDataCarrier)))
            .toArray(i -> new CompletableFuture<?>[i]);
    return CompletableFuture.allOf(futures);
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cb6bb25d-f05e-4d18-93c1-be25c72877cd" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
private void deliverForEachDocument(Token senderToken, DocumentDataCarrier documentDataCarrier, DeliveryContext deliveryContext) {
    //TODO: Should hold byte array before store temp in digital, will be refactored next sprint in new US
    try (InputStream inputStream = documentDataCarrier.getBinaryFileDataStream()) {
        documentDataCarrier.setBinaryFileData(IOUtils.toByteArray(inputStream));
        documentDataCarrier.setBinaryFileDataStream(null);
    } catch (Exception e) {
        LOGGER.log(Level.SEVERE, loggerMessageProducer
                .from(senderToken.getTenant().getTenantId(), deliveryContext.getDeliveryId(), documentDataCarrier.getDocumentId(),
                        documentDataCarrier.getDocumentMetadata().getSenderEndToEndId(),
                        "Could not obtain file data from request data"), e);
        documentDataCarrier.setDocumentDeliveryStatus(DocumentDeliveryStatus.FAILED_TO_RETRIEVE);
        return;
    }

    documentDataCarrier.getDocumentMetadata().getRecipients().stream().
            map(recipient -> documentRecipientSendingExecutor.run(() -> deliverForEachRecipient(senderToken, documentDataCarrier, recipient, deliveryContext)))
            .collect(Collectors.toList())
            .forEach(CompletableFuture::join);
    documentDataCarrier.setDocumentDeliveryStatus(getDocumentStatus(documentDataCarrier));

}
```

</div>

</div>

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

(Improved)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6a5090ea-17c9-4612-9ded-e13799ace847" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public CompletableFuture<Void> sendDocument(String senderTenantId, Token senderToken, List<DocumentDataCarrier> physicalDocuments, DeliveryContext deliveryContext) {
    CompletableFuture<?>[] futures = physicalDocuments.stream().map(documentDataCarrier ->
            deliverForEachDocument(senderToken, documentDataCarrier, deliveryContext)
                    .thenRunAsync(() -> documentPostDeliveringUpdater.updateDocumentAfterFinishingEachDocument(senderTenantId, senderToken, deliveryContext, documentDataCarrier),
                            documentSendingExecutor.getExecutorService())
                    .thenRunAsync(() -> documentPostDeliveringUpdater.deleteDocumentRecipientTrackingsAfterFinishingEachDocument(senderTenantId, senderToken, deliveryContext, documentDataCarrier),
                            documentSendingExecutor.getExecutorService()))
                    .toArray(i -> new CompletableFuture<?>[i]);
    return CompletableFuture.allOf(futures);
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a8b5d075-5ef9-4042-bd26-c0829ba8094f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
private CompletableFuture<Void> deliverForEachDocument(Token senderToken, DocumentDataCarrier documentDataCarrier, DeliveryContext deliveryContext) {
    //TODO: Should hold byte array before store temp in digital, will be refactored next sprint in new US
    try (InputStream inputStream = documentDataCarrier.getBinaryFileDataStream()) {
        documentDataCarrier.setBinaryFileData(IOUtils.toByteArray(inputStream));
        documentDataCarrier.setBinaryFileDataStream(null);
    } catch (Exception e) {
        LOGGER.log(Level.SEVERE, loggerMessageProducer
                .from(senderToken.getTenant().getTenantId(), deliveryContext.getDeliveryId(), documentDataCarrier.getDocumentId(),
                        documentDataCarrier.getDocumentMetadata().getSenderEndToEndId(),
                        "Could not obtain file data from request data"), e);
        documentDataCarrier.setDocumentDeliveryStatus(DocumentDeliveryStatus.FAILED_TO_RETRIEVE);
        return CompletableFuture.completedFuture(null);
    }

    return CompletableFuture.allOf(documentDataCarrier.getDocumentMetadata().getRecipients().stream().
                map(recipient -> documentRecipientSendingExecutor.run(() -> deliverForEachRecipient(senderToken, documentDataCarrier, recipient, deliveryContext)))
            .toArray(i -> new CompletableFuture[i]))
            // seems to be a cheap operation se we can run it in either thread
            // therefore we can use thenRun instead of thenRunAsync
            .thenRun(() -> documentDataCarrier.setDocumentDeliveryStatus(getDocumentStatus(documentDataCarrier)));
}
```

</div>

</div>

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

*note: this one isn’t as bad as* `deliverForEachDocument` *is called from a thread-pool anyway. But the method might be reused in the future so we need to make sure it handles its execution context correctly no matter the invocation context.*

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

**DigitalDocumentSendingService.java** (Original)

Sometimes refactoring is a bit harder as the async context must be passed all the way up. For example with have this method:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4eccb6e0-8e82-4028-acd3-2cec80efc8d2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
private void deliveryForEachDocument(String senderTenantId, Token token, DeliveryContext deliveryContext, 
                    MatchingResponse matchingResponse, DocumentDataCarrier documentDataCarrier) {
[...]
    ExecutorType executorType = ExecutorUtils.calculateAppropriateExecutorType(deliveryContext.getNumberOfDocument(), SMALL_POOL_LIMITATION, MEDIUM_POOL_LIMITATION);
    List<CompletableFuture<Void>> completableFutures = documentDataCarrier.getDocumentMetadata().getRecipients().stream()
            .map(recipient -> documentRecipientSendingExecutor.run(() -> {
                List<MatchedRecipient> multipleMatchedRecipients = matchedRecipients.get(recipient);
                deliverForSpecificRecipient(senderTenantId, token, deliveryContext, documentDataCarrier, recipient, multipleMatchedRecipients);
    }, executorType)).collect(Collectors.toList());
    // Make sure the delivery for the document completed before deleting
    // document from luz-docs. This caters for the case that it is failed to complete. Then,
    // we can retry this case
    completableFutures.forEach(CompletableFuture::join);
    deleteDocument(senderTenantId, token, deliveryContext, documentDataCarrier);
    documentDataCarrier.setDocumentDeliveryStatus(getDocumentStatus(documentDataCarrier));
}
```

</div>

</div>

Which passes its (implicit) synchronous nature all the way up DigitalDocumentSendingService::deliveryForEachDocument ← DigitalDocumentSendingService::sendDocument ← DocumentDeliveryService::sendDocument ← DocumentDeliveryService::importDocuments

Which has itself an async invocation:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="106c563d-407a-4f9b-8e85-3af2fe44f9ae" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public void importDocuments(String senderTenantId, Token token, List<Document> documents, long deliveryId,
                                DeliveryProcessingStatusTracking deliveryTracking) {
[...]
    ExecutorType executorType = ExecutorUtils.calculateAppropriateExecutorType(documents.size(), SMALL_POOL_LIMITATION, MEDIUM_POOL_LIMITATION);
    documentDataCarriers.stream().map(documentDataCarrier ->
            documentSendingExecutor.run(() -> sendDocument(senderTenantId, token, deliveryContext, documentDataCarrier), executorType)
            .thenRun(() -> documentPostDeliveringUpdater.updateDocumentAfterFinishingEachDocument(senderTenantId, token,
                    deliveryContext, documentDataCarrier))
            .thenRun(() -> documentPostDeliveringUpdater.deleteDocumentRecipientTrackingsAfterFinishingEachDocument(senderTenantId,
                 token, deliveryContext, documentDataCarrier)))
            .collect(Collectors.toList())
    .forEach(CompletableFuture::join);
[...]
```

</div>

</div>

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

(Improved)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a2a92aae-e39c-453c-8110-3a04e17729bc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
private CompletableFuture<Void> deliveryForEachDocument(String senderTenantId, Token token, DeliveryContext deliveryContext, 
                    MatchingResponse matchingResponse, DocumentDataCarrier documentDataCarrier) {
[...]
    ExecutorType executorType = ExecutorUtils.calculateAppropriateExecutorType(deliveryContext.getNumberOfDocument(), SMALL_POOL_LIMITATION, MEDIUM_POOL_LIMITATION);
    List<CompletableFuture<Void>> completableFutures = documentDataCarrier.getDocumentMetadata().getRecipients().stream()
            .map(recipient -> documentRecipientSendingExecutor.run(() -> {
                List<MatchedRecipient> multipleMatchedRecipients = matchedRecipients.get(recipient);
                deliverForSpecificRecipient(senderTenantId, token, deliveryContext, documentDataCarrier, recipient, multipleMatchedRecipients);
    }, executorType)).collect(Collectors.toList());
    // Make sure the delivery for the document completed before deleting
    // document from luz-docs. This caters for the case that it is failed to complete. Then,
    // we can retry this case
    return CompletableFuture.allOf(completableFutures.toArray(new CompletableFuture<?>[completableFutures.size()]))
        .thenRun(() -> {
            deleteDocument(senderTenantId, token, deliveryContext, documentDataCarrier);
            documentDataCarrier.setDocumentDeliveryStatus(getDocumentStatus(documentDataCarrier));
        });
}
```

</div>

</div>

Adding the async context, has some ugly consequences where we might have nested Futures that must be joined:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="191ef5a4-2f75-4049-9a92-02481ca62ca4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public void importDocuments(String senderTenantId, Token token, List<Document> documents, long deliveryId,
                            DeliveryProcessingStatusTracking deliveryTracking) {
[...]
    ExecutorType executorType = ExecutorUtils.calculateAppropriateExecutorType(documents.size(), SMALL_POOL_LIMITATION, MEDIUM_POOL_LIMITATION);
    documentDataCarriers.stream().map(documentDataCarrier ->
            {
                CompletableFuture<CompletableFuture<Integer>> future = documentSendingExecutor.supply(() -> sendDocument(senderTenantId, token, deliveryContext, documentDataCarrier), executorType);
                join(future).thenRun(() -> documentPostDeliveringUpdater.updateDocumentAfterFinishingEachDocument(senderTenantId, token,
                        deliveryContext, documentDataCarrier))
                .thenRun(() -> documentPostDeliveringUpdater.deleteDocumentRecipientTrackingsAfterFinishingEachDocument(senderTenantId,
                     token, deliveryContext, documentDataCarrier));
                return future;
            })
            .collect(Collectors.toList())
    .forEach(CompletableFuture::join);
[...]

public <T> CompletableFuture<T> join(CompletableFuture<CompletableFuture<T>> task) {
    return task.thenCompose(Function.identity());
}
```

</div>

</div>

Here we could also just use join() in the top-level thread but it’s probably better to retain the async context whenever possible.

On the other hand this also shows us that we have nested thread-pool invocations that might better be combined using thenApplyAsync etc. or poses the question whether it makes sense to create a new thread from an already asynchronous thread here.

(Note we call `join()` on the top-level here, so everything is blocking anyways, the also must be fixed as above.

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

There are overall 18 usages of CompletableFuture::join in the modules luz_eletter, luz_address_normalizer and luz_tenant_dir.

## Improvement 1: Distributed Processing of Batch-Jobs

Currently we have the following pattern for running long-running tasks. It consists of a k8s cronjob, a deployment providing a REST resource, a queue of pending work items (usually a table in a postgres database) and optionally a thread pool for processing multiple work items concurrently.


![[47091122608-cronjobpat.png]]



The k8s job looks something like this:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="da57792d-7d09-4e2c-b609-efcc8a3fbd46" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: batch/v1
kind: CronJob
metadata:
  name: some-cronjob
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: some-cronjob
            image: gcr.io/klara-repo/java8-with-common-libs
            args:
            - /bin/sh
            - -c
            - |
              # [...]
              curl -XGET "http://some-service:8080/demo/api/batch-jobs/parallel" \
                -H "Accept: application/json" \
                -H "Authorization: Bearer $LUZ_TOKEN"
          restartPolicy: OnFailure
```

</div>

</div>

### Improvement 1.1: Split the work

This has a few issues. First of all we can not make use of all available resources. This is especially important here as usually the batch jobs do heavy long-running work. With the configuration above the work will be done by a single randomly chosen pod instead of the whole service (i.e. all replicas). To mitigate this we add a message dispatcher which informs all available workers about the cron job execution.


![[47091122608-cronjobpatnew.png]]



To make sure the participants don’t work on the same items, they receive a mod value and a remainder value as query parameters in the request. These values can be used to spread the work equally over all pods. So the new job looks something like this:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e7248c7e-8a31-4567-876b-6c572a784d9a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
          containers:
          - name: some-cronjob
            image: gcr.io/klara-repo/java8-with-common-libs
            args:
            - /bin/sh
            - -c
            - |
              PORT=$(kubectl get endpoints some-servicee --namespace $NAMESPACE \
                -o jsonpath="{.subsets[*].ports[?(@.name == 'http')].port}")
              IPS=$(kubectl get endpoints demo-batch-jobs-service --namespace $NAMESPACE \
                -o jsonpath="{.subsets[*].addresses[*].ip}")
              MOD=$(printf '%s' "$IPS" | wc -w)
              REM=0

              for IP in $IPS; do
                curl -XGET "http://$IP:$PORT/demo/api/batch-jobs/parallel?m=$MOD&r=$REM" &
                     -H "Accept: application/json" \
                     -H "Authorization: Bearer $LUZ_TOKEN"
                PID=$!
                REM=$(expr $REM + 1)
              done
              
              for job in `jobs -p`; do
                  wait $job || echo "Failed $job"
              done
            env:
            - name: NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
```

</div>

</div>

Each worker will receive an HTTP request with a different remainder value. For example if we have 3 pods running we’ll have 3 requests such as:

- <a href="http://10.1.0.219:8080/demo/api/batch-jobs/parallel?m=3&amp;r=0&amp;t=50000" class="external-link" rel="nofollow">http://10.1.0.219:8080/demo/api/batch-jobs/parallel?m=3&amp;r=0</a>

- <a href="http://10.1.0.220:8080/demo/api/batch-jobs/parallel?m=3&amp;r=1&amp;t=50000" class="external-link" rel="nofollow">http://10.1.0.220:8080/demo/api/batch-jobs/parallel?m=3&amp;r=1</a>

- <a href="http://10.1.0.221:8080/demo/api/batch-jobs/parallel?m=3&amp;r=2&amp;t=50000" class="external-link" rel="nofollow">http://10.1.0.221:8080/demo/api/batch-jobs/parallel?m=3&amp;r=2</a>

In the service we use these values to only receive a part of the work items:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="814ac9f1-5be0-4c64-80d1-02b9507cf68d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT * FROM work_items WHERE status = `PENDING` AND id % $mod = $rem
```

</div>

</div>

### Improvement 1.2: Don’t repeat yourself or your work or do the same stuff over and over again or rerun the same jobs

By default a k8s will run concurrent cron jobs even if the previous one is still running. So if we schedule a cronjob every minute but the jobs have work for 4 minutes, after the third minute we will have 3 jobs working on the same work items. This is not only inefficient but depending on the implementation might lead to errors.


![[47091122608-cronjobissues.png]]



This behaviour can be overwritten using the `.spec.concurrencyPolicy` property. If we set it to `Forbid` no cronjob will be started if another one is already running. But this means that we are unable to scale the cron jobs if the load increases. So we start the first cronjob, it runs on one pod. The load is high, so CPU increases and k8s starts a second pod. A minute passes but the job on the first node is still running. Therefore no new job (which could spread the load over two nodes) is started until the first single pod has finished all the work.

With `Replace` we can have cronjob-2, stop cronjob-1 if it is still running. This is also not ideal, as cronjob-2 might have to repeat the work, that cronjob-1 started but could not finish in time.

Instead all cronjob should received an additional query parameter `timeout` which tells the service when it should stop processing items, finish its current batch of work and - if necessary - perform any clean up steps. So the request might look like this (having a polling interval of e.g. 1 minute, we set the timeout to 55 seconds to allow for some wiggle room):

- <a href="http://10.1.0.221:8080/demo/api/batch-jobs/parallel?m=3&amp;r=2&amp;t=50000" class="external-link" rel="nofollow">http://10.1.0.221:8080/demo/api/batch-jobs/parallel?m=3&amp;r=2&amp;timeout=55000</a>

An the cronjob executor checks this time like this:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dd901293-7f77-4c79-b44b-f7cbe79da21c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@GET
@Path("shared")
public Response shared(@QueryParam("r") int remainder, @QueryParam("m") int modulo, 
            @QueryParam("timeout") int timeout) {
    long timeoutAt = System.currentTimeMillis() + timeout;
    
    List<WorkItem> items = getItems(remainder, modulo);
    Iterator<WorkItem> itr = items.iterator();
    List<WorkItem> completedItems = new ArrayList<>();
    while (itr.hasNext() && System.currentTimeMillis() < timeoutAt) {
        WorkItem work = itr.next();
        worker.workOn(work);
        work.setStatus(WorkStatus.DONE);
        queue.update(work);
        completedItems.add(work);
    }
    // migh run in a thread pool, so response can
    // be returned immediately afterwards
    postProcess(completedItems);
    
    return Response.noContent().build();
}
```

</div>

</div>

Still, every cronjob should explicitly define the concurrency policy, as simply leaving it on the default value can be problematic. (Out of 36 CronJob in Klara at the time of writing only 10 specify a concurrency policy).

### Improvement 1.3: Enable scaling

The two improvements mentioned above also allow us, to scale our services based on the load caused by our batch jobs. If we have a single long running batch job on a single pod, no amount of scaling can improve the run time of the job. Additionally we’d see very inconsistent response times from the other services running on the bad. The clients unlucky enough to be routed to the node the received that batch job request might experience long wait times, while others will receive responses as quick as we hope. This makes provisioning reasonable resource limits for the pods pretty much impossible.

By having (1) the work shared by all pods and (2) run the jobs with a timeout, we can continually adapt to the demand.


![[47091122608-cronjobcpuscalepods.png]]



### Appendix A: A Comparison

Below you see three implementations of a cronjob and how long they took to process 200 work items with a processing time of 200-500 ms:


![[47091122608-compare.png]]



Green = CronJob on a single pod (no parelelism)  
Purple = CronJob running on three pods concurrently (no parallelism)  
Yellow = CronJob running on three pods concurrently with 4 threads in parallel on each pod.

<a href="https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/#concurrency-policy" class="external-link" data-card-appearance="inline" rel="nofollow">https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/#concurrency-policy</a>

</div>

</div>

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Blocking on CompletableFuture.get in a custom pool recreates the bottleneck]]
- [[Batching Design]]
- [[Enhance performance - Research on Parallel]]
- [[Performance pain points]]
- [[Luz Batch TypeScript - Sequence Diagram]]

%% ai-graph-end %%