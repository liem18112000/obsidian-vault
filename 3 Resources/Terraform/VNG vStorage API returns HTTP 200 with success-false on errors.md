---
ai_hash: b5779d74d6c60789
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- vngcloud
- vstorage
- rest-api
- gotcha
- error-handling
title: VNG vStorage API returns HTTP 200 with success-false on errors
type: lesson
---

# VNG vStorage API returns HTTP 200 with success-false on errors

The VNG Cloud vStorage control-plane REST API (and other VNG APIs of the same family) returns **HTTP 200 even when the operation logically failed**. Success/failure is signalled in the JSON body, not the status line:

```json
{"success": false, "errorMsg": "Invalid input error (projectName must be provided; quotaInGBytes must be provided; projectType must be provided)", "code": 112, "data": null}
```

**Consequence for scripts/clients:** checking only the HTTP status (or using `curl -f`) is not enough — a create can "200 OK" while creating nothing. Always parse the body and branch on `success` (and surface `errorMsg`). A naive client that trusts the 200 will report a phantom success. On the flip side, `curl -f` (fail-on-4xx) never triggers here, so you also must not rely on it to catch these errors.

Discovered while writing deployments/storage/scripts/create-project.sh for leo-customer360: the create returned 200 with success:false because the body used the wrong field names.

Related: [[vStorage REST control-plane API endpoints and vIAM bearer auth|vStorage REST control-plane API: endpoints and vIAM bearer auth]].

## Related

- [[vStorage REST control-plane API endpoints and vIAM bearer auth|vStorage REST control-plane API: endpoints and vIAM bearer auth]]

%% ai-graph-start %%

**Related notes:**
- [[vStorage REST control-plane API endpoints and vIAM bearer auth]]
- [[vStorage API project creation needs a billing order (payment method or POC wallet)]]
- [[vStorage create-project code 114 is account-side, not a payload bug]]
- [[Creating a vStorage bucket is free (data-plane); only the project quota + usage cost money]]
- [[Deleting a prepaid cloud resource does not auto-refund; failed-provision charges need a manual support refund]]

%% ai-graph-end %%