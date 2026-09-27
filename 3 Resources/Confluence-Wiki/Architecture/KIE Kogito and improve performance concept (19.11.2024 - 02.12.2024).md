---
ai_hash: 5b43f87dd2a86fc2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48187015672/KIE+Kogito+and+improve+performance+concept+19.11.2024+-+02.12.2024
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: KIE Kogito and improve performance concept (19.11.2024 - 02.12.2024)
topic: architecture
type: source
updated: 2024-12-02
---

# KIE Kogito and improve performance concept (19.11.2024 - 02.12.2024)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-12-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48187015672/KIE+Kogito+and+improve+performance+concept+19.11.2024+-+02.12.2024)
> Relevance 0.711 · topic `architecture`

# 1. Kie Sandbox

To convenience rule editing with Kogito, we have researched and documented various methods for editing rules using **KIE Sandbox** and **Code Server**.

## 1.1 Kie Sandbox

<div>

|  |  |
|----|----|
| Link | <a href="https://rules-dev.klara.tech/kie-sandbox" class="external-link" rel="nofollow">https://rules-dev.klara.tech/kie-sandbox</a> |
| Guideline | <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48155525261/Guide+to+use+Kogito+Sandbox+for+business+people+important?search_id=ed55493e-1ca4-4bea-9454-a20b11b17a83" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48155525261/Guide+to+use+Kogito+Sandbox+for+business+people+important?search_id=ed55493e-1ca4-4bea-9454-a20b11b17a83</a> |

</div>

## 1.2 Code Server (Visual Code Browser)

<div>

|  |  |
|----|----|
| Link (DMN project) | <a href="https://rules-dev.klara.tech/luz-rule-frontend/?folder=/home/coder/project" class="external-link" rel="nofollow">https://rules-dev.klara.tech/luz-rule-frontend/?folder=/home/coder/project</a> |
| Link (Legacy API - DRL) | <a href="https://rules-dev.klara.tech/luz-rule-frontend/?folder=/home/coder/drl" class="external-link" rel="nofollow">https://rules-dev.klara.tech/luz-rule-frontend/?folder=/home/coder/drl</a> |
| Guideline | [Guide to use VSCode for Business people \[important\]](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48161194240/Guide+to+use+VSCode+for+Business+people+important) |

</div>

## 1.3 Documentation for converting **GDST/DRL** to **DMN**

[GDST/DRL to DMN rule convert guide](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48176399108/GDST+DRL+to+DMN+rule+convert+guide)

[Migrating DRL from Drools 7 to Drools 8](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48177021035/Migrating+DRL+from+Drools+7+to+Drools+8)

[Customized Logic for Migrating from DRL & GDST to DMN (POC version only)](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48186753087/Customized+Logic+for+Migrating+from+DRL+GDST+to+DMN+POC+version+only)

# 2. Invoice Run 2

We found an issue where running invoices for company tenants could block the process for individual tenants.

To fix this, we updated the **Invoice Run 2** workflow so that company and individual tenant invoices can run without interfering with each other.

<div>

|  |  |
|----|----|
| **Individual Tenant** | **Company Tenant** |
| 

![[48187015672-image-20241202-030505.png]]

 | 

![[48187015672-image-20241202-030545.png]]

 |
|  |  |

</div>

# 3. Luz-docs

We are researching the concept of distributed transactions and developing a POC to enhance how **luz-docs** processes document information. This aims to improve data consistency and optimize service calls within **luz-docs**, reducing unnecessary requests to other services.

The POC covers several points, including updates to the **Enrichment workflow** and using **Redis cache** to reduce the load on **luz-jsonstore**.

Detail: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48171712551/Apply+Redis+for+caching+and+Create+API+V2" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48171712551/Apply+Redis+for+caching+and+Create+API+V2</a>

%% ai-graph-start %%

**Related notes:**
- [[Editor rule by Code Service (Visual Code in Browser)]]
- [[Confluence Export — Index]]
- [[CICD for Kogito]]
- [[LUZ Audit Refactor- 2025-2026]]
- [[Programming]]

%% ai-graph-end %%