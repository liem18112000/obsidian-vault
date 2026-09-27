---
title: "Using Google PubSub for events between Serverless workflow and Data Index"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48423174389/Using+Google+PubSub+for+events+between+Serverless+workflow+and+Data+Index
space: "FUT"
topic: infra
relevance: 0.8
depth: 2.9
updated: 2025-04-01
attachments: 1
tags:
  - confluence
  - infra
  - space/fut
---

# Using Google PubSub for events between Serverless workflow and Data Index

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-04-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48423174389/Using+Google+PubSub+for+events+between+Serverless+workflow+and+Data+Index)
> Relevance 0.8 · topic `infra`

## Overview


![[48423174389-pubsub-20250325-083018.png]]



## Implementation notes

Basic configuration for Google PubSub:

- on Serverless workflow:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5ace3dae-6906-4259-98c9-b44e5ad74279" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
quarkus.google.cloud.service-account-location=/deployments/pubsub-adminsdk-key.json
quarkus.google.cloud.project-id=${GCP_PUBSUB_PROJECT_ID}
workflow.events.topic=epost-workflow-events
```

</div>

</div>

- on Data index:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="421f4d40-39fb-4ab7-a9c8-70c3e828c42e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#GCP
quarkus.google.cloud.service-account-location=/deployments/pubsub-adminsdk-key.json
quarkus.google.cloud.project-id=${GCP_PROJECT_ID}
gcp.pubsub.workflow.events.subscription.name=pubsub-subscription-dev-epost-workflow-events
```

</div>

</div>

In order to support send and receive a new event type.

### On Serverless workflow

1.  Add a new outgoing channel in `application.properties`:  
    Ex:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0b68704a-799c-453f-a054-f931e67585e7" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    mp.messaging.outgoing.kogito-processinstances-events.connector=quarkus-http
    mp.messaging.outgoing.kogito-processinstances-events.url=http://localhost:${quarkus.http.port}${quarkus.http.root-path}/workflow-events/kogito-processinstances-events
    mp.messaging.outgoing.kogito-processinstances-events.topic=${workflow.events.topic}
    ```

    </div>

    </div>

2.  Add a new enum value in `WorkflowEventChannel.java` that match the channel key above:  
    Ex:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2f3b603f-af7c-4f47-8551-10110c5a0764" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    public enum WorkflowEventChannel {
        KOGITO_PROCESSINSTANCES_EVENTS("kogito-processinstances-events"),
        ...
    }
    ```

    </div>

    </div>

3.  Add a new REST API in `WorkflowEventResource.java` to handle the new event:  
    Ex:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="910db96b-b23d-4b16-a6b6-23f2df71fbd7" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    @POST
    @Blocking
    @Path("/kogito-processinstances-events")
    public void kogitoProcessInstancesEvents(CloudEventV1 event) {
        pubsubPublisherService.publishEvent(WorkflowEventChannel.KOGITO_PROCESSINSTANCES_EVENTS, event);
    }
    ```

    </div>

    </div>

### On Data Index

1.  Add a new outgoing channel in `application.properties`:  
    Ex:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="22da2989-6e21-40a4-be19-f80afc3ba41b" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    #Events
    mp.messaging.incoming.kogito-processinstances-events.connector=quarkus-http
    mp.messaging.incoming.kogito-processinstances-events.path=/processInstancesChannel
    ```

    </div>

    </div>
