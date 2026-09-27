---
ai_hash: d514432af4c655d9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.57
entities: []
relevance: 0.746
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48719364144/Luz+Batch+TypeScript+-+Class+Diagram
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: Luz Batch TypeScript - Class Diagram
topic: programming
type: source
updated: 2025-10-08
---

# Luz Batch TypeScript - Class Diagram

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-10-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48719364144/Luz+Batch+TypeScript+-+Class+Diagram)
> Relevance 0.746 · topic `programming`

![[48719364144-luz-batching-typescript-class-diagram-full.png]]



## Key Relationships

### Composition and Aggregation

- **AsyncHttpProcessor** contains:

  - `BackgroundBatchProcessor` (composition)

  - `AsyncBatchErrorHandler` (composition)

  - References to `LuzBatchingClient` and `AsyncLogger` (aggregation)

- **BackgroundBatchProcessor** contains:

  - `AsyncBatchErrorHandler` (composition)

  - References to `LuzBatchingClient` and `AsyncLogger` (aggregation)

### Implementation

- **MongoDBLuzBatchingClient** implements `LuzBatchingClient` interface

- **ConsoleLogger** implements `AsyncLogger` interface

### Usage Patterns

1.  **AsyncHttpProcessor** is an abstract class that must be extended by concrete implementations

2.  Concrete processors must implement `processBatchItems()` method

3.  Optional methods: `sendIndividualCallback()` and `pollBatchResults()` for different processing modes

%% ai-graph-start %%

**Related notes:**
- [[Luz Batch TypeScript - Sequence Diagram]]
- [[Batch Processor Library - NodeJS]]
- [[Batching Design]]
- [[Recipe Luz Batch TypeScript]]
- [[Luz Batch TypeScript - Configuration]]

%% ai-graph-end %%