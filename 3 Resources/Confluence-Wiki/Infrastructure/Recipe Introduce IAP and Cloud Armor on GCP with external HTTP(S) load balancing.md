---
ai_hash: 557a94d63af3e34c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 13
depth: 2.38
entities: []
relevance: 0.706
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47049670792/Recipe+Introduce+IAP+and+Cloud+Armor+on+GCP+with+external+HTTP+S+load+balancing
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: 'Recipe: Introduce IAP and Cloud Armor on GCP with external HTTP(S) load balancing'
topic: infra
type: source
updated: 2022-04-07
---

# Recipe: Introduce IAP and Cloud Armor on GCP with external HTTP(S) load balancing

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-04-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47049670792/Recipe+Introduce+IAP+and+Cloud+Armor+on+GCP+with+external+HTTP+S+load+balancing)
> Relevance 0.706 · topic `infra`

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh conf-macro output-inline" hasbody="false" macro-id="838bc288-36b8-45bf-bd9e-8366ad9de82e" macro-name="status">WORK_IN_PROGRESS</span>

This page shows a sample configuration on GCP regarding Identity Aware Proxy (IAP) and Cloud Armor on GCP with external HTTP(S) load balancing.

## Overview

Reference: <a href="https://cloud.google.com/kubernetes-engine/docs/concepts/ingress-xlb" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/kubernetes-engine/docs/concepts/ingress-xlb</a>


![[47049670792-GKE Ingress for HTTP_S Load Balancing.png]]



- 1 Ingress is associated with 1..n Services

- 1 Service is associated with 1..n Pods

- Creating an Ingress object the GKE Ingress controller creates a Google Cloud HTTP(S) Load Balancer

- To use ingress the HTTP load balancing add-on has to be enabled (default).

- <a href="https://cloud.google.com/kubernetes-engine/docs/concepts/ingress-xlb" class="external-link" rel="nofollow">Ingress for External HTTP(S) Load Balancing</a> deploys the <a href="https://cloud.google.com/load-balancing/docs/https" class="external-link" rel="nofollow">global external HTTP(S) load balancer (classic)</a>.

## Sample setup at KLARA


![[47049670792-luz-admin-tool kubernetes overview.png]]



- luz-ingress-tls SSL Certificate has been used for HTTP(S) load balancer TLS termination and admin-nginx-ingress-xxx TLS termination

- HTTP redirect to HTTPS will be applied with the admin-ingress-frontend-config

- Creating a HTTP(S) load balancer will automatically create an entry at GCP “External IP addresses” for a static IP. In our case the entry name is “k8s2-fr-wkl7ehhz-iap-admin-ingress-rule-i7vg6jvn”.

- SSL has been applied from inbound traffic until admin-nginx-ingress-xxx

- Health check request on port 8080 at admin-nginx-ingress-service (implemented on each admin-nginx-ingress-xxx pod)

- admin-nginx-ingress-service Type is NodePort (can also be Loadbalancer)

- Create secret with Kustomize for IAP client credentials

GIT Repo: <a href="https://bitbucket.org/axonivy-prod/iap-sample/src/master/" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/iap-sample/src/master/</a>

### Test: SSL from HTTP(S) load balancer to admin-nginx-ingress-controller

#### Steps

1.  Web browser: <a href="https://admin-dev.klara.tech/hello-app2" class="external-link" rel="nofollow">https://admin-dev.klara.tech/hello-app</a>1

2.  admin-nginx-ingress-service log (with ssl_protocol=TLSv1.3):  

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="354c2b5b-60f1-4f6a-b317-26e524366e83" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    {
      "timestamp": "2022-02-16T16:42:38+00:00", 
      "upstreamStatus": "200", 
      "upstreamAddr": "10.0.1.106:8080",
      "httpRequest":{
        "requestMethod": "GET", 
        "requestUrl": "admin-dev.klara.tech/hello-app1", 
        "status": 200,"requestSize": "2199", 
        "responseSize": "66", 
        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.80 Safari/537.36", 
        "remoteIp": "35.191.0.37", 
        "referer": "-", 
        "latency": "0.002 s", 
        "protocol":"HTTP/1.1", 
        "ssl_protocol":"TLSv1.3"
      }
    }
    ```

    </div>

    </div>

### Test: hello-app\*

Those tests will proof that the incoming request will be routed through the external HTTP(S) load balancer to reach the “hello world” applications.

- Configure /etc/hosts file to map external HPPT(S) load balancer IP address

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a4b3a8c9-5b18-49eb-ab11-fbc8db815d09" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  34.149.134.44      admin-dev.klara.tech
  ```

  </div>

  </div>

