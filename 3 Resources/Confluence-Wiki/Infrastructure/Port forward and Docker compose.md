---
ai_hash: c18d2d3a48910aa3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.84
entities: []
relevance: 0.757
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20502817909/Port+forward+and+Docker+compose
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Port forward and Docker compose
topic: infra
type: source
updated: 2023-07-31
---

# Port forward and Docker compose

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-07-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20502817909/Port+forward+and+Docker+compose)
> Relevance 0.757 · topic `infra`

### 1 Only develop on ivy side (1)

There is a api-gateway service deploy on k8s cluster this service user for forwarding request to services in backend side. So that we only have one portward let's say 8080 for all services in backend side.

<a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-dev/api-forwarder/" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/kubernetes-overlays/env-dev/api-forwarder/</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="feb2eb4e-648e-4dd4-8896-70694073be8b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl port-forward svc/api-forwarder --address 0.0.0.0 8080:8080 5000:8080 -n dev
```

</div>

</div>

Now we have a forward listen on port 8080 and normally we also use this port for wildfly so that we dont need to change everything in ivy.

<span class="legacy-color-text-red2">**Note:**</span> we can use account **admin/admin** to login or create your own account.

### 2. Port forward for luz-database

  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="22fdd73f-2b95-4af2-8fd4-e1543d7e70a8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl port-forward svc/luz-database-tunnel --address 0.0.0.0 6543:5432 -n devgcp
```

</div>

</div>

Now you can connect database in gcp in local with port 6543

**Cons :** Latency ,  inconsistency database from ivy designer and luz_system on GCP , luz_system store all case id and task id

### 3 Develop in backend side

Let's take example we are develop a feature in module **luzfin_finance**.

**<span class="legacy-color-text-red2">Note:</span>** We still keep for terminal (1) open. Then we have a docker compose file below for running luzfin_finance

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="942898eb-c56d-4d65-a1bd-04a6c06d48ce" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**docker-compose.yml**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
version: '3.7'
services:
  luzfin-finance:
    build:
      context: 'D:\Avatar\LUZ\project-service\luzfin_finance'
    environment:
      LUZFIN_JDBC: 'jdbc:postgresql://host.docker.internal:6543/luzfinance'
      LUZFIN_DB_USER: 'luzfinance'
      LUZFIN_DB_PASS: 'luzfinance'

      IVY_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_SEC_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_TENANT_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_PERSON_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_COMPENSATION_HOST_PORT: 'http://host.docker.internal:8080'
      LUZFIN_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_ACCOUNTING_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_KEYVALUESTORE_HOST_PORT: 'http://host.docker.internal:8080'
      LUZ_ARTICLE_HOST_PORT: 'http://host.docker.internal:8080'
    ports:
      - 9090:8080
      - 8787:8787
```

</div>

</div>

Run your docker compose with command: **docker-compose -f docker-compose.yaml up --build**

Note: In order to enable debug in image we have to change a little bit because we dont enable debug in default(This can be discuss to enable in base image)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="712bc80c-6005-4140-b07b-6fdd9402413b" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Dockerfile**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# above code
EXPOSE 8080 8787

CMD ["/opt/jboss/wildfly/bin/standalone.sh", "-b", "0.0.0.0", "-bmanagement", "0.0.0.0", "--debug"]
```

</div>

</div>

  

So far now you can work on luzfin_finance backend and other request to route to k8s.

### 4 Develop in Ivy and  backend

Change variable property api to the correct port in our example is <a href="http://localhost:9090/luzfin_finance/api/" class="external-link" rel="nofollow">http://localhost:9090/luzfin_finance/api/</a> for other we keep it like it is.

Change database filemanager(user:filemanager/password:filemanager), luz_admin(user:luzsystem/password:luzsystem), xent_additional(user:luzsystem/password:luzsystem) to the correct one in local or gcp.

### 5 Additional configuration

If you access to the **Widget Store** and images can not be loaded, you may need some more configuration: Open file  **C:\Windows\System32\drivers\etc\hosts **then add one more line to this file:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="50c18924-6ed7-4b8f-83ab-012cb0def075" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
127.0.0.1 luz-store
```

</div>

</div>

Then try to Ctrl + F5 on the browser then it should work.

%% ai-graph-start %%

**Related notes:**
- [[Port Forward to call GCP API in localhost]]
- [[How to Start Invoice Run v2]]
- [[Local access to GKE-hosted Luz services port-forward api-forwarder + luz-vault (+ mongo pod)]]
- [[Infrastructure]]
- [[Run luz_docs_statistic locally with docker-compose]]

%% ai-graph-end %%