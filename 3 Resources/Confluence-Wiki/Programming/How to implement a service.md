---
ai_hash: 760ff686486c70f1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 30
depth: 2.84
entities: []
relevance: 0.804
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47973925191/How+to+implement+a+service
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: How to implement a service
topic: programming
type: source
updated: 2024-12-04
---

# How to implement a service

> [!info] Imported from Confluence
> Space **FUT** · updated 2024-12-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47973925191/How+to+implement+a+service)
> Relevance 0.804 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6" macro-id="ba5047a4-f94b-46b6-9511-d6e353671b24" macro-name="toc" numberedoutline="false" structure="list">

</div>

To implement a cloud run service you can chose to either implement it using Java or Typescript/Javascript. Below is a way to chose which one is the suitable one

More **CPU bound tasks** (pdf conversion, extract text from pdf, image processing): consider using **Java**  
More **I/O bound** tasks (call to another module to get result): consider using **Typescript/Javascript**.

# Java

## Generate a skeleton

Using devportal to generate a service.

Note: you can choose to publish it to Bitbucket or to download it directly to your local.

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[47973925191-generate_java_template.mp4|generate_java_template.mp4]]</span>

## Code structure

(The same for both Java and Typescript)

The structure of the project is describing directly in the README.md file in the generated project.

### Pubsub message

[Pubsub convention](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48024322097/Pubsub+convention)

### Http request

The request body of the http request should look like this. Where

- `businessData`: data needed for processing business of the cloud run service

Note: `idempotency id`, `token `and `refresh token` are providing via headers of the request.


![[47973925191-image-20240808-014106.png]]



 

### Id<span class="inline-comment-marker" ref="75c76fec-5abb-44c5-9e94-50ef9dfe8019">empotenc</span>y

In a system which using pubsub or even with http call. If there is no mechanism to avoid process request/event only once. Then, one event/request might be processed multiple times, o<span class="inline-comment-marker" ref="3f70798d-611b-4c9d-b71d-94ee7c98226d">r concurrently or even race condition.</span>


![[47973925191-image-20240812-102348.png]]



In Common Service Architecture, we would like to prevent that situation happens. Then, the mechanism of idempotency introduced. The first step of idempotency is to solve race condition situation. We want to guarantee one event can only be processed once at a time.


![[47973925191-Common service architecture - Event Flowchart-1-20240806-103607.png]]



This mechanism **already implemented** in the template on devportal so you don’t need to do anything 

![[47973925191-smile.png]]



## What needs to be implemented

The general flow of cloud run service should be

(The same for both Java and Typescript)


![[47973925191-image-20240808-040959.png]]



To implement the flow you have to fulfill some places **marked with todo** in the generated project.

### Error and Exception handling

To map error and exception from technical to things that are meaningful there are mappers (`LocalizedRuntimeExceptionMapper` and `RestClientExceptionMapper`) to do that

### Tests

Tests including UT and ITs have already implemented for services/resources of the skeleton. Please add more tests as you implement business for the service.

### Fault t<span class="inline-comment-marker" ref="ca8efcfc-5f56-4062-b2e8-5379757f6e08">olerance</span>

(The same for both Java and Typescript)

In the generated project there are 2 patterns of fault tolerance already implemented they are retry and timeout. However, they are implemented with sample configuration values. Then, you have to adjust these configurations to suite your need.

Builkhead already handles by cloud run

As calling by http request to cloud run service the orchestrator must handle some errors that might throw from the cloud run service. Which includes

- <a href="https://cloud.google.com/run/docs/troubleshooting#abort-request" class="external-link" rel="nofollow">500 error code</a>: no available instance because no instance is ready to serve request

- <a href="https://cloud.google.com/run/docs/troubleshooting#429-max-instances" class="external-link" rel="nofollow">429 error code</a>: no available instance because all instances are busy, request can not routed to any instance

- <a href="https://cloud.google.com/run/docs/troubleshooting#timeout-504" class="external-link" rel="nofollow">504 error code</a>: gateway timeout, request has routed to instance but reached timeout

<a href="https://cloud.google.com/run/docs/troubleshooting" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/run/docs/troubleshooting</a>

# Typescript

## <span class="inline-comment-marker" ref="69eeb844-7e28-432c-baff-1729e4445053">Generate a skeleton</span>

Using devportal to generate a service.

Note: you can choose to publish it to Bitbucket or to download it directly to your local.

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[47973925191-generate_typescript_template.mp4|generate_typescript_template.mp4]]</span>

## What needs to be implemented

To implement the flow you have to fulfill some places **marked with todo** in the generated project.

### Error and Exception handling

To map error and exception from technical to things that are meaningful the `ErrorMapper` introduced to support that.

### Tests

Tests including UT and ITs have already implemented for services/resources of the skeleton. Please add more tests as you implement business for the service.

%% ai-graph-start %%

**Related notes:**
- [[Common service architecture]]
- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]
- [[Recipe Best practices implementing scalable distributed applications on cloud]]
- [[Batching Design]]
- [[Luz Batch TypeScript - Sequence Diagram]]

%% ai-graph-end %%