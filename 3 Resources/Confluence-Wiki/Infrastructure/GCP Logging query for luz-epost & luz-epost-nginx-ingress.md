---
ai_hash: 9ddf5144d2a0470f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.86
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47881093312/GCP+Logging+query+for+luz-epost+luz-epost-nginx-ingress
space: TS
status: reference
tags:
- confluence
- infra
- space/ts
title: GCP Logging query for [luz-epost] & [luz-epost-nginx-ingress]
topic: infra
type: source
updated: 2024-06-14
---

# GCP Logging query for [luz-epost] & [luz-epost-nginx-ingress]

> [!info] Imported from Confluence
> Space **TS** · updated 2024-06-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47881093312/GCP+Logging+query+for+luz-epost+luz-epost-nginx-ingress)
> Relevance 0.86 · topic `infra`

All pods contain `luz-epost` in the name

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5ac0b0b9-44cf-4e4b-bf1a-b15d7ce05781" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
resource.type="k8s_container"
resource.labels.cluster_name="klara-nonprod"
resource.labels.namespace_name="dev"
--labels.k8s-pod/app="luz-epost"
resource.labels.pod_name=~"luz-epost"
AND labels.k8s-pod/app!="luz-epost-hub"
```

</div>

</div>

`luz-epost-nginx-ingress` only:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7096915d-41dc-4139-a492-752c45d79def" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
resource.labels.cluster_name="klara-nonprod"
resource.labels.namespace_name="dev"
labels.k8s-pod/app="luz-epost-nginx-ingress"
```

</div>

</div>

`luz-epost` nextjs deployment only:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e25f1327-568b-4f9a-8c7e-f3eae0c75009" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
resource.labels.cluster_name="klara-nonprod"
resource.labels.namespace_name="dev"
labels.k8s-pod/app="luz-epost"
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Infrastructure]]
- [[POS & myKLARA nginx ingress quick notes]]
- [[Luz Kubernetes Terraform]]
- [[Apply changes on luz_kubernetes]]
- [[Apply branch code of webclient-nginx-ingress.yaml]]

%% ai-graph-end %%