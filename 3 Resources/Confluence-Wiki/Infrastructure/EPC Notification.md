---
ai_hash: 3d8aceca07ad33ff
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.86
entities: []
relevance: 0.708
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49104125959/EPC+Notification
space: Helios
status: reference
tags:
- confluence
- infra
- space/helios
title: EPC Notification
topic: infra
type: source
updated: 2026-05-27
---

# EPC Notification

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-05-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49104125959/EPC+Notification)
> Relevance 0.708 · topic `infra`

To be updated…

Related: [Deploy luz-epc-redis-service on GCP](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48950706199/Deploy+luz-epc-redis-service+on+GCP)

GCP instances:

- Update env.sh (Team can do it when we’re ready release)

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f83a3a0e-070d-4130-83ff-d0c3310e60b1" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  export REDIS_INSTANCE_VERSION_7_2=redis_7_2
  export EPC_REDIS_DNS_NAME_PREFIX=luz-epc-redis
  export EPC_REDIS_INSTANCE_ID=luz-epc-redis-{env}
  ```

  </div>

  </div>

- Redis memory (instance)

  - Create redis instance (more reference [Deploy luz-epc-redis-service on GCP](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48950706199/Deploy+luz-epc-redis-service+on+GCP) )

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9ba32849-f6c6-4f1c-b46f-1fba1704f893" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    ./cluster/create_redis_instance.sh <gcp-project> <environment>
    ```

    </div>

    </div>

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f65f9a07-153e-4cde-85d4-5d999bbaeab3" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # example on dev
    ./cluster/gcp/create_redis_instance.sh gcp-nonprod dev
    ```

    </div>

    </div>

    Expect to have a new redis instance like this

    

![[49104125959-image-20260202-071914.png]]



  - Access into the redis instance and copy the “Auth String” to generate secret for luz-epc-redis-service

- Google pub/sub

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="96f2b235-be5c-4f42-a589-085664f2a532" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  ./create_epc_notification_pubsub_environment.sh gcp-nonprod dev
  ./create_epc_ws_gateway_pubsub_environment.sh gcp-nonprod dev
  ```

  </div>

  </div>

  - Those scripts will prepare topics/subscriptions that being used by luz-epc-ws-gateway, luz-epc-notification

Services/Modules

- luz-epc-ws-gateway (Team can do it when we’re ready release)

  - system.properties

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="36299f66-f7e8-41af-ba71-75bb1f925c57" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    LUZ_EPC_WS_GATEWAY_PUBSUB_TOPIC_ID=<environment>-epc-ws-gateway-topic
    LUZ_EPC_WS_GATEWAY_PUBSUB_SUBSCRIPTION_ID=<environment>-epc-ws-gateway-subscription
    LUZ_GCP_PROJECT_ID=klara-nonprod
    ```

    </div>

    </div>

- luz-epc-redis-service

  - system.properties

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="de2da34b-7185-4160-9800-533b62d4a36a" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    QUARKUS_REDIS_HOSTS=redis://luz-epc-redis:6379
    ```

    </div>

    </div>

  - secret: the Auth String value can find on redis instance, or use this command

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="59e59126-3a21-4f50-8020-6cbac61aad55" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    gcloud beta redis instances get-auth-string <redis-instance-name> --project=klara-nonprod --region=europe-west6 --format="value(authString)"
    ```

    </div>

    </div>

    - Then create env file

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="81c8ae00-65cf-40b1-a9d6-ff7718899204" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      QUARKUS_REDIS_PASSWORD=<authString here>
      ```

      </div>

      </div>

    - Then use this script to generate secret file

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="854e55b5-bc0b-4caf-874c-4dd4efd19e20" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      # luz-epc-redis-service-create-env-secret.sh
      ```

      </div>

      </div>

- luz-epc-notification

  - system.properties

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e4072e78-8d01-4c34-9ef5-e2991c2754ae" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    LUZ_EPC_NOTIFICATION_PUBSUB_TOPIC_ID=dev-epc-notification-topic
    LUZ_EPC_NOTIFICATION_PUBSUB_SUBSCRIPTION_ID=dev-epc-notification-subscription
    LUZ_GCP_PROJECT_ID=klara-nonprod
    ```

    </div>

    </div>

%% ai-graph-start %%

**Related notes:**
- [[Deploy luz-epc-redis-service on GCP]]
- [[ELM5 PubSub Message Queue]]
- [[Setup Redis and DNS on TEST and PROD]]
- [[Luz Kubernetes Terraform]]
- [[Infrastructure]]

%% ai-graph-end %%