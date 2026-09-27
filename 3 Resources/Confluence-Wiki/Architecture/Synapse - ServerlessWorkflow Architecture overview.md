---
title: "[Synapse - ServerlessWorkflow] Architecture overview"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48910631026/Synapse+-+ServerlessWorkflow+Architecture+overview
space: "FUT"
topic: architecture
relevance: 0.81
depth: 2.67
updated: 2025-11-26
attachments: 1
tags:
  - confluence
  - architecture
  - space/fut
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
