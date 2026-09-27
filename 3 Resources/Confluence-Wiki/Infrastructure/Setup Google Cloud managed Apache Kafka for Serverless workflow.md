---
ai_hash: fe232af72a366beb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.87
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48406003792/Setup+Google+Cloud+managed+Apache+Kafka+for+Serverless+workflow
space: FUT
status: reference
tags:
- confluence
- infra
- space/fut
title: Setup Google Cloud managed Apache Kafka for Serverless workflow
topic: infra
type: source
updated: 2025-03-17
---

# Setup Google Cloud managed Apache Kafka for Serverless workflow

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-03-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48406003792/Setup+Google+Cloud+managed+Apache+Kafka+for+Serverless+workflow)
> Relevance 0.87 · topic `infra`

## Required resources

1.  User role: Managed Kafka Admin, Service Account Key Admin

2.  Google API: Managed Service for Apache Kafka

## Resources to be created.

1.  Regional Kafka Cluster per GCP project with minimum configuration, we can increase it later:

    1.  3 CPUs

    2.  4 Gi of RAM per CPU

    3.  1 replicas  
        <a href="https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/managed_kafka_cluster" class="external-link" rel="nofollow">Terraform: managed_kafka_cluster</a>

2.  IAM service account that has ‘Managed Kafka Client’ role assigned.  
    Terraform: <a href="https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/google_service_account" class="external-link" rel="nofollow">google_service_account</a>  
    And, create a credential JSON of that service account.  
    Ex:  
    `gcloud iam service-accounts keys create managed-kafka-sa-credential.json --iam-account=dev-managed-kafka-client@klara-nonprod.iam.gserviceaccount.com`

3.  Create a environment variable `KAFKA_SASL_JAAS_CONFIG` on both `epost-serverless-workflow` and '`serverless-workflow-data-index`' to configure them to connect to managed Kafka instance. The value should be encrypted as a **Kubernetes secret**:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1547ae88-4d53-4538-b442-42e4b9d9beef" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    KAFKA_SASL_JAAS_CONFIG=org.apache.kafka.common.security.plain.PlainLoginModule required username="service account email" password="base64 encoded content of service account credential JSON file";
    ```

    </div>

    </div>

    \- Configure other SASL params and topics per env for `epost-serverless-workflow` in ConfigMap:  
    Example on dev:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="846924a8-a51f-4ea5-a94b-0a0d2e16cc98" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    MP_MESSAGING_OUTGOING_KOGITO_PROCESSINSTANCES_EVENTS_TOPIC: dev-kogito-processinstances-events
    MP_MESSAGING_OUTGOING_KOGITO_USERTASKINSTANCES_EVENTS_TOPIC: dev-kogito-usertaskinstances-events
    MP_MESSAGING_OUTGOING_KOGITO_JOBS_EVENTS_TOPIC: dev-kogito-jobs-events
    MP_MESSAGING_OUTGOING_KOGITO_VARIABLES_EVENTS_TOPIC: dev-kogito-variables-events
    MP_MESSAGING_OUTGOING_KOGITO_PROCESSDEFINITIONS_EVENTS_TOPIC: dev-kogito-processdefinitions-events

    KAFKA_BOOTSTRAP_SERVERS: bootstrap.<cluster-name>.europe-west6.managedkafka.klara-nonprod.cloud.goog:9092
    KAFKA_SECURITY_PROTOCOL: SASL_SSL
    KAFKA_SASL_MEHCANISM: PLAIN
    ```

    </div>

    </div>

    \- Configure topic per env for `serverless-workflow-data-index` in ConfigMap:  
    Example on dev:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c37012e0-94dc-4e1e-8eed-b8079f151983" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    MP_MESSAGING_INCOMING_KOGITO_PROCESSINSTANCES_EVENTS_TOPIC: dev-kogito-processinstances-events
    MP_MESSAGING_INCOMING_KOGITO_USERTASKINSTANCES_EVENTS_TOPIC: dev-kogito-userstaskinstances-events
    MP_MESSAGING_INCOMING_KOGITO_JOBS_EVENTS_TOPIC: dev-kogito-jobs-events

    KAFKA_BOOTSTRAP_SERVERS: bootstrap.<cluster-name>.europe-west6.managedkafka.klara-nonprod.cloud.goog:9092
    KAFKA_SECURITY_PROTOCOL: SASL_SSL
    KAFKA_SASL_MEHCANISM: PLAIN
    ```

    </div>

    </div>

%% ai-graph-start %%

**Related notes:**
- [[Using Google PubSub for events between Serverless workflow and Data Index]]
- [[Deploy luz-epc-redis-service on GCP]]
- [[GCP - Connect Database]]
- [[Deployment with terraform]]
- [[EPC Notification]]

%% ai-graph-end %%