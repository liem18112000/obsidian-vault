---
title: "Axonivycloud - Monitoring EKS cluster using Prometheus and Grafana"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3557491091/Axonivycloud+-+Monitoring+EKS+cluster+using+Prometheus+and+Grafana
space: "AII"
topic: infra
relevance: 0.866
depth: 3
updated: 2019-12-02
attachments: 0
tags:
  - confluence
  - infra
  - space/aii
---

# Axonivycloud - Monitoring EKS cluster using Prometheus and Grafana

> [!info] Imported from Confluence
> Space **AII** · updated 2019-12-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3557491091/Axonivycloud+-+Monitoring+EKS+cluster+using+Prometheus+and+Grafana)
> Relevance 0.866 · topic `infra`

# I. INSTALL HELM CLI

## 1. Download

Before we can get started configuring helm we’ll need to first install the command line tools that you will interact with. To do this run the following:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8a42285f-3c65-4624-ac4c-b03a3bc24e8d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
chmod +x get_helm.sh
./get_helm.sh
```

</div>

</div>

## 2. Configure Helm access with RBAC

Helm relies on a service called tiller that requires special permission on the kubernetes cluster, so we need to build a Service Account for tiller to use. We’ll then apply this to the cluster.

Create **rbac.yaml** file with following content:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="64656d1a-1204-4b98-865d-6fd62a237ed1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

</div>

</div>

  

Next apply the config:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3022f9eb-eef4-4295-86f3-732b3d93efc4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl apply -f rbac.yaml
```

</div>

</div>

## 3. Install

Now we can install helm using the helm tooling

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cde26df9-cedf-4f6a-9530-07e01dd11d93" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
helm init --service-account tiller
```

</div>

</div>

This will install tiller into the cluster which gives it access to manage resources in your cluster.

# II. MONITORING USING PROMETHEUS AND GRAFANA

This guide is a modified version of the full guide:

<a href="https://eksworkshop.com/monitoring/" class="external-link" rel="nofollow">https://eksworkshop.com/monitoring/</a>

## 1. Requirements

1.  We need to have Helm installed
2.  We need to have a persistent storage storageclass before creating the monitor system. In this example, we created a storageclass named **aws-efs**

## 2. Download Prometheus

Download Prometheus config using the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d08a65ba-70f8-4495-8e23-0269a6ecc8c8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl -o prometheus-values.yaml https://raw.githubusercontent.com/helm/charts/master/stable/prometheus/values.yaml
```

</div>

</div>

Open the **prometheus-values.yaml** you downloaded and you need to make some edits to this file.

Search for **storageClass** in the prometheus-values.yaml, uncomment and change the value to “**aws-efs**”. You will do this twice, under both server & alertmanager manifests.

## 3. Deploy Prometheus

Run the following command to deploy:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ab2f4d10-c369-4031-b5bc-9be1bd79e926" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
helm install -f prometheus-values.yaml stable/prometheus --name prometheus --namespace prometheus
```

</div>

</div>

Make a note of prometheus endpoint in helm response (you will need this later). It should look similar to below

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c957ecc4-2ccc-44c5-a583-b8d82655804e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
The Prometheus server can be accessed via port 80 on the following DNS name from within your cluster:
prometheus-server.prometheus.svc.cluster.local
```

</div>

</div>

Check if Prometheus components deployed as expected

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="37df2d9b-7e9c-42b8-8cf7-35ec50bfbbcb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl get all -n prometheus
```

</div>

</div>

Note: We do not need to directly access to Prometheus so we will not create any ingress for it. Grafana can access Prometheus internally using `prometheus-server.prometheus.svc.cluster.local`

## 4. Deploy Grafana

Download Grafana config using the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="97df964b-75b7-420d-a3e2-47c3fd47f650" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl -o grafana-values.yaml https://raw.githubusercontent.com/helm/charts/master/stable/grafana/values.yaml
```

</div>

</div>

We will make three edits to **grafana-values.yaml**. Search for **storageClassName**, uncomment and change the value to **“prometheus” **, change **enabled** to **true**. Search for **adminPassword**, uncomment and change the password to **“our*very*secure_pass”**. Make a note of this password as you will need it for logging into grafana dashboard later

The third edit you will do is for adding Prometheus as a datasource. Search for datasources.yaml and uncomment entire block, update prometheus to the endpoint referred earlier by helm response. The configuration will look similar to below:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="de5dc296-26c3-4e21-b92e-9d6e67d0491d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
datasources:
 datasources.yaml:
   apiVersion: 1
   datasources:
   - name: Prometheus
     type: prometheus
     url: http://prometheus-server.prometheus.svc.cluster.local
     access: proxy
     isDefault: true
```

</div>

</div>

Deploy grafana using the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="998d7adb-fdf7-466c-8c83-306ec7628e68" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
helm install -f grafana-values.yaml stable/grafana --name grafana --namespace grafana
```

</div>

</div>

Run the command to check if Grafana is running properly

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b81133e7-82b8-45e0-ad06-b61a5a048f4f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl get all -n grafana
```

</div>

</div>

To change any values of Grafana or Prometheus, edit the values file and run the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="294ac92e-4a95-4076-a3a0-5e2dfae470f8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
helm upgrade -f grafana-values.yaml grafana stable/grafana --namespace grafana
```

</div>

</div>

  
5. Expose Grafana for public access

Create grafana-tls.yaml with following content:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="093dcac5-1a86-4c66-b0fc-eccee2f725b0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: v1
kind: Secret
type: kubernetes.io/tls
metadata:
  name: grafana-tls
  namespace: grafana
data:
  tls.crt: ...
  tls.key: ...
```

</div>

</div>

Create tls secret by running the following command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2101554e-f3c3-47a5-86a0-7f298ce2d850" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl create -f grafana-tls.yaml
```

</div>

</div>

Create the grafana-ingress.yaml with following content:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9cf8e751-7f2a-43b2-8a55-f2491adf7cba" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: grafana-ingress
  namespace: grafana
  annotations:
    kubernetes.io/tls-acme: "true"
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    nginx.ingress.kubernetes.io/configuration-snippet: |
      proxy_set_header               X-Real-IP $remote_addr;
      proxy_set_header               X-Forwarded-For $proxy_add_x_forwarded_for;
      add_header                     X-Cache-Status $upstream_cache_status;
      add_header                     X-Frame-Options sameorigin;
      proxy_redirect                 off;
      recursive_error_pages          on;
      proxy_set_header               Connection "";
spec:
  tls:
  - hosts:
      - ivy-demo-monitor.axonivy.io
    secretName: grafana-tls
  rules:
  - host: ivy-demo-monitor.axonivy.io
    http:
      paths:
        - path: /
          backend:
            serviceName: grafana
            servicePort: 3000
```

</div>

</div>

  

Create ingress by running command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b0815fd2-3b76-4799-858d-431c6b3be6f8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl create -f grafana-ingress.yaml
```

</div>

</div>

Now we can access Grafana using: http://monitor.axonivy.io

Add some board for monitoring using below id or links:

- Infrastructure monitor: 1860
- Application metrics: 7624
- Nginx Ingress monitor: <a href="https://github.com/kubernetes/ingress-nginx/tree/master/deploy/grafana/dashboards" class="external-link" rel="nofollow">https://github.com/kubernetes/ingress-nginx/tree/master/deploy/grafana/dashboards</a>

**Repo**: <a href="https://bitbucket.org/tnhthanh/eks_monitoring.git" class="external-link" rel="nofollow">https://bitbucket.org/tnhthanh/eks_monitoring.git</a>
