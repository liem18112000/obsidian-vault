---
ai_hash: 3cb9c998284e2720
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 10
depth: 2.72
entities: []
relevance: 0.701
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47398814200/One+API+end+to+end+testing.
space: LUZ
status: reference
tags:
- confluence
- testing
- space/luz
title: One API end to end testing.
topic: testing
type: source
updated: 2024-03-11
---

# One API end to end testing.

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-03-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47398814200/One+API+end+to+end+testing.)
> Relevance 0.701 · topic `testing`

This page document test case for end to end testing

# Related PRs

luz_kubernetes performance branch: <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/load-test-with-scaling" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/load-test-with-scaling</a>

public API branch for create data for test: <a href="https://bitbucket.org/axonivy-prod/luz_public_api_adapter/branch/load-test-services" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_public_api_adapter/branch/load-test-services</a>

# Setup steps

### Postman Collection

- support to send delivery with specific tenant - public_api_adapter branch was updated 9.11.23

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="306f304a-1a39-44c6-9f36-0b3e20e500c3" macro-name="view-file"><a href="../_attachments/47398814200-Load Test large concurrent sending requests.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47398814200/Load%20Test%20large%20concurrent%20sending%20requests.postman_collection.json?version=1&amp;modificationDate=1699959846817&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47398814200-Load Test large concurrent sending requests.postman_collection.json]]

</a></span>

- standards api for load test (updated 24/11/2023)

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="1dba7f94-cf04-46f2-9e79-3a72aa34ff19" macro-name="view-file"><a href="../_attachments/47398814200-Load test.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47398814200/Load%20test.postman_collection.json?version=1&amp;modificationDate=1700818174365&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47398814200-Load test.postman_collection.json]]

</a></span>

### Build jenkins

<a href="https://build.axongroupio.ch/job/KLARA/job/gcp-performance-deployment/" class="external-link" rel="nofollow">https://build.axongroupio.ch/job/KLARA/job/gcp-performance-deployment</a>

branch: **load-test-with-scaling**

