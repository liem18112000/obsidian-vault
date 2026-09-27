---
title: "System design"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47159345567/System+design
space: "HACKA"
topic: architecture
relevance: 0.711
depth: 2.44
updated: 2022-10-26
attachments: 2
tags:
  - confluence
  - architecture
  - space/hacka
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
