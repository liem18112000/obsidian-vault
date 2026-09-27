---
ai_hash: 5243657bb8ca1e03
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.927
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530459723/Serverless+with+Google+Cloud+Run
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Serverless with Google Cloud Run
topic: infra
type: source
updated: 2021-07-21
---

# Serverless with Google Cloud Run

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-07-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530459723/Serverless+with+Google+Cloud+Run)
> Relevance 0.927 · topic `infra`

## Cloud Run and Cloud Function


![[20530459723-screen-shot 2021-07-02 um 14.02.57.png]]



## Repository

<a href="https://bitbucket.org/axonivy-prod/luz_thumbnail_function" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_thumbnail_function</a>

## Performance comparison to Kubernetes

Test generating 10 thumbnails in a row

Cloud Run instance takes 550ms on average.

Kubernetes pod takes 700ms on average.

## Scaling

<a href="https://cloud.google.com/run/docs/about-instance-autoscaling" class="external-link" rel="nofollow">About container instance autoscaling  |  Cloud Run Documentation (google.com)</a>

Ability to scale to zero.

Scale base on CPU utilization (Default is 60% CPU).

Allow concurrency setting and automatically scale to handle incoming requests. We can define the maximum number of requests each instance can handle at a time. <a href="https://cloud.google.com/run/docs/about-concurrency" class="external-link" rel="nofollow">Concurrency  |  Cloud Run Documentation  |  Google Cloud</a>

## Call to Cloud Run service from Kubernetes

By default, each service is assigned to an URL like this <a href="https://luz-thumbnail-run-q5rqhzn2uq-as.a.run.app" class="external-link" rel="nofollow">https://luz-thumbnail-run-q5rqhzn2uq-as.a.run.app</a>. We are able to config that allowing internal traffic only. 

`https://<serviceName>-<projectHash>-<region>.run.app`

<a href="https://stackoverflow.com/questions/62785417/what-is-the-format-of-cloud-run-service-urls" class="external-link" rel="nofollow"><code>https://stackoverflow.com/questions/62785417/what-is-the-format-of-cloud-run-service-urls</code></a>

URLs of a same service are different in different environment ← If is a a problem then we need to find a solution. Eg: An internal load balancer for services in the same environment, proxy for cloud Run in Kubernetes side...

By default, Could Run service required Bearer Authorization for role <a href="https://console.cloud.google.com/iam-admin/roles/details/roles%3Crun.invoker?project=klara-nonprod" class="external-link" rel="nofollow">Cloud Run Invoker – IAM</a>.

We can disable it and use our own Authorization method.

```
```


![[20530459723-image2021-7-21_8-44-58.png]]



## Call to Kubernetes service from Could Run

<a href="https://ahmet.im/blog/cloud-run-vpc-to-kubernetes/" class="external-link" rel="nofollow">Connecting Kubernetes privately from Cloud Run over VPC Network (ahmet.im)</a>

## Deployment

<a href="https://cloud.google.com/run/docs/deploying#yaml" class="external-link" rel="nofollow">Deploying container images  |  Cloud Run Documentation  |  Google Cloud</a>

Images could be stored in Container Registry as currently.

Define YAML file to deploy. For example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="28824c86-f625-469a-9c7f-d9f2a4546256" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud beta run services replace service.yaml --region asia-southeast1
```

</div>

</div>

  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a07184f8-2926-4fd0-a09a-4eef5e867a4c" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**luz-thumbnail**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: luz-thumbnail-dev-vn
spec:
  template:
    metadata:
      annotations:
        autoscaling.knative.dev/maxScale: '30'
    spec:
      containerConcurrency: 80
      containers:
      - image: gcr.io/klara-nonprod/luz-thumbnail-function:3
        ports:
        - containerPort: 8080
        resources:
          limits:
            cpu: 2000m
            memory: 2Gi
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run generates a per-project URL hash, breaking environment config parity]]
- [[Kubernetes knowledge]]
- [[One API end to end testing]]
- [[Infrastructure]]
- [[Recipe Deploy with Terraform]]

%% ai-graph-end %%