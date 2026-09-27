---
title: "Load Test Physical Order API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/14806965299/Load+Test+Physical+Order+API
space: "HACKA"
topic: programming
relevance: 0.786
depth: 3
updated: 2021-07-20
attachments: 87
tags:
  - confluence
  - programming
  - space/hacka
---

# Load Test Physical Order API

> [!info] Imported from Confluence
> Space **HACKA** · updated 2021-07-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/14806965299/Load+Test+Physical+Order+API)
> Relevance 0.786 · topic `programming`

Environment: DEV

Tool: Jmeter

<span class="legacy-color-text-default">luz-scancenter</span>

<span class="legacy-color-text-default">          limits:</span>  
<span class="legacy-color-text-default">            cpu: "6"</span>  
<span class="legacy-color-text-default">            memory: 5Gi</span>  
<span class="legacy-color-text-default">          requests:</span>  
<span class="legacy-color-text-default">            cpu: 200m</span>  
<span class="legacy-color-text-default">            memory: 5Gi</span>

Test account: <a href="mailto:hcmc-hacka@axonactive.com" class="external-link" rel="nofollow">hcmc-hacka@axonactive.com</a>  
Tenant ID: 7d1d4753-dfa6-499c-bec7-f5e961e05538

Testing API: POST <span class="resolvedVariable" style="text-decoration: none;">http://{luz_scancenter_url}</span>/api/<span class="resolvedVariable" style="text-decoration: none;">{tenant_id}</span>/epost/documents/physical-order

  
Step 1: Port-forward jwt-service and luz-scancenter

Step 2: Get tenant token

Step 3: Call physical order API with the token

Result:

- TestCase: 50 concurrent requests   
  

![[14806965299-image2021-7-20_11-42-5.png]]



  

- TestCase: 100 concurrent requests =\> Some requests are success and the others ran into issue: "<span class="legacy-color-text-red2">Connection reset</span>".  
  

![[14806965299-image2021-7-20_12-0-25.png]]

  
    
- Noted: Cannot load test with 50 concurrent requests anymore once it ran into the issue.