after build job finish, delete unused stuff:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="db2bd5cf-a51d-423b-942e-75b831f93a6a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud container clusters get-credentials klara-performance --zone europe-west6-a --project klara-performance
```

</div>

</div>

<div id="expander-58704269" class="expand-container conf-macro output-block" hasbody="true" macro-id="635ee9ac-a1e1-4ef0-940d-e92467632496" macro-name="expand">

<div id="expander-control-58704269" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-58704269" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8d5160ae-9db0-4726-8080-0e829bcd19c2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl -n performance delete deployment luz-article-image-clean-up 
kubectl -n performance delete deployment luz-asset
kubectl -n performance delete deployment luz-booking

kubectl -n performance delete deployment luz-corapi
kubectl -n performance delete deployment luz-creditcard
kubectl -n performance delete deployment luz-creditcard-transactions

kubectl -n performance delete CronJob batch-normalize-cache-extract-job 
kubectl -n performance delete CronJob batch-normalize-cache-process-job 
kubectl -n performance delete CronJob billing-delivered-documents 
kubectl -n performance delete CronJob clean-up-date-address-batch-request-job 
kubectl -n performance delete CronJob clean-up-processed-zips 
kubectl -n performance delete CronJob deactivate-expired-scanning-addresses 
kubectl -n performance delete CronJob earchive-widget-consumption-billing-monthly-job   
kubectl -n performance delete CronJob epost-api-consumption-billing-job 
kubectl -n performance delete CronJob epost-data-monitor-volumn-cronjob 
kubectl -n performance delete CronJob export-audit-files-daily-cron-job 
kubectl -n performance delete CronJob fetching-not-scannable-documents 
kubectl -n performance delete CronJob fetching-scanned-documents 
kubectl -n performance delete CronJob fetching-scanning-tenants 
kubectl -n performance delete CronJob health-check 
kubectl -n performance delete CronJob import-public-holiday-job 
kubectl -n performance delete CronJob individual-inactive-user-retention-job 
kubectl -n performance delete CronJob individual-user-address-verify-retention-job 
kubectl -n performance delete CronJob reporter-billing-monthly-job

kubectl -n performance delete CronJob klara-uptime-monitoring

kubectl -n performance delete deployment luz-about
kubectl -n performance delete deployment luz-creditsuisse 
kubectl -n performance delete deployment luz-fortuna 
kubectl -n performance delete deployment luz-google
kubectl -n performance delete deployment luz-jobalino
kubectl -n performance delete deployment luz-kmplus


kubectl -n performance delete deployment earchive-widget-consumption-billing-monthly-job    
kubectl -n performance delete deployment reporter-billing-monthly-job

kubectl -n performance delete deployment epost-api-consumption-billing-job 
kubectl -n performance delete deployment epost-data-monitor-volumn-cronjob 
kubectl -n performance delete deployment export-audit-files-daily-cron-job 
kubectl -n performance delete deployment fetching-not-scannable-documents 
kubectl -n performance delete deployment fetching-scanned-documents 
kubectl -n performance delete deployment fetching-scanning-tenants

kubectl -n performance delete deployment import-public-holiday-job 
kubectl -n performance delete deployment individual-inactive-user-retention-job 
kubectl -n performance delete deployment individual-user-address-verify-retention-job 

kubectl -n performance delete deployment luz-ebill

kubectl -n performance delete deployment luz-finnova

kubectl -n performance delete deployment klara-uptime-monitoring 
kubectl -n performance delete deployment klee-event-broker 

kubectl -n performance delete deployment luz-kmuplus

kubectl -n performance delete deployment luz-marketing
kubectl -n performance delete deployment luz-mobile
kubectl -n performance delete deployment luz-mobiliar

kubectl -n performance delete deployment luz-notification-center 

kubectl -n performance delete deployment luz-okomo
kubectl -n performance delete deployment luz-online
kubectl -n performance delete deployment luz-pffds

kubectl -n performance delete deployment luz-online-web 

kubectl -n performance delete deployment luz-public-api-adapter-messaging

kubectl -n performance delete deployment luz-retention
kubectl -n performance delete deployment luz-scancenter

kubectl -n performance delete deployment luz-smartletter
kubectl -n performance delete deployment luz-smartform

kubectl -n performance delete deployment luz-sua
kubectl -n performance delete deployment luz-smart-letter 

kubectl -n performance delete CronJob luz-tenant-dir-cronjob-matching 
kubectl -n performance delete CronJob luz-tenant-dir-cronjob-normalizing 

kubectl -n performance delete deployment luz-vaudoise

kubectl -n performance delete deployment migration-normalizing-name 

kubectl -n performance delete deployment send-billing-data


kubectl -n performance delete deployment sftp-server
kubectl -n performance delete deployment luz-suva

kubectl -n performance delete deployment luz-time-management 

kubectl -n performance delete Job luzfin-scripts 
kubectl -n performance delete Job migration-normalizing-name 
kubectl -n performance delete deployment nfs-server 
kubectl -n performance delete deployment nfs-tmpfile-server

kubectl -n performance delete CronJob polling-delivery-status 
kubectl -n performance delete CronJob polling-redirection-order-status

kubectl -n performance delete CronJob recurring-invoice-template-job 
kubectl -n performance delete CronJob extend-time-period-job
kubectl -n performance delete CronJob refresh-address-cache-job 
kubectl -n performance delete CronJob restart-pods-cronjob 
kubectl -n performance delete CronJob resume-suspended-deliveries-v2 
kubectl -n performance delete CronJob send-billing-data 
kubectl -n performance delete CronJob send-push-notification-to-verify-address-job
kubectl -n performance delete deployment webclient-maintenance

#kubectl -n performance delete deployment emcm
#kubectl -n performance delete deployment eom
#kubectl -n performance delete deployment spay-adapter
#kubectl -n performance delete deployment luz-reporting
#kubectl -n performance delete deployment luz-reporting-dotnet
#kubectl -n performance delete deployment luz-viewgen
#kubectl -n performance delete deployment luz-elm
```

