---
ai_hash: b61374ed675d3829
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.866
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3597925187/Deploy+AFDEMO+CPM
space: AII
status: reference
tags:
- confluence
- infra
- space/aii
title: Deploy AFDEMO CPM
topic: infra
type: source
updated: 2021-10-26
---

# Deploy AFDEMO CPM

> [!info] Imported from Confluence
> Space **AII** · updated 2021-10-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3597925187/Deploy+AFDEMO+CPM)
> Relevance 0.866 · topic `infra`

## Kubernetes cluster

<span class="legacy-color-text-blue3">(1)Create Nginx ingress controller which being expose by Internal network load balancer(NLB). The Ingress class name is: "nginx-nlb"</span>

  

There are something need to be changed after download deployment package:

- (2)Add dist-registry-secret.yaml: This is credential of <a href="https://dist.axonfintech.io/" class="external-link" rel="nofollow" style="text-decoration: none;">https://dist.axonfintech.io/</a> , the secret name "dist-registry"

- cpm-app-application-config-kubernetes.yaml: <span class="legacy-color-text-blue3">Change database.jdbc.url to external DB instance</span>

- cpm-app-database-secret-kubernetes.yaml: username, password are database credential for application

- cpm-app-idm-config-kubernetes-idm.yaml: <span class="legacy-color-text-blue3">change db.addr to external DB instance</span>

- cpm-app-idm-secret-kubernetes.yaml: <span class="legacy-color-text-blue3">change db-username/db-password of DB instance, admin-username/admin-password for keycloak admin</span>

- cpm-app-kubernetes.yaml:
  - Delete nginx ingress creation part (from begining to line 661 "# Source: cpm-app-keycloak-adapter-service-secret.yaml")

  - In Ingress: add <a href="http://kubernetes.io/ingress.class" class="external-link" rel="nofollow" style="text-decoration: none;">kubernetes.io/ingress.class</a>: "nginx-nlb" below annotations

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4ccf9753-969d-4b80-9a20-520335ab9c4f" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # Source cpm-app-services-kubernetes.yaml
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
    ......
      annotations:
        kubernetes.io/ingress.class: "nginx-nlb"
    ```

    </div>

    </div>

- Push the changes to <a href="https://bitbucket.org/axonivy-prod/conditionandpricemanager" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/conditionandpricemanager</a>

### Undeploy current version

- Go to current version folder of CPM. For example, the old version is 0.9.1:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cfdc1be8-2c13-4bf5-81b1-b76dae6f66cc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd conditionandpricemanager/0.9.1/package/
```

</div>

</div>

- Delete the application

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5ea95abc-ebe9-4704-b104-06d541a296b8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl delete -f cpm-app-kubernetes.yaml
```

</div>

</div>

### Deploy new version:

- Navigate to new version of CPM. For example, the new version is 0.9.3:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e2b3c366-49bf-4ac0-8a54-70ed8d0a2d30" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd conditionandpricemanager/0.9.1/package/
```

</div>

</div>

- Go to config folder, then create all configs & secrets

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5f311750-a443-4eab-b09b-87ec19ba5482" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd config
kubectl apply -f .
```

</div>

</div>

- Modify the default service account for the namespace to use "dist-registry" secret which is created by the file in step 2 as an imagePullSecret.
  - Linux:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b7348a8a-e3a6-423e-80e2-6b0ca482dfe0" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "dist-registry"}]}' -n com-axonfintech-cpm
    ```

    </div>

    </div>

  - Windows:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e95ce16a-3c4c-44f7-a91d-13718132a9cd" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    kubectl patch serviceaccount default -p '{\"imagePullSecrets\": [{\"name\": \"dist-registry\"}]}' -n com-axonfintech-cpm
    ```

    </div>

    </div>
- deploy application

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8e9aba51-8c8b-4dae-b848-6ce13e8d5b49" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd ..
kubectl apply -f cpm-app-kubernetes.yaml
```

</div>

</div>

## afdemo-nat

Edit /etc/hosts file, add the cpm-app.local record, ip address is the IP of internal load balancer which created in step (1)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="235ba93b-edbd-45fb-895e-1196add8ae69" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
10.136.66.157 cpm-app.local
```

</div>

</div>

<span style="letter-spacing: 0.0px;">Create file /etc/nginx/sites-enabled/conditionpricemanager-af1-demo.axonfintech.io.conf</span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5641b5d5-58a9-43a8-9838-8c9afea5e641" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
server {
    listen 80;
    server_name conditionpricemanager-af1-demo.axonfintech.io;
    return 301 https://$server_name$request_uri;
}

server {
        listen 443 ssl http2;
        server_name conditionpricemanager-af1-demo.axonfintech.io;
        ssl_certificate      /etc/nginx/ssl/axonfintechio.pem;
        ssl_certificate_key  /etc/nginx/ssl/axonfintechio.key;

        access_log /var/log/nginx/conditionpricemanager-af1-demo.axonfintech.io.log combined  buffer=512k flush=5m;
        error_log /var/log/nginx/conditionpricemanager-af1-demo.axonfintech.io.error.log ;
        client_max_body_size 10M;
        # gzip
        gzip on;
        gzip_min_length 256;
        gzip_types text/plain application/xml application/javascript text/css application/octet-stream image/svg+xml application/json;
    location / {
         proxy_pass         http://cpm-app.local;
         proxy_set_header X-Forwarded-Host $http_host;
         proxy_set_header        X-Forwarded-Proto $http_x_forwarded_proto;
   }

}
```

</div>

</div>

## DNS

Create A record <a href="https://conditionpricemanager-af1-demo.axonfintech.io/" class="external-link" rel="nofollow">conditionpricemanager-af1-demo.axonfintech.io</a> point to public IP address of Nginx load balancer which created in step 1

%% ai-graph-start %%

**Related notes:**
- [[Deploy AFDEMO OM]]
- [[Axonivycloud - Deploy Nginx Ingress for EKS]]
- [[2.31 Build & deploy agent review service to k8s (POC)]]
- [[Infrastructure]]
- [[How to connect K8S database from postgres]]

%% ai-graph-end %%