#### Steps

1.  Test 1: with Web Browser:  
    - <a href="https://admin-dev.klara.tech/hello-app2" class="external-link" rel="nofollow">https://admin-dev.klara.tech/hello-app2</a>  
    - <a href="https://admin-dev.klara.tech/hello-app2" class="external-link" rel="nofollow">https://admin-dev.klara.tech/hello-app</a>1

2.  Check result:  

    

![[47049670792-https___admin-dev_klara_tech_hello-app2_and_admin-nginx-ingress_–_Details_zum_De.png]]



      

    

![[47049670792-https___admin-dev_klara_tech_hello-app1.png]]



3.  Test 2: with CURL

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4e0d47f9-618e-45ce-92f7-9ac5a3829ac3" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    ➜  ~ curl https://admin-dev.klara.tech/hello-app1
    Hello, world!
    Version: 1.0.0
    Hostname: hello-app-599779cf6c-tlwbk

    ➜  ~ curl https://admin-dev.klara.tech/hello-app2
    Hello, world!
    Version: 1.0.0
    Hostname: hello-app2-b4dfcc48f-wlhjk
    ```

    </div>

    </div>

### Changes to KLARA GCP projects

- Introduce external HTTP(S) load balancer

- Apply HTTP to HTTPS redirect for inbound traffic

- Change external load balancer to internal load balancer

- Apply Cloud Armor backend config to services

### Test: IAP

This test will proof that the “hello world” apps are available only for IAP specific users to get access from www.

#### Steps

1\. Web browser: <a href="https://admin-dev.klara.tech/hello-app1" class="external-link" rel="nofollow">https://admin-dev.klara.tech/hello-app1</a>

2\. Login screen


![[47049670792-Sign_in_-_Google_Accounts.png]]



3\. Access permission denied


![[47049670792-KLARA_Admin__Access_Denied.png]]



  
5. Application access (Access permission allowed)


![[47049670792-https___admin-dev_klara_tech_hello-app1_and_Enabling_IAP_for_Compute_Engine_ _ _.png]]



### Test: Cloud Armor

Those tests will proof that incoming request from www will be allowed and denied.

Prerequisites

- Cloud Armor security policy configured

#### Steps

1.  Web browser input URI <a href="https://admin-dev.klara.tech/hello-app1" class="external-link" rel="nofollow">https://admin-dev.klara.tech/hello-app1</a>

2.  Check result at Cloud Armor security policy logs

    1.  There is a policy configured as preview (previewSecurityPolicy) which would block the request to hello-app1 with rule owasp-crs-v030001-id942421-sqli. Since this rule has been configured in preview mode this request won’t be denied.

    2.  Another rule (no preview) allows the request (enforcedSecurityPolicy).

        <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c4f0034f-c88c-4900-b73e-c3723ebf319a" macro-name="code" style="border-width: 1px;">

        <div class="codeContent panelContent pdl">

        ``` syntaxhighlighter-pre
        {
          "insertId": "ibk6glfhb0tsa",
          "jsonPayload": {
            "previewSecurityPolicy": {
              "priority": 99998,
              "name": "ca-how-to-security-policy",
              "outcome": "DENY",
              "preconfiguredExprIds": [
                "owasp-crs-v030001-id942421-sqli"
              ],
              "configuredAction": "DENY"
            },
            "statusDetails": "response_sent_by_backend",
            "@type": "type.googleapis.com/google.cloud.loadbalancing.type.LoadBalancerLogEntry",
            "enforcedSecurityPolicy": {
              "priority": 2147483647,
              "name": "ca-how-to-security-policy",
              "configuredAction": "ALLOW",
              "outcome": "ACCEPT"
            }
          },
          "httpRequest": {
            "requestMethod": "GET",
            "requestUrl": "https://admin-dev.klara.tech/hello-app1",
            "requestSize": "1515",
            "status": 200,
            "responseSize": "284",
            "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.80 Safari/537.36",
            "remoteIp": "51.154.10.247",
            "serverIp": "10.172.0.61",
            "latency": "0.007318s"
          },
          "resource": {
            "type": "http_load_balancer",
            "labels": {
              "url_map_name": "k8s2-um-wkl7ehhz-iap-admin-ingress-rule-i7vg6jvn",
              "target_proxy_name": "k8s2-ts-wkl7ehhz-iap-admin-ingress-rule-i7vg6jvn",
              "forwarding_rule_name": "k8s2-fs-wkl7ehhz-iap-admin-ingress-rule-i7vg6jvn",
              "project_id": "klara-playground-dg-01",
              "backend_service_name": "k8s1-6a6efc53-iap-admin-nginx-ingress-service-60000-75c6862b",
              "zone": "global"
            }
          },
          "timestamp": "2022-02-16T07:48:40.510456Z",
          "severity": "INFO",
          "logName": "projects/klara-playground-dg-01/logs/requests",
          "trace": "projects/klara-playground-dg-01/traces/bec0c5cc5c08df112c5837f007401ad7",
          "receiveTimestamp": "2022-02-16T07:48:40.992478942Z",
          "spanId": "f58d458421925e4d"
        }     
        ```

        </div>

        </div>

3.  Response on web browser  

    

![[47049670792-https___admin-dev_klara_tech_hello-app1_and_Skype__2__and_luz_kubernetes_–_kuber.png]]



## Challenges

- Correct Ingress class configuration

  - Older version (`metadata/annotations/kubernetes.io/ingress.class`) new version (`spec/ingressClassName`)

  - Default Ingress class for HTTP(S) load balander is `GCE`

- Rules with host (DNS or IP) has to be defined on Ingress even with default backend

- Enable access log with `access_log on` did not work. Just remove access log config

- Service account

  - needed to apply nginx-ingress deployment

  - `container.roleBindings.create` permission needed to create a service account

- SSL Cert is needed for nginx-ingress deployment event if it is not used

- Applying admin-ingress-rule (HTTP(S) load balancer) takes time ~15min)

## TODO

- <span class="placeholder-inline-tasks">Introduce IAP</span>
- <span class="placeholder-inline-tasks">Introduce SSL between HTTP(S) load balancer and admin-nginx-ingress</span>
- <span class="placeholder-inline-tasks">Static IPs for HTTP(S) load balancer ?  
  --\> Creating a HTTP(S) load balancer will automatically create an entry at GCP “External IP addresses” for a static IP.</span>
- <span class="placeholder-inline-tasks">Check in encrypted cert → won’t be checked in at GIT</span>
- <span class="placeholder-inline-tasks">Update document and diagram</span>
- <span class="placeholder-inline-tasks">Introduce Cloud Armor policies</span>
- <span class="placeholder-inline-tasks">Next steps IAP </span>
- <span class="placeholder-inline-tasks">Next steps Cloud Armor</span>
- <span class="placeholder-inline-tasks">Deploy Apps on Infra project , e.g. keycloak, luz_docs, etc.</span>
- <span class="placeholder-inline-tasks">Share Service among GCP projects?  
  Shared VPC (<a href="https://cloud.google.com/vpc/docs/shared-vpc" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/vpc/docs/shared-vpc</a> )  
  - Only available within the same organization (Google Cloud organization) with host project  
  <a href="https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-shared-vpc" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-shared-vpc</a></span>
- <span class="placeholder-inline-tasks">Cloud Armor step by step guide for DEV [Recipe: GCP Cloud Armor (WAF) step by step guide](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47063531691/Recipe+GCP+Cloud+Armor+WAF+step+by+step+guide)</span>
- <span class="placeholder-inline-tasks">Update diagram with certificate info</span>
- <span class="placeholder-inline-tasks">IAP adding roles instead of adding each user separately</span>


![[47049670792-image-20220228-140103.png]]



## Links

- Setup IAP on GCP GKE <a href="https://cloud.google.com/iap/docs/enabling-compute-howto" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/iap/docs/enabling-compute-howto</a>

- GCP Cloud Armor dokument <a href="https://cloud.google.com/armor" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/armor</a>

- IAP Session Management <a href="https://cloud.google.com/iap/docs/sessions-howto" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/iap/docs/sessions-howto</a>

%% ai-graph-start %%

**Related notes:**
- [[Recipe Introduce a Global HTTP(S) Load Balancer on GCP at KLARA]]
- [[GKE Kubernetes Gateway API]]
- [[Axonivycloud - Deploy Nginx Ingress for EKS]]
- [[Apply changes on luz_kubernetes]]
- [[POS & myKLARA nginx ingress quick notes]]

%% ai-graph-end %%