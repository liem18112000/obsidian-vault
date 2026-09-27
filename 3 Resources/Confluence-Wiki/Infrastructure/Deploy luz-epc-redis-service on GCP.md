---
ai_hash: 0303b39efc1f54ba
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.879
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48950706199/Deploy+luz-epc-redis-service+on+GCP
space: Helios
status: reference
tags:
- confluence
- infra
- space/helios
title: Deploy luz-epc-redis-service on GCP
topic: infra
type: source
updated: 2025-12-09
---

# Deploy luz-epc-redis-service on GCP

> [!info] Imported from Confluence
> Space **Helios** · updated 2025-12-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48950706199/Deploy+luz-epc-redis-service+on+GCP)
> Relevance 0.879 · topic `infra`

- Create redis instance on Memorystore:

  - Run script `create_redis_instance.sh`

  - Run this command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="997924ee-33bf-4828-a204-4b46e7187665" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
bash ./cluster/gcp/create_redis_instance.sh $LUZ_CLUSTER_CONFIG $LUZ_ENV_CONFIG
```

</div>

</div>

For example: On dev-vn

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="465e00e6-90b3-46a7-89cc-b0fa17fca92e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
bash ./cluster/gcp/create_redis_instance.sh gcp-dev-vn dev-vn
```

</div>

</div>

After script done:


![[48950706199-image-20251209-073744.png]]

![[48950706199-image-20251209-074011.png]]



- Prepare the environment

  - Run this command to get the authString:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="424584bd-78dc-4423-82c5-1a6e1ce3fe13" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    gcloud beta redis instances get-auth-string <redis-instance-name> --project=klara-nonprod --region=europe-west6 --format="value(authString)
    ```

    </div>

    </div>

  - Create an env file with the value authString:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="743f296c-2d78-46b8-95d3-164cff973cb8" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    QUARKUS_REDIS_PASSWORD=<authString here>
    ```

    </div>

    </div>

  - Run this command to create the secret file

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="71af3a22-6df3-4986-93b3-a1a779ed5f12" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker run -it -v <PATH TO LUZ_KUBERNETES>:/root/development/luz_kubernetes europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-deploy:0.0.2 bash
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ae5ec714-e6d3-49eb-9f31-9e4d58053c92" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd root/development/luz_kubernetes/
cd sops/scripts
sed -i -e 's/\r$//' luz-epc-redis-service-create-env-secret.sh
./luz-epc-redis-service-create-env-secret.sh dev ../../kubernetes-overlays/env-dev/luz-epc-redis-service/.env
```

</div>

</div>

- Deploy on GCP following step below

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="93e326cf-6ae6-4410-93f9-2d6c2e8eee79" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker run -it -v <PATH TO LUZ_KUBERNETES>:/root/development/luz_kubernetes europe-west6-docker.pkg.dev/klara-repo/artifact-registry-container-images/luz-deploy:0.0.2 bash
cd root/development/luz_kubernetes/
sed -i -e 's/\r$//' ./deploy_to_stdout.sh
sed -i -e 's/\r$//' kustomize-plugins/kustomize/plugin/klara.ch/v1/sopssecretgenerator/SopsSecretGenerator
./deploy_to_stdout.sh dev > deployment.yaml
exit
kubectl apply -l klara.ch/module=luz-epc-redis -f deployment.yaml -n dev
kubectl apply -l klara.ch/module=luz-epc-redis-service -f deployment.yaml -n dev
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[EPC Notification]]
- [[Setup Redis and DNS on TEST and PROD]]
- [[Kubernetes knowledge]]
- [[Recipe Deploy with Terraform]]
- [[luz-vault - How to run Vault Benchmark]]

%% ai-graph-end %%