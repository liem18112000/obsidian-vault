---
title: "Deploy AFDEMO OM"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3597920218/Deploy+AFDEMO+OM
space: "AII"
topic: infra
relevance: 0.779
depth: 2.83
updated: 2023-01-11
attachments: 1
tags:
  - confluence
  - infra
  - space/aii
---

# Deploy AFDEMO OM

> [!info] Imported from Confluence
> Space **AII** · updated 2023-01-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3597920218/Deploy+AFDEMO+OM)
> Relevance 0.779 · topic `infra`

## Kubernetes cluster

(1)Create Nginx ingress controller which being expose by Internal network load balancer(NLB). The Ingress class name is: "nginx-nlb"

  

**Note: Jan-11-2023:**  
**Ingress install by Bitnami chart: bitnami/nginx-ingress-controller**

**Load balancer using type: Network (NLB)  
  

![[3597920218-image2023-1-11_15-3-46.png]]

  **

  

  

There are something need to be changed after download deployment package:

- (2)Add dist-registry-secret.yaml: This is credential of <a href="https://dist.axonfintech.io/" class="external-link" rel="nofollow">https://dist.axonfintech.io/</a> , the secret name "dist-registry"
- om-app-application-config-kubernetes.yaml: Change database.jdbc.url to external DB instance
- om-app-database-secret-kubernetes.yaml: change username and password of DB instance
- om-app-idm-config-kubernetes-idm.yaml: change db.addr to external DB instance
- om-app-idm-secret-kubernetes.yaml: change db-username/db-password of DB instance, admin-username/admin-password for keycloak admin
- om-app-kubernetes.yaml:
  - Delete nginx ingress creation part (from begining to line 661 "# Source: om-app-keycloak-adapter-service-secret.yaml")

  - In Ingress: add <a href="http://kubernetes.io/ingress.class" class="external-link" rel="nofollow">kubernetes.io/ingress.class</a>: "nginx-nlb" below annotations

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d63d127c-15bb-4ec2-af87-3093a59031a4" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
      labels:
        app.com.axonfintech.om.ingress: om-app
      annotations:
        kubernetes.io/ingress.class: "nginx-nlb"
    ```

    </div>

    </div>
- Push the changes to <a href="https://bitbucket.org/axonivy-prod/onlinemortgage/src/master/0.8.13/package/" class="external-link" rel="nofollow" style="letter-spacing: 0.0px;" title="https://bitbucket.org/axonivy-prod/onlinemortgage/src/master/0.8.13/package/">https://bitbucket.org/axonivy-prod/onlinemortgage</a>

### Undeploy current version

- Go to current version folder of OM. For example, the old version is 0.8.13:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1c4b656c-b851-426a-9b44-0c5aaf7dc70a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd onlinemortgage/0.8.13/package/
```

</div>

</div>

- Delete the application

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e278562a-dc9c-41d2-802a-d07609709786" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl delete -f om-app-kubernetes.yaml
```

</div>

</div>

### Deploy new version:

- Navigate to new version of OM. For example, the new version is 0.8.17:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3abe563c-2c73-4fe9-a544-ed6fab06a3b1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd onlinemortgage/0.8.17/package/
```

</div>

</div>

- Go to config folder, then create all configs & secrets

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8e4b772e-c2cb-44ac-a1a2-393306cb295e" macro-name="code" style="border-width: 1px;">

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
    kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "dist-registry"}]}' -n com-axonfintech-om
    ```

    </div>

    </div>

  - Windows:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d08c07e1-7c28-4cf2-a34f-4f578eb341fd" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    kubectl patch serviceaccount default -p '{\"imagePullSecrets\": [{\"name\": \"dist-registry\"}]}' -n com-axonfintech-om
    ```

    </div>

    </div>

<!-- -->

- deploy application

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="46ef79b3-3fac-400c-940b-460aacda6057" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cd ..
kubectl apply -f om-app-kubernetes.yaml
```

</div>

</div>

## afdemo-nat

Edit /etc/hosts file, add the om-app.local record, ip address is the IP of internal load balancer which created in step (1)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ad4b1301-8875-4d92-aaca-b85b0be9a0d3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
10.136.66.157 om-app.local
```

</div>

</div>

Create <a href="https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/" class="external-link" rel="nofollow">.htpasswd-fintech-om file</a> , this file will be including in the configuration below.

Create file /etc/nginx/sites-enabled/onlinemortgage-af1-demo.axonfintech.io.conf

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="277659e1-7dc8-45f3-a6ee-8ea3c09e882e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
server {
    listen 80;
    server_name onlinemortgage-af1-demo.axonfintech.io;
    return 301 https://$server_name$request_uri;
}

server {
        listen 443 ssl http2;
        server_name onlinemortgage-af1-demo.axonfintech.io;
        ssl_certificate      /etc/nginx/ssl/axonfintechio.pem;
        ssl_certificate_key  /etc/nginx/ssl/axonfintechio.key;

        access_log /var/log/nginx/onlinemortgage-af1-demo.axonfintech.io.log combined  buffer=512k flush=5m;
        error_log /var/log/nginx/onlinemortgage-af1-demo.axonfintech.io.error.log ;
        client_max_body_size 10M;
        # gzip
        gzip on;
        gzip_min_length 256;
        gzip_types text/plain application/xml application/javascript text/css application/octet-stream image/svg+xml application/json;
    location = / {
         auth_basic           "Restricted area";
         auth_basic_user_file .htpasswd-fintech-om;
         proxy_pass         http://om-app.local;
         proxy_set_header X-Forwarded-Host $http_host;
         proxy_set_header        X-Forwarded-Proto $http_x_forwarded_proto;

    }
    location / {
         proxy_pass         http://om-app.local;
         proxy_set_header X-Forwarded-Host $http_host;
         proxy_set_header        X-Forwarded-Proto $http_x_forwarded_proto;
   }
}
```

</div>

</div>

  

- The second tenant can be create the same, 

## DNS

Create A record onlinemortgage-af1-demo.axonfintech.io, <a href="http://onlinemortgage-af1-demo.axonfintech.io" class="external-link" rel="nofollow">onlinemortgage-blb-demo.axonfintech.io</a> point to public IP address of afdemo-nat instance
