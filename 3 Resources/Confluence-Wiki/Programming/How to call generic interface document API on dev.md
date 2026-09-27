---
ai_hash: 0fa14c41577a783e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.49
entities: []
relevance: 0.729
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47470215362/How+to+call+generic+interface+document+API+on+dev
space: HACKA
status: reference
tags:
- confluence
- programming
- space/hacka
title: How to call generic interface document API on dev
topic: programming
type: source
updated: 2023-08-25
---

# How to call generic interface document API on dev

> [!info] Imported from Confluence
> Space **HACKA** · updated 2023-08-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47470215362/How+to+call+generic+interface+document+API+on+dev)
> Relevance 0.729 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="72fe8fec-a018-4b77-8862-f3486bc421fd" macro-name="toc">

</div>

# 1-Port forward

Get credentials for dev environment:

gcloud container clusters get-credentials klara-nonprod --zone europe-west6-a --project klara-nonprod

Port forward API service  
kubectl port-forward service/api-forwarder 8080:8080 -n dev

Port forward database if you want to modify database

kubectl port-forward service/luz-database 6543:5432 -n dev

# 2-Call API via postman

Find your tenant Id and get token via API (replace with your tenant Id)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7f13de2d-938c-4945-a35a-8944e4cb3b12" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
http://localhost:8080/luzsec/api/867f28da-25f0-4e55-876d-03544c1dd62b/tokens
```

</div>

</div>


![[47470215362-image-20230825-083535.png]]



Generate file (replace with your tenant Id)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2bd872f7-690e-4431-b569-4cf8766e8982" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
http://localhost:8080/luz_compensation/api/79ae64ad-0178-45fb-b57e-52bad81bc056/companies/1/accounting-interfaces/generic-interface-document
```

</div>

</div>


![[47470215362-image-20230825-082647.png]]



Input query parameter

salaryRunId or payslipIds

Set Accept-Language in Header tab


![[47470215362-image-20230825-082907.png]]



# 3-Modify database via DBeaver

Make sure the port is the same as port forward command above


![[47470215362-image-20230825-094523.png]]



Accounting Interface tables are located in luz_compensation → tenant specific schema:


![[47470215362-image-20230825-083224.png]]

%% ai-graph-start %%

**Related notes:**
- [[Use KLARA Swagger UI for REST API]]
- [[Port Forward to call GCP API in localhost]]
- [[Research Design architecture concept for the service to generate the Generic Interface File]]
- [[How to Start Invoice Run v2]]
- [[OpenAPI UI (API on SwaggerUI)]]

%% ai-graph-end %%