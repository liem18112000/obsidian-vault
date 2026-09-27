---
ai_hash: a1d0c8adfbcf35b5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.48
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48473309206/Apply+branch+code+of+webclient-nginx-ingress.yaml
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Apply branch code of webclient-nginx-ingress.yaml
topic: infra
type: source
updated: 2025-04-29
---

# Apply branch code of webclient-nginx-ingress.yaml

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-04-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48473309206/Apply+branch+code+of+webclient-nginx-ingress.yaml)
> Relevance 0.724 · topic `infra`

### Example of code change


![[48473309206-image-20250414-031337.png]]



### Generate yaml file for performance environment

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="036d951d-b313-4dc0-95e3-82552deec821" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Change path "C:/Work/klara/git/luz_kubernetes" to your local part
winpty docker run -it -v /C:/Work/klara/git/luz_kubernetes:/root/development/luz_kubernetes gcr.io/klara-repo/luz-deploy:latest bash

# After in the context of docker, run commands:

cd root/development/luz_kubernetes/

sed -i -e 's/\r$//' deploy_to_stdout.sh
./deploy_to_stdout.sh performance > deployment-dev-performance.yaml
# Double-check the size of file to make use the file generated successfully
```

</div>

</div>

### Delete pod webclient-nginx-ingress and ingress webclient-nginx-ingress-rules

Alternative: Deleting on GUI instead of commands is safer

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="37c681b3-2a71-44e8-9b70-6e020f206e81" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# In the cmd path is your repository, in my case it is: "C:/Work/klara/git/luz_kubernetes"
# Switch to projet performance
gcloud container clusters get-credentials klara-performance --zone europe-west6-a --project klara-performance

# Get pods on namespace performance to re-check before delete
kubectl get pods -n performance

# webclient-nginx-ingress is pod name, ex: webclient-nginx-ingress-79dd4b87d8-tw5xg
kubectl delete pod/webclient-nginx-ingress -n performance

# Get ingresses on namespace performance to re-check before delete
kubectl get ingresses -n performance

kubectl delete ingress/webclient-nginx-ingress-rules -n performance
```

</div>

</div>

### Apply new changes of ingress

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a002fc6e-f3d4-4646-bad7-1375ae1ae421" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl apply -l klara.ch/module=main-ingress -f deployment-dev-performance.yaml
```

</div>

</div>

### Re-check on GCP after apply to know whether new pod and ingress are running


![[48473309206-image-20250414-031447.png]]

![[48473309206-image-20250414-031606.png]]

%% ai-graph-start %%

**Related notes:**
- [[Apply changes on luz_kubernetes]]
- [[Kubernetes knowledge]]
- [[How to deploy in Performance]]
- [[POS & myKLARA nginx ingress quick notes]]
- [[Recipe Deploy with Terraform]]

%% ai-graph-end %%