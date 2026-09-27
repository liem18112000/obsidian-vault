---
title: "[Migrate GKE to CloudRun] - Gradually migrate the luz-antivirus to Cloud Run"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49224646657/Migrate+GKE+to+CloudRun+-+Gradually+migrate+the+luz-antivirus+to+Cloud+Run
space: "LUZ"
topic: infra
relevance: 0.855
depth: 3
updated: 2026-03-11
attachments: 9
tags:
  - confluence
  - infra
  - space/luz
---

# [Migrate GKE to CloudRun] - Gradually migrate the luz-antivirus to Cloud Run

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-03-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49224646657/Migrate+GKE+to+CloudRun+-+Gradually+migrate+the+luz-antivirus+to+Cloud+Run)
> Relevance 0.855 · topic `infra`

<div hasbody="true" macro-id="55c0a00c-c487-4931-a792-4dd4c76e0571" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

We are shifting the `luz-antivirus` traffic from GKE to Cloud Run using `Service Mesh Istio Route`.


![[49224646657-image-20260311-103808.png]]



Migration Plan Visualization: <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="c92e37a9-c63a-4e19-9e96-57811a657f80" macro-name="view-file"><a href="../_attachments/49224646657-2026-01-27-antivirus-lb-mesh.drawio.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/49224646657/2026-01-27-antivirus-lb-mesh.drawio.pdf?version=1&amp;modificationDate=1773225649111&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[49224646657-2026-01-27-antivirus-lb-mesh.drawio.pdf]]

</a></span>

</div>

</div>

# Step 1 – Deploy the `luz-antivirus` Cloud Run service individually *(\*\*skip this step if all Cloud Run services are already redeployed)*.

Run the following command

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fba431c0-45a6-46f6-8cb1-b40c6949a1e4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./deploy_terraform.sh <env> --target=module.luz-antivirus

# for TEST
./deploy_terraform.sh test --target=module.luz-antivirus

# for PROD
./deploy_terraform.sh prod --target=module.luz-antivirus
```

</div>

</div>


![[49224646657-image-20260223-073538.png]]



------------------------------------------------------------------------

# Step 2 – Configure the Service Mesh Istio route for PROD and TEST

Retrieve the Cloud Run service URL by running the following command

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b7145d32-cb8e-4b8b-8bd2-f616d9730356" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
read s p <<< "$(gcloud run services describe <SERVICE_NAME> --region <REGION> --project <PROJECT_ID> --format='value(metadata.name,metadata.namespace)')" && echo "${s}-${p}.<REGION>.run.app"

# for TEST
read s p <<< "$(gcloud run services describe test-luz-antivirus --region europe-west6 --project klara-nonprod --format='value(metadata.name,metadata.namespace)')" && echo "${s}-${p}.europe-west6.run.app"

# for PROD
read s p <<< "$(gcloud run services describe prod-luz-antivirus --region europe-west6 --project klara-prod --format='value(metadata.name,metadata.namespace)')" && echo "${s}-${p}.europe-west6.run.app"
```

</div>

</div>


![[49224646657-image-20260311-083629.png]]



In the `luz-kubernetes` repository, replace the `REPLACE_WITH_CLOUDRUN_HOSTNAME` placeholder in these files with the returned URLs:

- <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-test/luz-antivirus/istio-cloudrun-routing.yaml" class="external-link" rel="nofollow">kubernetes-overlays\env-test\luz-antivirus\istio-cloudrun-routing.yaml</a>

- <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-prod/luz-antivirus/istio-cloudrun-routing.yaml" class="external-link" rel="nofollow">kubernetes-overlays\env-prod\luz-antivirus\istio-cloudrun-routing.yaml</a>


![[49224646657-image-20260311-085435.png]]



------------------------------------------------------------------------


![[49224646657-image-20260311-103138.png]]



<div class="panel conf-macro output-block" hasbody="true" macro-id="" macro-name="panel" style="background-color: #EAE6FF;border-color: #998DD9;border-width: 1px;">

<div class="panelContent" style="background-color: #EAE6FF;">

# How to adjust the Service Mesh Istio Weighted Routing?

In the `luz-kubernetes` repository, modify the following files:

- <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-test/luz-antivirus/istio-cloudrun-routing.yaml" class="external-link" rel="nofollow">kubernetes-overlays\env-test\luz-antivirus\istio-cloudrun-routing.yaml</a>

- <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-prod/luz-antivirus/istio-cloudrun-routing.yaml" class="external-link" rel="nofollow">kubernetes-overlays\env-prod\luz-antivirus\istio-cloudrun-routing.yaml</a>

Modify the `VirtualService`, adjusting the `weight` values under the `route` section:

For example, with this configuration → *when luz-docs sends requests to the luz-antivirus service on GKE, the Service Mesh Istio Route distributes **50%** of traffic to the GKE deployment and **50%** to the CloudRun service*.


![[49224646657-image-20260311-101112.png]]



</div>

</div>
