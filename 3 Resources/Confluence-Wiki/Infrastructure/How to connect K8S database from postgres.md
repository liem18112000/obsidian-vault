---
ai_hash: 39bd105fb9083323
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.82
entities: []
relevance: 0.8
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/46985904382/How+to+connect+K8S+database+from+postgres
space: GRAVITY
status: reference
tags:
- confluence
- infra
- space/gravity
title: How to connect K8S database from postgres
topic: infra
type: source
updated: 2021-10-20
---

# How to connect K8S database from postgres

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2021-10-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/46985904382/How+to+connect+K8S+database+from+postgres)
> Relevance 0.8 · topic `infra`

When you run

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3348cb8d-2b29-46b5-8425-5e3923b3eefe" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
install.bat --replicas=1 --db
```

</div>

</div>

The database will be generated like ClusterIP in Kubernetes app, so the first thing you need to change to NodePort to expose your database.

1\. Download this script and put it everywhere you want

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="fa8a5786-4d57-455d-8629-f957912fcac0" macro-name="view-file"><a href="../_attachments/46985904382-expose-database.yaml" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/46985904382/expose-database.yaml?version=1&amp;modificationDate=1634698245626&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[46985904382-expose-database.yaml]]

</a></span>

2\. After all services are running, go to the folder containing expose-database.yaml and run kubectl command to expose the database

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="66dd8970-4a38-4a9f-9b74-d331967e4380" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl apply -f expose-database.yaml --force
```

</div>

</div>

3\. Get all services and check

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2bc0da74-a5ae-4404-981f-2324587a9a1f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl get service --namespace=com-axonfintech-lp
```

</div>

</div>

if the database change to NodePort, everything is OK


![[46985904382-2021-10-20_09h55_14.png]]



4\. Open the app to connect to Postgres DB and create a new server with

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3fb8f1f1-2fa2-463c-b06b-fbb32fdb3d38" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Host name/address: lp-app.local
Port: 30003 //which one is configed in the expose-database.yaml
Username: dbadmin
Password: pleaseCh@nge1t
```

</div>

</div>

The information from username and password, you can easily get from lp-app-kubernetes-database.yaml


![[46985904382-2021-10-20_10h02_01.png]]



5\. When connecting successfully, you can get your data in **“appuser“** schema.

%% ai-graph-start %%

**Related notes:**
- [[Deploy to Kubernetes and get External IP]]
- [[2.31 Build & deploy agent review service to k8s (POC)]]
- [[Deploy AFDEMO CPM]]
- [[GCP - Connect Database]]
- [[Port forward and Docker compose]]

%% ai-graph-end %%