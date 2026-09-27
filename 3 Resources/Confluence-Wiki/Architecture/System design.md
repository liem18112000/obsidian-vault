---
ai_hash: 48f571c1af0d2224
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47159345567/System+design
space: HACKA
status: reference
tags:
- confluence
- architecture
- space/hacka
title: System design
topic: architecture
type: source
updated: 2022-10-26
---

# System design

> [!info] Imported from Confluence
> Space **HACKA** · updated 2022-10-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47159345567/System+design)
> Relevance 0.711 · topic `architecture`

![[47159345567-LUZ-83376-sysem-design.png]]



**Modify the flow of getting recipient list to add a new source which is from filtering selections.**  
**Flow:** If the CSV file is not provided then try to get recipient list from filtering selections.  
There are two main cases that affect to the code flow:

- **Normal case:**

  - Get a list of recipients filtering by scanning subscription (call to luz_store)

  - Filter the retrieved recipients list again by Tenant Type and Language (call to luz_tenant_dir).

- **Special case(\*):**

  - Get all private tenants having scanning subscription (call to luz_store).

  - Get recipients list filtering by Tenant Type, language and excluding above tenant list. (call to luz_tenant_dir).

**(\*)** Enabled toggles: No Scanning subscription and Private Tenant

In this special case, luz_store doesn’t store any data about private tenant because all private tenants have digital letter box by default.  
So the tenant list will be retrieved by this way: **All private tenants - private tenants having scanning subscription.**

%% ai-graph-start %%

**Related notes:**
- [[Evaluation of final solution including implementation needs]]
- [[OneAPI Architecture overview]]
- [[One API Module Responsibilities]]
- [[Architecture]]
- [[Public API (30 August 2021)]]

%% ai-graph-end %%