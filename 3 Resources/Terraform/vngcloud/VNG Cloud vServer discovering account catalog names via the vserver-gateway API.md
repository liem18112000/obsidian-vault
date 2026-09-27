---
ai_hash: 600593f5c8bce155
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18 leo-customer360 deployments/server
status: seedling
tags:
- terraform
- vngcloud
- greennode
- vserver
- api
- oauth2
- gotcha
title: 'VNG Cloud vServer: discovering account catalog names via the vserver-gateway
  API'
type: howto
---

# VNG Cloud vServer: discovering account catalog names via the vserver-gateway API

When Terraform `vngcloud_vserver_*` data sources fail with `not found ... with name X`, the fix is to discover the real per-account catalog names via the vserver-gateway REST API. All GET, base = `vserver_base_url` (default `https://hcm-3.api.vngcloud.vn/vserver/vserver-gateway`).

## Auth (the non-obvious part)
Token endpoint `https://iamapis.vngcloud.vn/accounts-api/v2/auth/token` is OAuth2 **client-credentials**, and the SDK (`golang.org/x/oauth2/clientcredentials`) sends the client id/secret via **HTTP Basic auth header** with body `grant_type=client_credentials&scope=email`. Gotchas found empirically:
- client_id/secret in the *body* → `AUTHENTICATION_FAILED` (must be Basic header).
- `scope` is required and must be non-empty; the SDK uses `scope=email`.
Response has `access_token`; then call APIs with `Authorization: Bearer <token>`.

## List endpoints (path uses project id `pro-...`)
- Flavor zones:  `GET /v1/{project}/flavor_zones/product` → `{flavorZones:[{name,id}]}`
- Flavors in a zone: `GET /v1/{project}/{flavor_zone_id}/flavors` → `{flavors:[{name,...}]}`
- Volume-type zones: `GET /v1/{project}/volume_type_zones` → `{volumeTypeZones:[{name,id}]}`
- Volume types in a zone: `GET /v1/{project}/{volume_type_zone_id}/volume_types` → `{volumeTypes:[{name,iops,minSize,maxSize}]}`
- OS images: `GET /v1/{project}/images/os` → `{images:[{imageVersion,imageType,id,flavorZoneIds}]}`

## Which JSON field each Terraform input matches
- `flavor_zone_name`      ⇒ flavorZones[].name
- flavor (`servers[].flavor_name`) ⇒ flavors[].name (listed under a flavor zone)
- `volume_type_zone_name` ⇒ volumeTypeZones[].name
- `root_disk_type_name`   ⇒ volumeTypes[].name (under a volume-type zone)
- `image_name`            ⇒ images[].**imageVersion** (NOT a `name` field), AND the image is only valid if its `flavorZoneIds` contains the chosen flavor zone id.

## Why it matters
These data sources ERROR on a name miss (not empty id), so `try(...id,"")` preconditions cannot catch them — the names must be exact. Catalog names are per-account/zone, so hardcoded guesses (`General v2 Instances`, `SSD-IOPS3000`) fail. A read-only discovery script (`deployments/server/discover-catalog.py`) enumerates all of the above.

## Related
[[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

## Related

- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vServer OS images are not associated with the s2-general flavor zone (image data-source trap)]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]
- [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]]
- [[VNG Cloud list-projects endpoint is GET vserver-gatewayv1projects (not accounts-api)]]
- [[VNG Cloud IaC = Terraform provider (no first-party CLI); vStorageregistry via S3+docker]]

%% ai-graph-end %%