---
ai_hash: 301f5690df92c096
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.814
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47723151789/Recipe+Introduce+a+Global+HTTP+S+Load+Balancer+on+GCP+at+KLARA
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: 'Recipe: Introduce a Global HTTP(S) Load Balancer on GCP at KLARA'
topic: infra
type: source
updated: 2024-06-04
---

# Recipe: Introduce a Global HTTP(S) Load Balancer on GCP at KLARA

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-06-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47723151789/Recipe+Introduce+a+Global+HTTP+S+Load+Balancer+on+GCP+at+KLARA)
> Relevance 0.814 · topic `infra`

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh conf-macro output-inline" hasbody="false" macro-id="53b916c3-c988-417b-8380-44250f63593f" macro-name="status">WORK_IN_PROGRESS</span>

This page is a step by step guide to introduce a new or update an existing Global HTTP(S) Load Balancer on GCP at KLARA. This kind of LB is mainly needed to introduce a Cloud Armor Web Application Firewall (WAF).

## Overview

## Create a new Global HTTP(S) Load Balancer on GCP

### Prerequisites

- Create a Global Static IP address in GCP  
  Why global? <a href="https://cloud.google.com/armor/docs/security-policy-overview" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/armor/docs/security-policy-overview</a> → Because there are some limitations with region based HTTP(S) LBs.  
  Please update an existing `luz_kubernetes` infrastructure script or add a new script for your project to partially automate the deployment (Sample `luz_kubernetes` script <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/713b2f6f2509914cd902231a4f2d83e72405fbbf/cluster/gcp/create_environment_cloud_armor.sh#lines-26:38" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/713b2f6f2509914cd902231a4f2d83e72405fbbf/cluster/gcp/create_environment_cloud_armor.sh#lines-26:38</a> )

- Define a Sub-/Domain which is accessible from the outside (internet) and to the mapping of your Global Static IP address

- Create or use an existing SSL Certificate (e.g `luz-ingress-tls`)

- KLARA SSL policy is available (e.g. `klara-gke-ingress-ssl-policy-global`). Script to create a new SSL policy <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/cluster/gcp/manual_script/create_ssl_policy_for_external_lb.sh" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/master/cluster/gcp/manual_script/create_ssl_policy_for_external_lb.sh</a>

### Steps

1.  **Create an ingress front end config (SSL policy needed) if it does not exist yet**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="228d9b87-259c-4cf1-9220-01896b20d6ff" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    apiVersion: networking.gke.io/v1beta1
    kind: FrontendConfig
    metadata:
      name: <project_name>-ingress-frontend-config
    spec:
      redirectToHttps:
        enabled: true
        responseCodeName: MOVED_PERMANENTLY_DEFAULT
      sslPolicy: <ssl_policy_name>
    ```

    </div>

    </div>

    \<ssl_policy_name\> valid policy name  
    \<project_name\> e.g. klara or eh  
    **Note:** Do not forget to add this config to kustomization.yaml  

2.  **Create Global HTTP(S) LB Ingress routing configuration**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1ae312d9-2f00-4e09-bb00-64ed64973786" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # External HTTP(s) Load Balancing
    # Routing configuration (rules)
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: <ingress_name>-ingress-rule
      annotations:
        networking.gke.io/v1beta1.FrontendConfig: "<ingress_frontend_config_name>"
        ingress.kubernetes.io/ssl-redirect: "true"
        kubernetes.io/ingress.class: "gce"
        kubernetes.io/ingress.global-static-ip-name: "<global_static_ip_address_name>"
    spec:
      tls:
      - hosts:
        - <domain_name>
        secretName: <tls_secret_name>
      defaultBackend:
        service:
          name: <backend_service_name>-nginx-ingress
          port:
            number: 443
      rules:
      - host: <domain_name>
        http:
          paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: <backend_service_name>-nginx-ingress
                port:
                  number: 443
    ```

    </div>

    </div>

    \<ingress_name\> e.g. gateway  
    \<domain_name\> e.g test.klara-eh.tech  
    \<tls_secret_name\> e.g. luz-ingress-tls  
    \<ingress_frontend_config_name\> e.g eh-ingress-frontend-config  
    \<global_static_ip_address_name\> e.g. test-klara-eh-tech-global  

3.  **Configure NGINX Ingress custom health check.**  
    Add the following server snippet to you NGINX Ingress configmap.  
    Health check:  
    - Path: /healthz  
    - Port: 8080

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="885dfb36-a64a-4137-9337-5ede7b68babf" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
      http-snippets: |
        
        ...
        
        server {
          listen 8080 default_server;
          server_name _;
          server_tokens "on";
          access_log off;

          location /healthz {
              return 200;
          }
        }
    ```

    </div>

    </div>

4.  **Adding backend-config to backend service**  
    The backend config contains the following configurations:  
    - Cloud Armor security policy  
    - Health check used by the Global HTTPS LB  
    - Set custom X-Forwarded-Host header (for security reason)

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="088a8431-cbba-4bb0-a500-bca5a3610c9b" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    apiVersion: cloud.google.com/v1
    kind: BackendConfig
    metadata:
      name: <backend_service_name>-backend-config
    spec:
      securityPolicy:
        name: "<security_policy_name>"
      timeoutSec: 3600
      healthCheck:
        checkIntervalSec: 60
        port: <health_check_port>
        type: <health_check_type>
        requestPath: <health_check_subpath>
      customRequestHeaders:
        headers:
        - "X-Forwarded-Host:{tls_sni_hostname}"
    ```

    </div>

    </div>

    \<backend_service_name\> e.g. login-nginx-ingress  
    \<security_policy_name\> e.g. dev-security-policy  
    \<health_check_port\> e.g. 8080  
    \<health_check_subpath\> e.g. /healthz  
    \<health_check_type\> e.g. HTTP  
    **Note:** Do not forget to add this config to kustomization.yaml  

5.  **Add backend-config to NGINX ingress backend service**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d7142881-cfe3-4bcc-85cc-c86dbead73e1" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    apiVersion: v1
    kind: Service
    metadata:
      name: <backend_service_name>
      annotations:
        cloud.google.com/neg: '{"ingress": true}'
        networking.gke.io/load-balancer-type: "Internal"
        cloud.google.com/backend-config: '{"default": "<backend_config_name>"}'
        cloud.google.com/app-protocols: '{"https":"HTTPS"}'
    spec:
      ports:
      - name: health
        port: <health_check_port>
        targetPort: <health_check_port>
    ```

    </div>

    </div>

    \<backend_service_name\> e.g. login-nginx-ingress  
    \<health_check_port\> e.g. 8080  
    \<backend_config_name\> e.g. login-nginx-ingress-backend-config  

6.  **Testing**

## Update an existing Global HTTP(S) Load Balancer

### Prerequisites

- Create a Global Static IP address in GCP  
  Why global? <a href="https://cloud.google.com/armor/docs/security-policy-overview" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/armor/docs/security-policy-overview</a> → Because there are some limitations with region based HTTP(S) LBs.  
  Please update an existing `luz_kubernetes` infrastructure script or add a new script for your project to partially automate the deployment (Sample `luz_kubernetes` script <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/src/713b2f6f2509914cd902231a4f2d83e72405fbbf/cluster/gcp/create_environment_cloud_armor.sh#lines-26:38" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/src/713b2f6f2509914cd902231a4f2d83e72405fbbf/cluster/gcp/create_environment_cloud_armor.sh#lines-26:38</a> )

- Define a Sub-/Domain which is accessible from the outside (internet) and to the mapping of your Global Static IP address

- Create or use an existing SSL Certificate (e.g `luz-ingress-tls`)

### Steps

1.  **Update Global HTTP(S) LB Ingress routing configuration with your NGINX Ingress backend service**  
    Adding:  
    - `spec.tls.hosts` configs (domain name and TLS secret name)  
    - `spec.rules` configs

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="604b9aab-62e3-4fce-9215-9f8be127f9a7" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # External HTTP(s) Load Balancing
    # Routing configuration (rules)
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: <ingress_name>-ingress-rule
      annotations:
        networking.gke.io/v1beta1.FrontendConfig: "<ingress_frontend_config_name>"
        ingress.kubernetes.io/ssl-redirect: "true"
        kubernetes.io/ingress.class: "gce"
        kubernetes.io/ingress.global-static-ip-name: "<global_static_ip_address_name>"
    spec:
      tls:
      - hosts:
        - <domain_name_1>
        secretName: <tls_secret_name_1>
      - hosts:
          - <domain_name_2>
          secretName: <tls_secret_name_2>
      defaultBackend:
        service:
          name: <backend_service_name>-nginx-ingress
          port:
            number: 443
      rules:
      - host: <domain_name_1>
        http:
          paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: <backend_service_name_1>-nginx-ingress
                port:
                  number: 443
      - host: <domain_name_2>
          http:
            paths:
            - path: /
              pathType: Prefix
              backend:
                service:
                  name: <backend_service_name_2>-nginx-ingress
                  port:
                    number: 443
    ```

    </div>

    </div>

    \<ingress_name\> e.g. eh  
    \<domain_name\> e.g test.klara-eh.tech  
    \<tls_secret_name\> e.g. luz-ingress-tls  
    \<ingress_frontend_config_name\> e.g eh-ingress-frontend-config  
    \<global_static_ip_address_name\> e.g. test-klara-eh-tech-global  

2.  **Follow the step 3 until the end from “Create a new Global HTTP(S) Load Balancer on GCP“**

%% ai-graph-start %%

**Related notes:**
- [[Recipe Introduce IAP and Cloud Armor on GCP with external HTTP(S) load balancing]]
- [[GKE Kubernetes Gateway API]]
- [[Apply changes on luz_kubernetes]]
- [[POS & myKLARA nginx ingress quick notes]]
- [[Deploy module GKE with a public url]]

%% ai-graph-end %%