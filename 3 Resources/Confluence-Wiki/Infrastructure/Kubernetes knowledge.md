---
ai_hash: 7282f959a41f96ae
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.67
entities: []
relevance: 0.716
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/47095054407/Kubernetes+knowledge
space: TK
status: reference
tags:
- confluence
- infra
- space/tk
title: Kubernetes knowledge
topic: infra
type: source
updated: 2022-04-19
---

# Kubernetes knowledge

> [!info] Imported from Confluence
> Space **TK** · updated 2022-04-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/47095054407/Kubernetes+knowledge)
> Relevance 0.716 · topic `infra`

## Deploy new deployment in Kubernetes

#### Prepare image version in kubernetes

clone project kubernetes

<a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/</a>

Normally this is commit hash from git

**{your Path}\luz_kubernetes\kubernetes\luz-docs**


![[47095054407-image-20220419-022625.png]]



If you want deploy your image, you need to push your image to <a href="http://gcr.io" class="external-link" data-card-appearance="inline" rel="nofollow">http://gcr.io</a> then you can replace this image in your local then start these step below for deploy pod

**Step 1: Delete pod and services of Module (ex: luz_docs)**

noted: if you don’t delete pod with these command line, gcp will automaticly create the pod again with same image

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0a07381b-26ce-4f15-9bf7-ac5524411488" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl delete deployment luz-docs -n dev-vn
kubectl delete service luz-docs dev-vn
```

</div>

</div>

**Step 2: Run luz-deploy then access it for preparing deployment**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4cad48be-b7ab-4438-80f4-bbc194f166c6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker run --rm -it -v {Path in your local}/luz_kubernetes:/root/development/luz_kubernetes http://gcr.io/klara-repo/luz-deploy:0.0.1
```

</div>

</div>

After finish running the command line, we will see screen as below:


![[47095054407-image-20220419-021849.png]]



**Step 3: Continue login gcp (DEV-VN for example) in command screen**

Run these script as below

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="aa8c4f8e-671f-4117-a898-47434e6371f2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud auth login
gcloud container clusters get-credentials klara-dev-vn --zone asia-southeast1-a --project klara-nonprod
cd /root/development/luz_kubernetes
./deploy_to_stdout.sh dev-vn | kubectl apply -l klara.ch/module=luz-docs -f -
```

</div>

</div>

## Edit deployment in kubernetes

we can run this command line as below for editing k8s directly in kubernetes

for example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="75fb0c0f-fab4-437d-8dce-729ba2e05290" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl edit deployments/jwt-service -n dev-vn
```

</div>

</div>

After run this command, it will show up editor for edit k8s


![[47095054407-image-20220419-025129.png]]



## Optimize, scaling pod, configure variable and increase resource for POD in kubernetes

We can access to Luz_kubernetes change some config to optimize the resource of Pod

**{your path}\luz_kubernetes\kubernetes-overlays\env-dev\luz-docs**

The configration to link for each configration file will be in **kustomization.yaml**

You can follow these set up to configure new module

depend on your environment you can change to “env-dev-vn”


![[47095054407-image-20220419-025635.png]]



**patch-requests-limits.json**

this file will be the resource of each pod


![[47095054407-image-20220419-025746.png]]



**patch-horizontal-pod-autoscaler.json**

this file will be configure auto scaling of module.


![[47095054407-image-20220419-030141.png]]



## How to make secret the key in kubernetes

you can follow this link for more details

[Setup Configuration LUZ-AUDIT Storage on PROD](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47011364932/Setup+Configuration+LUZ-AUDIT+Storage+on+PROD)

## Config new module

[Configurations New Modules To GCP](https://axonivy.atlassian.net/wiki/spaces/TK/pages/30671884780/Configurations+New+Modules+To+GCP)

%% ai-graph-start %%

**Related notes:**
- [[Deploy luz-epc-redis-service on GCP]]
- [[Recipe Deploy with Terraform]]
- [[Apply branch code of webclient-nginx-ingress.yaml]]
- [[Apply changes on luz_kubernetes]]
- [[Document flow setup build Jenkins job Maven]]

%% ai-graph-end %%