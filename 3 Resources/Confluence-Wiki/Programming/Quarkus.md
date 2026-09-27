---
ai_hash: 12aa342dc2686e26
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.84
entities: []
relevance: 0.804
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47780167681/Quarkus
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Quarkus
topic: programming
type: source
updated: 2024-05-24
---

# Quarkus

> [!info] Imported from Confluence
> Space **Helios** · updated 2024-05-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47780167681/Quarkus)
> Relevance 0.804 · topic `programming`

<a href="https://quarkus.io/" class="external-link" data-card-appearance="inline" rel="nofollow">https://quarkus.io/</a>

<a href="https://axonivy.atlassian.net/wiki/spaces/AVATAR/pages/47062778889/Install+quarkus?search_id=3e7910e6-ce58-4823-a643-4d713c436053" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/AVATAR/pages/47062778889/Install+quarkus?search_id=3e7910e6-ce58-4823-a643-4d713c436053</a>

<a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/10/01/46967849037/QUARKUS+REACTIVE+ARCHITECTURE?search_id=3f3c203a-45d6-4119-9394-d51226b17356" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/10/01/46967849037/QUARKUS+REACTIVE+ARCHITECTURE?search_id=3f3c203a-45d6-4119-9394-d51226b17356</a>

More references:

<a href="https://developers.redhat.com/blog/2019/11/18/how-quarkus-brings-imperative-and-reactive-programming-together#is_that_all_" class="external-link" data-card-appearance="inline" rel="nofollow">https://developers.redhat.com/blog/2019/11/18/how-quarkus-brings-imperative-and-reactive-programming-together#is_that_all_</a>

------------------------------------------------------------------------

First look:

- Create a simple Rest API and deploy into docker

  - Sample: <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="bd90f415-8224-4c37-b1fa-3bea9decf580" macro-name="view-file"><a href="../_attachments/47780167681-quarkus.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/47780167681/quarkus.zip?version=1&amp;modificationDate=1716545050068&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[47780167681-quarkus.zip]]

</a></span>

  - 

![[47780167681-image-20240524-101635.png]]



<div id="expander-1997354976" class="expand-container conf-macro output-block" hasbody="true" macro-id="d2a89f78-cbf6-473e-94ce-7d7d875e3133" macro-name="expand">

<div id="expander-control-1997354976" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Deploy to gcp - gke, workload - reference from github copilot (not try yet)</span>

</div>

<div id="expander-content-1997354976" class="expand-content expand-hidden">

To deploy your Quarkus application to Google Cloud Platform (GCP) using Google Kubernetes Engine (GKE), you can use Workloads. Here are the steps:

1.  Build your Quarkus application:  
    You can use Maven to build your Quarkus application. Run the following command in your project directory:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5d37ae82-33cf-4071-8476-f046defe0fd2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mvn package
```

</div>

</div>

2.  Build the Docker image:  
    Use the Dockerfile provided in your project. The Dockerfile should be located in the `src/main/docker` directory. Run the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6701856e-3f6d-4a0a-9060-b8f57f14c45f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker build -f src/main/docker/Dockerfile.jvm -t gcr.io/your-gcp-project-id/quarkus-app .
```

</div>

</div>

Replace `your-gcp-project-id` with your GCP project ID and `quarkus-app` with the name of your Docker image.

3.  Push the Docker image to Google Container Registry (GCR):  
    After the Docker image is built, you can push it to GCR using the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a0164d13-0b5e-4660-9d54-0b4ad9bf6cc9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker push gcr.io/your-gcp-project-id/quarkus-app
```

</div>

</div>

4.  Create a Kubernetes Deployment:  
    You can create a Kubernetes Deployment to manage your application. Here's an example of a Deployment configuration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e643bbbb-672b-42dc-bc4f-ec3f2356ef09" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: apps/v1
kind: Deployment
metadata:
  name: quarkus-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: quarkus-app
  template:
    metadata:
      labels:
        app: quarkus-app
    spec:
      containers:
      - name: quarkus-app
        image: gcr.io/your-gcp-project-id/quarkus-app
        ports:
        - containerPort: 8080
```

</div>

</div>

Save this configuration to a file, for example `quarkus-app-deployment.yaml`, and create the Deployment using the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2b66e675-75da-48b8-808f-0a93e17df174" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl apply -f quarkus-app-deployment.yaml
```

</div>

</div>

5.  Create a Kubernetes Service:  
    To make your application accessible, you can create a Kubernetes Service. Here's an example of a Service configuration:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3bb56701-48fe-49fb-8368-9637ff13b7eb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: v1
kind: Service
metadata:
  name: quarkus-app
spec:
  type: LoadBalancer
  ports:
  - port: 8080
  selector:
    app: quarkus-app
```

</div>

</div>

Save this configuration to a file, for example `quarkus-app-service.yaml`, and create the Service using the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="567a9259-dd81-4842-b9a5-58cf7ca70e98" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl apply -f quarkus-app-service.yaml
```

</div>

</div>

After running these commands, your Quarkus application should be running on GCP and accessible via the external IP address of the Service.

Please note that you need to have the Google Cloud SDK and kubectl installed and configured on your machine to run these commands. You can download them from <a href="https://cloud.google.com/sdk/docs/install" class="external-link" rel="nofollow">here</a> and <a href="https://kubernetes.io/docs/tasks/tools/install-kubectl/" class="external-link" rel="nofollow">here</a>, respectively.

</div>

</div>

------------------------------------------------------------------------

------------------------------------------------------------------------

%% ai-graph-start %%

**Related notes:**
- [[Recipe Quarkus.io getting started]]
- [[GCP Overview]]
- [[Recipe Deploy with Terraform]]
- [[Deploy Google Cloud Run for new module]]
- [[Document flow setup build Jenkins job Maven]]

%% ai-graph-end %%