---
ai_hash: 64639d17571a887d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38218808673/Port+Forward+to+call+GCP+API+in+localhost.
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Port Forward to call GCP API in localhost.
topic: programming
type: source
updated: 2026-01-28
---

# Port Forward to call GCP API in localhost.

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-01-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38218808673/Port+Forward+to+call+GCP+API+in+localhost.)
> Relevance 0.738 · topic `programming`

1.  Login with Gcloud: go to CMD put  "gcloud auth login" ( make sure your PC installed gcloud SDK )

2.  Get the pods list for getting pod's name: "kubectl get pods -n dev"

3.  Port forward : "kubectl port-forward service/luz-pos \[local-port\]:8080 -n dev" (ex: local port is 8082 - \> 8082:8080)

4.  Call API from Postman: <span class="legacy-color-text-default">localhost:8082/luz_pos/api/{{tenant-id}}/companies/1/subscriptions?filter=existing&widget-codes=booking,booking2&from-date=2020-08-09T10:15:30Z&to-date=2020-09-20T10:15:30Z</span>

<span class="legacy-color-text-default">Note: turn off the CMD terminal after finish the test.</span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7932d6c0-a2b9-42d5-ad03-749a7cb476c3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl port-forward services/api-forwarder -n dev 8080:8080
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="467c433c-81d0-401e-98a9-89f5ad6950d7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
* GCP-DEV
    1. Change clusters
        gcloud container clusters get-credentials klara-nonprod --zone europe-west6-a --project klara-nonprod   2. Port-forward
        kubectl port-forward service/jwt-service 8081:8080 -n dev
        kubectl port-forward service/luz-article 8082:8080 -n dev
        kubectl port-forward service/luz-notification 8083:8080 -n dev
        kubectl port-forward service/luz-mobile 8084:8080 -n dev
        kubectl port-forward service/luz-pos-adapter 8084:8080 -n dev
        kubectl port-forward service/luz-pos 8086:8080 -n dev
        kubectl port-forward service/luz-register 8087:8080 -n dev
        kubectl port-forward service/luz-booking 9080:8080 -n dev
        kubectl port-forward service/luzfin-finance 8088:8080 -n dev
        kubectl port-forward service/ivy 8089:8080 -n dev
        kubectl port-forward service/luz-tenant 8090:8080 -n dev
        kubectl port-forward service/key-cloak 8091:8080 -n dev
        kubectl port-forward service/luz-compensation 8092:8080 -n dev
        kubectl port-forward service/luz-person 8093:8080 -n dev
        kubectl port-forward service/luz-accounting 8094:8080 -n dev
        kubectl port-forward service/luz-store 8095:8080 -n dev
        kubectl port-forward service/luz-database 5555:5432 -n dev  3. Logs console:
        kubectl get pods -n dev
        kubectl logs -f -n dev luz-mobile-5b68dd646-9dhz8 --tail=2000
* GCP-DEV-VN
    1. Change clusters
        gcloud container clusters get-credentials klara-dev-vn --zone asia-southeast1-a --project klara-nonprod 2. Port-forward
        kubectl port-forward service/jwt-service 8081:8080 -n dev-vn
        kubectl port-forward service/luz-article 8082:8080 -n dev-vn
        kubectl port-forward service/luz-notification 8083:8080 -n dev-vn
        kubectl port-forward service/luz-mobile 8084:8080 -n dev-vn
        kubectl port-forward service/luz-pos-adapter 8084:8080 -n dev-vn
        kubectl port-forward service/luz-pos 8086:8080 -n dev-vn
        kubectl port-forward service/luz-register 8087:8080 -n dev-vn
        kubectl port-forward service/luz-booking 9080:8080 -n dev-vn
        kubectl port-forward service/luzfin-finance 8088:8080 -n dev-vn
        kubectl port-forward service/ivy 8089:8080 -n dev-vn
        kubectl port-forward service/luz-tenant 8090:8080 -n dev-vn
        kubectl port-forward service/key-cloak 8091:8080 -n dev-vn
        kubectl port-forward service/luz-compensation 8092:8080 -n dev-vn
        kubectl port-forward service/luz-person 8093:8080 -n dev-vn
        kubectl port-forward service/luz-accounting 8094:8080 -n dev-vn
        kubectl port-forward service/luz-store 8095:8080 -n dev-vn
        kubectl port-forward service/luz-database 5555:5432 -n dev-vn   3. Logs console:
        kubectl get pods -n dev-vn
        kubectl logs -f -n dev-vn luz-mobile-5b68dd646-9dhz8 --tail=2000
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Port forward and Docker compose]]
- [[Local access to GKE-hosted Luz services port-forward api-forwarder + luz-vault (+ mongo pod)]]
- [[OpenAPI UI (API on SwaggerUI)]]
- [[GCP - Connect Database]]
- [[Load test]]

%% ai-graph-end %%