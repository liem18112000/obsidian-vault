---
ai_hash: 40342534d1a780a4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.67
entities: []
relevance: 0.81
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48910631026/Synapse+-+ServerlessWorkflow+Architecture+overview
space: FUT
status: reference
tags:
- confluence
- architecture
- space/fut
title: '[Synapse - ServerlessWorkflow] Architecture overview'
topic: architecture
type: source
updated: 2025-11-26
---

# [Synapse - ServerlessWorkflow] Architecture overview

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-11-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48910631026/Synapse+-+ServerlessWorkflow+Architecture+overview)
> Relevance 0.81 · topic `architecture`

Reference <a href="https://github.com/serverlessworkflow/synapse?tab=readme-ov-file#architecture" class="external-link" data-card-appearance="inline" data-local-id="fafd4b16-9efd-468e-bc86-3b541de78f6c" rel="nofollow">https://github.com/serverlessworkflow/synapse?tab=readme-ov-file#architecture</a>

# Component overview


![[48910631026-synapse-architecture-overview-latest.png]]



# What is good

- Microservices Separation (API, Operator, Correlator, Runner)

- Ephemeral Runners (shared-nothing, perfect isolation)

- Event-Driven

- Optimistic Concurrency Control (e.g. version-based, no locks)

- Separate document data from process data (e.g. workflow definition, workflow instance, document (input/output, context))

- Native resource watching in database (e.g. Redis with Redis pubsub) (*consider perfomance impact*)

%% ai-graph-start %%

**Related notes:**
- [[Synapse - ServerlessWorkflow Database analysis]]
- [[Architecture Design]]
- [[Borrow the Kubernetes resource shape for objects in a schemaless store]]
- [[POC SecuredMail Serverless Workflow]]
- [[Batching Design]]

%% ai-graph-end %%