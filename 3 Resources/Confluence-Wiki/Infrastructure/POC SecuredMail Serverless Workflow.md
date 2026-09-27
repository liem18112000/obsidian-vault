---
ai_hash: ac26af04d2a5676f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 17
depth: 2.38
entities: []
relevance: 0.706
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48220536889/POC+SecuredMail+Serverless+Workflow
space: FUT
status: reference
tags:
- confluence
- infra
- space/fut
title: POC SecuredMail Serverless Workflow
topic: infra
type: source
updated: 2025-03-19
---

# POC SecuredMail Serverless Workflow

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-03-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48220536889/POC+SecuredMail+Serverless+Workflow)
> Relevance 0.706 · topic `infra`

![[48220536889-SecureMail serverless workflow.png]]



# Deployment overview


![[48220536889-POC SecureMail serverless workflow - Deployment Overview.png]]




![[48220536889-image-20250206-034953.png]]



# Migration process

## Dev mode

To differentiate the process using service map or serverless workflow, check this condition when

- start a service map

- publish the event to trigger next service

To differentiate the source whether from message data (mongodb) or workflow data (data index) for the message status and service history on the monitoring screen

## Track workflow instance id

Store the workflow instance id immediately after starting a workflow into message data.

To map the workflow instances to messages, to support showing the workflow data (status, service path,..) on the monitoring screen.

## Service adapter layer

This layer is required to translate the workflow input data into the business service’s input.

As workflow data input is only pass message id to the service but the current implementation expects an cloud event with full message data.


![[48220536889-service-adapter.drawio.png]]



# Consideration

## Final status

The final status can be getting from either

- The status of the workflow (serverless workflow)

- The status of the last service.

We decided to use the status of the workflow because it’s consistent and managed by serverless workflow. Then, we don’t need to have any kind of state machine to handle it. Then, it leads to one concern that should it be **synced** to the message?

# ~~What’s next?~~

- ~~Persist business data~~

  - ~~<span class="inline-comment-marker" ref="28a8a4e8-b94d-415a-a594-648351b63e85">Differentiate workflow data and business data</span>.~~

  - ~~Set up mongodb.~~

  - ~~Implement logic to persist business data.~~

- ~~Clean up SecureMail workflow data~~

  - ~~Only store the necessary data for orchestration.~~

- ~~Monitor the workflow using Data Index~~

  - ~~Statistics.~~

  - ~~The path of the flow, to know which steps are executed, and the status of each step.~~

%% ai-graph-start %%

**Related notes:**
- [[Architecture Design]]
- [[Synapse - ServerlessWorkflow Architecture overview]]
- [[Synapse - ServerlessWorkflow Database analysis]]
- [[Architecture]]
- [[Unified architecture overview]]

%% ai-graph-end %%