</div>

</div>

</div>

</div>

**Note**: make sure this job is stopped else it will cause this issue:


![[47398814200-image-20230608-054926.png]]



Job name:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b38acbda-2570-4ad0-b881-a6193340497b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
send-reminder-pn-for-document-job
```

</div>

</div>


![[47398814200-image-20230608-054548.png]]



<a href="https://console.cloud.google.com/kubernetes/cronjob/europe-west6-a/klara-performance/performance/send-reminder-pn-for-document-job/details?project=klara-performance" class="external-link" rel="nofollow">https://console.cloud.google.com/kubernetes/cronjob/europe-west6-a/klara-performance/performance/send-reminder-pn-for-document-job/details?project=klara-performance</a>

### Local machine setup

build maven and run these port forward command:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="88179ded-41e0-4102-ad75-d9cca476e7f9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud container clusters get-credentials klara-performance --zone europe-west6-a --project klara-performance

kubectl port-forward service/luz-public-api-adapter 8135:8080 -n performance
#kubectl port-forward service/luz-public-api-adapter 8085:8080 -n performance

kubectl port-forward service/api-forwarder -n performance 8080:8080

#if need access db:
kubectl port-forward service/luz-database -n performance 2345:5432
```

</div>

</div>

for the public api branch, run:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5b9e1ab3-fe36-4122-8fbb-50a43f4b7b18" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
mvn clean install -Dmaven.antrun.skip -DskipUTs -DskipITs -DskipTests

docker-compose -f docker-compose-load-test.yml up --build luz-public-api-adapter-load-test

-> postman use 8135 port
```

</div>

</div>

<span class="inline-comment-marker" ref="88b5a1ee-0a74-4c7b-9f55-868d6d862f1f">use this postman collection:</span>

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="0a8e6158-a6bb-4a94-823d-56d13f495769" macro-name="view-file"><a href="../_attachments/47398814200-Load_test_end_to_end_latest.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47398814200/Load_test_end_to_end_latest.postman_collection.json?version=1&amp;modificationDate=1686821709401&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47398814200-Load_test_end_to_end_latest.postman_collection.json]]

</a></span>

### Some SQL can call to follow the process of delivery:

- Query to list delivery, document, recipient:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8bc10afa-bf4b-42bd-8387-13b637490e49" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
select de.id as delivery_id, d.error_code as document_error_code, d.status as document_status, r.error_code as recipient_error_code, r.status as recipient_status, count(d.status)
from s_b2797b6d_0f27_4250_9406_24c586fdb81b.delivery de
join s_b2797b6d_0f27_4250_9406_24c586fdb81b.document d
on de.id = d.delivery_id
join s_b2797b6d_0f27_4250_9406_24c586fdb81b.recipient_tracking r
on r.document_id = d.id
-- where de.id in (40049,40061,40057)
where de.id = 20
-- and d.id in (516396, 515527)
-- and d.error_code is not null
-- and d.status in ('FAILED_TO_RETRIEVE', 'FAILED_TO_STORE', 'FAILED_TO_DELIVER')
-- and d.status !='DELIVERED'
-- and d.status != 'PUBLISHED_TO_RECIPIENT_SENDING_QUEUE'
group by de.id,d.error_code, d.status, r.error_code, r.status
```

</div>

</div>

- Query stop time of last recipient belong to delivery

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="afe23025-0a78-4904-8230-f0b91f39b567" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
-- document level
SELECT id, create_date, update_date, status FROM s_b2797b6d_0f27_4250_9406_24c586fdb81b.document 
where delivery_id = 25 
--and status != 'DELIVERED'
ORDER BY update_date DESC;

