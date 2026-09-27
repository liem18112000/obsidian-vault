---
title: "test-agent-v2 Redis VPC connector stuck in ERROR (CIDR/network misconfig)"
created: 2026-09-16
type: lesson
status: seedling
source: "Testing-Agent deploy 3a39108-report"
tags: [testing-agent, deploy, terraform, redis, vpc-connector, gotcha]
---

# test-agent-v2 Redis VPC connector stuck in ERROR (CIDR/network misconfig)

After the IAM grant cleared the 403, the test-agent-v2 Redis deploy hit a SECOND blocker: the Serverless VPC Access connector `kga-v2-cache-conn` (redis.tf, `name = "${var.name_prefix}-cache-conn"`) was created but landed in **STATE: ERROR** — on network `default` with `ip_cidr_range = 10.8.0.0/28`, the SAME /28 as the existing READY `cr-cloudsql-connector` (which is on `luz-custom-network-cloud-nat`).

Symptoms/chain:
- First apply (post-grant) submitted the async connector create; the create FAILED server-side (ERROR state) and/or terraform didn't record it. The retry apply then tried to create it again → `Error 409: Requested entity already exists`.
- **Importing won't fix it** — an ERROR-state connector is broken; it must be DELETED and recreated with a valid config.
- Likely root cause: wrong `redis_network` (`default` instead of the real VPC `luz-custom-network-cloud-nat`) and/or a `vpc_connector_cidr` that collides (10.8.0.0/28 is already taken by cr-cloudsql-connector). A VPC connector needs a free /28 on the network that can reach the Memorystore (`kga-v2-cache` @ 10.161.251.187).

**To unblock the non-Redis deliverables** (fixes + report), `deploy_redis=false` drops the connector requirement so all Cloud Run services can update to the new image; the ERROR connector is orphaned (not in TF state, since the 409 proves terraform never recorded it) and must be deleted manually: `gcloud compute networks vpc-access connectors delete kga-v2-cache-conn --region=europe-west6`.

## Related

- [[test-agent-v2 Redis deploy blocked by vpcaccess.connectors.create IAM denial]]

---

## CORRECTION — the real root cause (CIDR theory was WRONG)

The CIDR/network theory above was disproven. `10.8.0.0/28` does NOT overlap any subnet in the `default` network (its subnets are all 10.12x–10.19x + a 10.123.0.0/24), and the network `default` was correct (the Memorystore `kga-v2-cache` lives on `default` @ 10.161.251.187). The connector's actual create failure, seen only after deleting it and letting terraform recreate:

> Error waiting to create Connector: Error code 3, message: **Operation failed: A Connector must specify either max_throughput or max_instances.**

**Root cause: `redis.tf`'s `google_vpc_access_connector.redis` was missing required instance sizing.** The google provider (8.0.0) requires either `max_throughput` (throughput mode) or `min_instances`+`max_instances` (instance mode). With neither, every create fails → the connector object lands in ERROR → the deploy.sh retry then hits `409 already exists`.

**Fix:** add to the resource — `min_instances = 2` and `max_instances = 3` (the minimums for the default e2-micro machine type; max_instances must be > min_instances). Then DELETE the ERROR connector (`gcloud compute networks vpc-access connectors delete kga-v2-cache-conn --region=europe-west6 --quiet`) so terraform recreates it cleanly.

**Debugging lesson:** don't trust the surface `409 already exists` from the retry — DELETE the ERROR resource and let the FIRST create run to see the true `Error code 3` message. And an ERROR-state connector must be deleted+recreated, never imported.
