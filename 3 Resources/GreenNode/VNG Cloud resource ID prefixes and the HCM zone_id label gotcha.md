---
ai_hash: 220c45619bc7efff
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- vngcloud
- greennode
- zone
- region
- ids
- gotcha
title: VNG Cloud resource ID prefixes and the HCM zone_id label gotcha
type: lesson
---

# VNG Cloud resource ID prefixes and the HCM zone_id label gotcha

VNG Cloud / GreenNode resource IDs carry a type prefix: instance = ins-, project = pro-, network = net-, subnet = sub-, vDB instance = db-, Kafka cluster = clus-. The Terraform input vserver_project_id needs the **pro-** value, which is NOT shown on a cloud servers "General information" panel (that shows the ins- instance ID). Find pro- in the console project selector or the browser URL projectId= query param, and pick the project that owns your network/subnet.

Zone label gotcha: the console shows a friendly zone label like "HCM-1A", but the Terraform/API zone_id token is region-qualified, e.g. HCM03-1A (allowed: HCM03-1A/-1B/-1C). The console host also tells you the region (hcm-3.console.greennode.ai = HCM03). Keep zone_id + vng_vserver_base_url (hcm-3.api...) aligned to where your network/compute live, and co-locate vDB with the app. vStorage region may differ since S3 buckets are reached by endpoint, not VPC.

## Related
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]

%% ai-graph-start %%

**Related notes:**
- [[Customer360 GreenNode region split compute HCM03, vStorage HCM04]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]
- [[VNG Cloud list-projects endpoint is GET vserver-gatewayv1projects (not accounts-api)]]
- [[VNG vServer default catalog endpoints return the DISABLED default AZ; use zoneId=AZ and pin UUIDs]]
- [[VNG Cloud Terraform provider maps managed Postgres and Redis to vdb resources]]

%% ai-graph-end %%