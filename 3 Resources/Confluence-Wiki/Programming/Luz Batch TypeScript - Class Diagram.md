---
title: "Luz Batch TypeScript - Class Diagram"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48719364144/Luz+Batch+TypeScript+-+Class+Diagram
space: "FUT"
topic: programming
relevance: 0.746
depth: 2.57
updated: 2025-10-08
attachments: 2
tags:
  - confluence
  - programming
  - space/fut
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
