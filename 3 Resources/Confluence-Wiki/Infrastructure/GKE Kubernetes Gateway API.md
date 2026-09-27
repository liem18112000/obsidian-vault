---
ai_hash: bd51bbe32cb1e3ed
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 8
depth: 2.55
entities: []
relevance: 0.748
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/48411803705/GKE+Kubernetes+Gateway+API
space: IO
status: reference
tags:
- confluence
- infra
- space/io
title: GKE Kubernetes Gateway API
topic: infra
type: source
updated: 2025-03-19
---

# GKE Kubernetes Gateway API

> [!info] Imported from Confluence
> Space **IO** · updated 2025-03-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48411803705/GKE+Kubernetes+Gateway+API)
> Relevance 0.748 · topic `infra`

The GKE Kubernetes Gateway API enables advanced traffic routing and management for Kubernetes clusters. We use it to create regional external application load balancer for Cloud Armor WAF.

## Overview

**This overview is not complete!**


![[48411803705-Kubernetes Gateway API overview.png]]



### Own Domain


![[48411803705-Own Domain (Klara Website) overview.png]]



## Implementation

Implementations can be found at `luz_kubernetes/kubernetes-overlays/<env>/gateway`.

### **Login Gateway API sample (DEV)**

#### Gateway

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="53920375-26f9-4dd1-8ec4-2089f801c0dc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
metadata:
  name: login-gateway
  labels:
    klara.ch/module: login-gateway
spec:
  addresses:
    - type: NamedAddress
      value: login-dev-klara-tech-regional            # Regional static IP address name
  gatewayClassName: gke-l7-regional-external-managed  # Gateway class for Regional external application LB  
  listeners:
    - name: http-listener                             # HTTP listener port 80
      protocol: HTTP
      port: 80
      allowedRoutes:
        kinds:
          - kind: HTTPRoute
        namespaces:
          from: Same
    - name: https-listener                            # HTTPS listener port 443
      port: 443
      protocol: HTTPS
      allowedRoutes:
        kinds:
          - kind: HTTPRoute
        namespaces:
          from: Same
      tls:
        mode: Terminate
        certificateRefs:
          - kind: Secret
            group: ""
            name: luz-ingress-tls                     # SSL Certificate for klara sub domains
          - kind: Secret
            group: ""
            name: luz-ingress-epost-tls               # SSL Certificate for ePost sub domains
```

</div>

</div>

#### HTTPRoute

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="638a394a-4254-449d-8bb7-48f02a5ad19e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: login-https-route                                             # HTTPS route
  labels:
    klara.ch/module: login-gateway
spec:
  hostnames:                                                          # Routes for following sub dommains
    - "login-dev.klara.tech"                                          
    - "login-dev.klara-epost.tech"
  parentRefs:
    - kind: Gateway
      name: login-gateway
      sectionName: https-listener                                     # For HTTPS listener
  rules:
    - matches:
        - path:
            value: /
            type: PathPrefix
      filters:
        - type: RequestHeaderModifier                                 # Set X-Forwarded-Host header (security)
          requestHeaderModifier:
            set:
              - name: X-Forwarded-Host
                value: "{tls_sni_hostname}"
        - type: ResponseHeaderModifier
          responseHeaderModifier:
            set:
              - name: Strict-Transport-Security                       # Set STS header (security)
                value: "max-age=31536000; includeSubDomains; preload"
      backendRefs:
        - name: login-nginx-ingress                                   # route to login backend
          port: 443
---
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: login-http-redirect-route                                     # Redirect route HTTP --> HTTPS
  labels:
    klara.ch/module: login-gateway
spec:
  hostnames:                                                          # Routes for following sub dommains
    - "login-dev.klara.tech"
    - "login-dev.klara-epost.tech"
  parentRefs:
    - kind: Gateway
      name: login-gateway
      sectionName: http-listener                                      # For HTTP listener
  rules:
    - filters:
        - type: RequestRedirect                                       # Redirect config
          requestRedirect:
            scheme: https
---
```

</div>

</div>

#### HealthCheckPolicy

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="815093d7-7baf-4242-8fa1-ff7871fa5374" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: networking.gke.io/v1
kind: HealthCheckPolicy
metadata:
  name: login-nginx-ingress-healthcheck
  labels:
    klara.ch/module: login-gateway
spec:
  default:
    checkIntervalSec: 60                    # Health check interval
    timeoutSec: 10                          # HC timeout
    healthyThreshold: 1                     
    unhealthyThreshold: 2
    logConfig:                              
      enabled: true                         # Log config
    config:
      type: HTTP
      httpHealthCheck:
        requestPath: /healthz               # HC URL path
        port: 8080                            
  targetRef:
    group: ""
    kind: Service
    name: login-nginx-ingress               # Target backend
```

</div>

</div>

#### GCPGatewayPolicy

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ca9988a0-abbd-48b7-adc6-959a23a7f616" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: networking.gke.io/v1
kind: GCPGatewayPolicy
metadata:
  name: login-policy
  labels:
    klara.ch/module: login-gateway
spec:
  default:
    sslPolicy: dev-ssl-policy-regional       # SSL policy reference
  targetRef:
    group: gateway.networking.k8s.io
    kind: Gateway
    name: login-gateway                      # For login gateway
```

</div>

</div>

#### GCPBackendPolicy

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7ff5b2b2-4306-445e-ab02-54ea1c8be841" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: networking.gke.io/v1
kind: GCPBackendPolicy
metadata:
  name: login-backend-policy
  labels:
    klara.ch/module: login-gateway
spec:
  default:
    timeoutSec: 3600
    securityPolicy: dev-security-policy-regional    # Cloud Armor WAF security policy reference 
    logging:
      enabled: true
      sampleRate: 1000000
  targetRef:
    group: ""
    kind: Service
    name: login-nginx-ingress                       # For login backend 
```

</div>

</div>

## Links

- <a href="https://cloud.google.com/kubernetes-engine/docs/concepts/gateway-api" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/kubernetes-engine/docs/concepts/gateway-api</a>

%% ai-graph-start %%

**Related notes:**
- [[Recipe Introduce a Global HTTP(S) Load Balancer on GCP at KLARA]]
- [[Recipe Introduce IAP and Cloud Armor on GCP with external HTTP(S) load balancing]]
- [[Apply changes on luz_kubernetes]]
- [[POS & myKLARA nginx ingress quick notes]]
- [[Deploy module GKE with a public url]]

%% ai-graph-end %%