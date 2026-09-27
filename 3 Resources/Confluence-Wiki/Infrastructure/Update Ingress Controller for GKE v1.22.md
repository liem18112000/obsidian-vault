---
ai_hash: f5df1e6b97d31658
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 2.83
entities: []
relevance: 0.779
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47380824122/Update+Ingress+Controller+for+GKE+v1.22
space: TS
status: reference
tags:
- confluence
- infra
- space/ts
title: Update Ingress Controller for GKE v1.22
topic: infra
type: source
updated: 2023-05-17
---

# Update Ingress Controller for GKE v1.22

> [!info] Imported from Confluence
> Space **TS** · updated 2023-05-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47380824122/Update+Ingress+Controller+for+GKE+v1.22)
> Relevance 0.779 · topic `infra`

Related ticket: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47380824122_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-93069" macro-id="1cde3ab5-9e63-4d6f-890a-20eabf79d268" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-93069" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-93069</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## \uD83D\uDCD8 Details

**Ingress Controller @KLARA** : [Deployment View](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20481507904/Deployment+View)

Repo: luz_kubernetes <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes/ingress/" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes/ingress/</a>

<div hasbody="true" macro-id="cb710c27-dbe5-4f29-9ee8-d2a1ff402be1" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**Nginx Ingress**

</div>

</div>

Nginx Ingress ( <a href="https://docs.nginx.com/nginx-ingress-controller/releases" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.nginx.com/nginx-ingress-controller/releases</a> ), GitHub: <a href="https://github.com/nginxinc/kubernetes-ingress" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/nginxinc/kubernetes-ingress</a>

→ Based on nginxinc distribution, the old version was running 1.6.2. We want to run the latest stable version

3.0.2, change logs: <a href="https://docs.nginx.com/nginx-ingress-controller/releases" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.nginx.com/nginx-ingress-controller/releases</a>

- Use nginx-ingress version 3.0.2 (latest stable) for all deployments using nginx-ingress, for example <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes/ingress/webclient-nginx-ingress.yaml" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes/ingress/webclient-nginx-ingress.yaml</a>

  

![[47380824122-image-20230517-084752.png]]



- Since nginx-ingress v2.x.x+ use IngressClass <a href="https://kubernetes.io/docs/concepts/services-networking/ingress/#ingress-class" class="external-link" data-card-appearance="inline" rel="nofollow">https://kubernetes.io/docs/concepts/services-networking/ingress/#ingress-class</a> resource and API, need a ClusterRole and ClusterRoleBinding for nginx-ingress k8s ServiceAccount

- 

![[47380824122-image-20230517-085817.png]]



- Migrate deprecated `kubernetes.io/ingress.class` → New IngressClass resource (keep for **gce** and **kong** ingress. Sees:  
  **GCE:** <a href="https://cloud.google.com/kubernetes-engine/docs/concepts/ingress#deprecated_annotation" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/kubernetes-engine/docs/concepts/ingress#deprecated_annotation</a>  
  **Kong:** <a href="https://docs.konghq.com/kubernetes-ingress-controller/latest/concepts/custom-resources/#tcpingress" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.konghq.com/kubernetes-ingress-controller/latest/concepts/custom-resources/#tcpingress</a> )

- nginx-ingress ServiceAccount is namespace scope, we need different binding for each namespace → Create patches for <a href="http://metadata.name" class="external-link" data-card-appearance="inline" rel="nofollow">http://metadata.name</a> and set **subjects.namespace** as default (`#default namespace for ClusterRoleBinding has special treatment with Kustomize, they will get adapt to namespace:{ENV} where you apply the kustomization.yaml.` See: <a href="https://github.com/kubernetes-sigs/kustomize/issues/3127" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/kubernetes-sigs/kustomize/issues/3127</a>)

- 

![[47380824122-image-20230517-090027.png]]



  nginx-ingress v3.0.2 use newer sets of k8s API (changelog: <a href="https://docs.nginx.com/nginx-ingress-controller/releases" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.nginx.com/nginx-ingress-controller/releases</a> ) → Update service account RBAC <a href="https://github.com/nginxinc/kubernetes-ingress/blob/main/deployments/rbac/rbac.yaml" class="external-link" data-card-appearance="inline" rel="nofollow">https://github.com/nginxinc/kubernetes-ingress/blob/main/deployments/rbac/rbac.yaml</a> and deployments target API

- 

![[47380824122-image-20230517-090413.png]]



## \uD83D\uDCCB Related articles

[Kinds of Nginx Ingress Controller and what is being used in Klara](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47120519124/Kinds+of+Nginx+Ingress+Controller+and+what+is+being+used+in+Klara)

[Deployment View](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20481507904/Deployment+View)

%% ai-graph-start %%

**Related notes:**
- [[Axonivycloud - Deploy Nginx Ingress for EKS]]
- [[Apply changes on luz_kubernetes]]
- [[POS & myKLARA nginx ingress quick notes]]
- [[Apply branch code of webclient-nginx-ingress.yaml]]
- [[Recipe Introduce IAP and Cloud Armor on GCP with external HTTP(S) load balancing]]

%% ai-graph-end %%