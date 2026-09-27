---
ai_hash: a6d28ac8cab2cd1a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.87
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3557491085/Axonivycloud+-+Deploy+Nginx+Ingress+for+EKS
space: AII
status: reference
tags:
- confluence
- infra
- space/aii
title: Axonivycloud - Deploy Nginx Ingress for EKS
topic: infra
type: source
updated: 2019-11-11
---

# Axonivycloud - Deploy Nginx Ingress for EKS

> [!info] Imported from Confluence
> Space **AII** · updated 2019-11-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3557491085/Axonivycloud+-+Deploy+Nginx+Ingress+for+EKS)
> Relevance 0.87 · topic `infra`

This guide shows how to deploy nginx ingress for EKS and how to create an ingress for kubernetes application.

# **I. DEPLOY NGINX INGRESS**

This guide is shorted version of the full guide:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="080f862f-623c-4e5d-9f4d-b0f8a77ddfa1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://kubernetes.github.io/ingress-nginx/deploy/
```

</div>

</div>

## **1. Prerequisite Generic Deployment Command**

The following Mandatory Command is required for all deployments.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="945a3f77-ab63-4a96-8859-a1783ad70d15" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml
```

</div>

</div>

## **2. Deploy Nginx Ingress**

Run the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="eee1cf1b-b183-4673-aa08-307ecb5e65c1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/aws/service-l4.yaml

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/aws/patch-configmap-l4.yaml
```

</div>

</div>

**3. Verify installation**

To check if the ingress controller pods have started, run the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8c485582-4146-45a1-9a72-68768f209da5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl get pods --all-namespaces -l app.kubernetes.io/name=ingress-nginx --watch
```

</div>

</div>

## **4. Config Nginx configuration**

To modify nginx config, run the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="349864d7-ae42-48ba-b768-4d26a0c5bce4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl edit configmap nginx-configuration -n ingress-nginx
```

</div>

</div>

Clear all nginx config in data section, input the following:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="695a8cd4-31af-4212-ac91-d405f569a4ae" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
 client-body-buffer-size: 128K
 client-max-body-size: 20m
 hsts: "true"
 proxy-body-size: 1G
 proxy-buffering: "off"
 proxy-read-timeout: "600"
 proxy-send-timeout: "600"
 server-tokens: "false"
 ssl-redirect: "false"
 upstream-keepalive-connections: "50"
 use-forwarded-headers: "true"
 use-proxy-protocol: "true"
```

</div>

</div>

So the configmap should look like this:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3dda3f3c-cba4-4b09-b4ed-f5c951c23854" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: v1
data:
 client-body-buffer-size: 128K
 client-max-body-size: 20m
 hsts: "true"
 proxy-body-size: 1G
 proxy-buffering: "off"
 proxy-read-timeout: "600"
 proxy-send-timeout: "600"
 server-tokens: "false"
 ssl-redirect: "false"
 upstream-keepalive-connections: "50"
 use-forwarded-headers: "true"
 use-proxy-protocol: "true"
kind: ConfigMap
metadata:
 creationTimestamp: "2019-03-08T04:31:38Z"
 labels:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
 name: nginx-configuration
 namespace: ingress-nginx
 resourceVersion: "690313"
 selfLink: /api/v1/namespaces/ingress-nginx/configmaps/nginx-configuration
 uid: 107098c7-415b-11e9-b965-0ab09295d7a8
```

</div>

</div>

# **II. CREATE INGRESS FOR KUBERNETES APPLICATION**

## **1. Requirements**

Before create an ingress for kubernetes application, the application need to be exposed as a service.

## **2. Create ingress**

Firstly, we can know the service name and it's port by running the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a90f21ff-bebe-4b89-bb8e-9e3a78302579" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl get svc -n namespace_of_application
```

</div>

</div>

We will get something like this:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0c00d094-86bc-4d78-ae71-f988c4b54ce8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
ivy-service   ClusterIP   172.20.99.117   <none>        8080/TCP   10d
```

</div>

</div>

So the service name is **ivy-service** and it's port is **8080**.

We can create an ingress for it by using the yaml file as below:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f177ee1d-72a8-4508-b5b3-539b800cd996" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: ivy-ingress-domain
 annotations:
    kubernetes.io/tls-acme: "true"
    ingress.kubernetes.io/force-ssl-redirect: "true"
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/app-root: "/ivy"
    nginx.ingress.kubernetes.io/configuration-snippet: | 
        proxy_set_header X-Real-IP $remote_addr; 
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
        add_header X-Cache-Status $upstream_cache_status; 
        add_header X-Frame-Options sameorigin; 
        proxy_redirect off; 
        recursive_error_pages on; 
        proxy_set_header Connection ""; 
spec:
 tls:
 - hosts:
 - ivy-demo.axonivy.io
 secretName: axonivy-tls
 rules:
 - host: ivy-demo.axonivy.io
 http:
 paths:
 - path: /ivy
 backend:
 serviceName: ivy-service
 servicePort: 8080
```

</div>

</div>

# **Notes:**

After created nginx controller, aws will create a new ELB and automate add security group of ELB to security of EKS nodes. In case, the ELB status is outofservice, you need to check EKS nodes security group, does is allow all traffic for ELB

## **Troubleshoot:**

### 504 Timeout error

This occurs when the pod runs on the same node with the nginx pod. To fix this, change nginx service config from **externalTrafficPolicy: Cluster** to **externalTrafficPolicy: Local**

> kubectl edit svc ingress-nginx -n ingress-nginx
>     ...
>     externalTrafficPolicy: Local
>     ...

Delete old nginx pod and wait for new pod for change to effect

%% ai-graph-start %%

**Related notes:**
- [[Update Ingress Controller for GKE v1.22]]
- [[Recipe Introduce IAP and Cloud Armor on GCP with external HTTP(S) load balancing]]
- [[AxonivyCloud - Infrastructure Diagram EKS Proposal]]
- [[Deploy AFDEMO OM]]
- [[Deploy AFDEMO CPM]]

%% ai-graph-end %%