---
ai_hash: e3001d47b0ae1327
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 3
entities: []
relevance: 0.926
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48491233533/One+API+load+test+with+locust
space: HACKA
status: reference
tags:
- confluence
- programming
- space/hacka
title: One API load test with locust
topic: programming
type: source
updated: 2025-05-13
---

# One API load test with locust

> [!info] Imported from Confluence
> Space **HACKA** · updated 2025-05-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48491233533/One+API+load+test+with+locust)
> Relevance 0.926 · topic `programming`

<a href="https://bitbucket.org/axonivy-prod/locust-load-test/src/master/" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/locust-load-test/src/master/</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="84452aa6-1280-416c-876a-b8727622319a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
### How do I get set up? ###

* Clone/Pull the source code
* Create a new Python script in `scripts` folder for testing your API (referance to CreatePdf.py for example)
* Update the `Dockerfile` to match your new script
* Commit your code
* Build the project on your desired server at: `https://console.cloud.google.com/cloud-build/triggers?inv=1&invt=AbxP1g&project=klara-infra&pageState=(%22triggers%22:(%22f%22:%22%255B%257B_22k_22_3A_22_22_2C_22t_22_3A10_2C_22v_22_3A_22_5C_22one_5C_22_22%257D%255D%22))`
* Deploy that code: `https://build.axongroupio.ch/job/KLARA/job/gcp-performance-deploy-individually/759/`
* Port-forward for port 8089: `kubectl port-forward service/locust-load-test -n <gcp_namespace> <your_desired_port>:8089`, `gcp_namespace` is either `dev`, `dev-vn`, or `performance`, depending of what server you choosed to build the project in previous step
* Go to `localhost:<your_desired_port>` and run your test
```

</div>

</div>


![[48491233533-image-20250513-035916.png]]

![[48491233533-image-20250513-040100.png]]

![[48491233533-image-20250513-040117.png]]

%% ai-graph-start %%

**Related notes:**
- [[Load test]]
- [[One API end to end testing]]
- [[Port Forward to call GCP API in localhost]]
- [[Recipe Deploy with Terraform]]
- [[Infrastructure]]

%% ai-graph-end %%