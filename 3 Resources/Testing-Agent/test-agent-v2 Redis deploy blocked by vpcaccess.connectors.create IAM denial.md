---
ai_hash: 55bb3ac0df471a46
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: Testing-Agent deploy 3a39108-report
status: seedling
tags:
- testing-agent
- deploy
- terraform
- iam
- redis
- gotcha
title: test-agent-v2 Redis deploy blocked by vpcaccess.connectors.create IAM denial
type: lesson
---

# test-agent-v2 Redis deploy blocked by vpcaccess.connectors.create IAM denial

Deploying **test-agent-v2 with `deploy_redis=true`** fails for an account without VPC-connector-admin rights:

> Error creating Connector: Error 403: Permission **`vpcaccess.connectors.create`** denied on projects/klara-nonprod/locations/europe-west6 ... IAM_PERMISSION_DENIED — with `google_vpc_access_connector.redis[0]` (redis.tf:26)

`redis.tf` creates a NEW `google_vpc_access_connector` so Cloud Run can reach the Memorystore (`kga-v2-cache`). Creating a Serverless VPC Access connector needs `roles/vpcaccess.admin` (or `vpcaccess.connectors.create`), which `lam.nguyen@axonactive.com` lacks on klara-nonprod. The Memorystore instance itself CAN be created (that succeeded), but the connector can't — so the Redis wiring never completes.

**Partial-apply behaviour:** the connector is created early, and Cloud Run services whose `vpc_access` references it depend on it — so the failed apply left the stack SPLIT: services that don't need the connector updated to the new image, while the connector + the services that depend on it (the cache consumer + anything ordered after the error) stayed on the old image. Re-running hits the same 403.

**Ways past it:** (a) admin grants `roles/vpcaccess.admin` to the deploy account, then re-deploy; (b) `deploy_redis=false` — skip Redis entirely (cache falls back to no-op, GCS stays source of truth), but this TEARS DOWN the already-provisioned Memorystore; (c) point `redis.tf` at an EXISTING connector (e.g. `cr-cloudsql-connector`, 10.8.0.0/28) instead of creating one, if it reaches the Redis network.

## Related

- [[Concurrent test-agent-v2 deploys collide on terraform local-state lock (fails safe)]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 Redis VPC connector stuck in ERROR (CIDRnetwork misconfig)]]
- [[redis_proxy.sh needs compute firewall + VM + IAP permissions]]
- [[test-agent-v2 Redis cache port + Memorystore needs a VPC connector]]
- [[test-agent-v2 hardened deploy.sh flow and the unique image-tag bump that forces a new revision]]
- [[Concurrent test-agent-v2 deploys collide on terraform local-state lock (fails safe)]]

%% ai-graph-end %%