--recipient level
SELECT 
-- count (*)
a.*
FROM s_b2797b6d_0f27_4250_9406_24c586fdb81b.recipient_tracking a
    join s_b2797b6d_0f27_4250_9406_24c586fdb81b.document b on a.document_id = b.id
    join s_b2797b6d_0f27_4250_9406_24c586fdb81b.delivery c on b.delivery_id = c.id
where c.id = 34
and a.update_date is not null
-- and (a.status = 'FAIL' OR a.status is null)
ORDER BY a.update_date DESC LIMIT 100;
```

</div>

</div>

### Clear all the message enricher before start new test:

<a href="https://console.cloud.google.com/cloudpubsub/subscription/list?project=klara-performance&amp;tab=messages&amp;pageState=(%22cpsSubscriptionList%22:(%22f%22:%22%255B%257B_22k_22_3A_22_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22enrich_5C_22_22%257D%255D%22),%22duration%22:(%22groupValue%22:%22PT1H%22,%22customValue%22:null))&amp;authuser=6" class="external-link" rel="nofollow">Enricher subscriptions.</a>


![[47398814200-image-20230614-025107.png]]



For arrow topic:

- `performance-delivery-status`

- `performance-document-sending`

- `performance-document-status`

- `performance-recipient-sending`

# Test the marketing case setup

## Branches:

<a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/ARROW-99/LUZ-112970_loadtest_print_99?dest=future%2FLUZ-112907-upgrade-ivy-10" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/ARROW-99/LUZ-112970_loadtest_print_99?dest=future%2FLUZ-112907-upgrade-ivy-10</a>  
<a href="https://bitbucket.org/axonivy-prod/luz_public_api_adapter/branch/load-test-services" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_public_api_adapter/branch/load-test-services</a>

## Apply latest config for just specific module

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="480ce6f1-6a85-4593-a17e-39c1eb7a906f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker run -it -v D:\klara\work\workspace_ivy_9\luz_kubernetes:/root/development/luz_kubernetes gcr.io/klara-repo/luz-deploy:latest bash
cd root/development/luz_kubernetes
./deploy_to_stdout.sh performance > luz-deploy-performance.yaml

#exit

kubectl apply -l klara.ch/module=luz-public-api-adapter -f luz-deploy-performance.yaml -n performance --dry-run=server

kubectl apply -l klara.ch/module=luz-eletter -f luz-deploy-performance.yaml -n performance --dry-run=server
kubectl apply -l klara.ch/module=luz-docs-view-controller -f luz-deploy-performance.yaml -n performance --dry-run=server
kubectl apply -l klara.ch/module=luz-docs -f luz-deploy-performance.yaml -n performance --dry-run=server
```

</div>

</div>

## Curls

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="89d97fcb-9bae-45ed-8084-94138650540c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#port-forward
kubectl port-forward service/luz-public-api-adapter-loadtest 8085:8080 -n performance

#curl:
curl --location 'http://localhost:8085/load-test/marketing-delivery?costCenter=phi-test-02-02-2024-case-5k-no-001&to=5000&sync=false'
```

</div>

</div>

## Update 11/03/24 - script to support marketing cases (multiple task submiting)  <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="a84a7550-ba89-4b4b-ab8c-f58fe2415cb2" macro-name="view-file"><a href="../_attachments/47398814200-Main.java" class="confluence-embedded-file" data-nice-type="Java Source File" data-file-src="/wiki/download/attachments/47398814200/Main.java?version=2&amp;modificationDate=1710147287802&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47398814200-Main.java]]

</a></span> → configure and run this main class


![[47398814200-image-20240311-085557.png]]

%% ai-graph-start %%

**Related notes:**
- [[Load test]]
- [[EPC API - Load Test]]
- [[One API load test with locust]]
- [[Post-deployment Batch messageCount backfill (Test & Prod) — LUZ-155431 LUZ-155435]]
- [[Infrastructure]]

%% ai-graph-end %%