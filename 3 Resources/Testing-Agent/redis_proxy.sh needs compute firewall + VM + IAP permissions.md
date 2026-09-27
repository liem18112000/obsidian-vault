---
ai_hash: 3c202ff408571eb0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-16
entities: []
source: Testing-Agent deploy 3a39108-report
status: seedling
tags:
- testing-agent
- redis
- iam
- gcp
- gotcha
title: redis_proxy.sh needs compute firewall + VM + IAP permissions
type: lesson
---

# redis_proxy.sh needs compute firewall + VM + IAP permissions

`test-agent-v2/tools/redis_proxy.sh` (local access to the Memorystore benchmark cache) opens an IAP-tunneled jump VM, which needs several compute permissions the deploy account may lack:

- `compute.firewalls.create` — to add the `allow-iap-ssh-v2` firewall rule (IAP range -> tcp:22). Denial seen: "Required 'compute.firewalls.create' permission for projects/klara-nonprod/global/firewalls/allow-iap-ssh-v2". Role: `roles/compute.securityAdmin`.
- `compute.instances.create` (+ disks/SA actAs) — to create the throwaway jump VM. Role: `roles/compute.instanceAdmin.v1` (+ `roles/iam.serviceAccountUser`).
- `roles/iap.tunnelResourceAccessor` — to SSH via IAP.

Simplest single grant that covers firewall + VM: `roles/compute.admin` (broad); still add `roles/iap.tunnelResourceAccessor` for the tunnel. Grant with `--condition=None` (the klara-nonprod policy has conditional bindings).

IMPORTANT: the proxy is OPTIONAL debugging — the benchmark cache is EMPTY until the deployed TEV app writes `memory/benchmarks/*`, and the main deploy does NOT need the proxy. The proxy did successfully RESOLVE the endpoint (Memorystore kga-v2-cache @ 10.161.251.187:6379, auth disabled) before the firewall step failed, so connectivity metadata is fine.

## Related

- [[test-agent-v2 Redis VPC connector stuck in ERROR (CIDRnetwork misconfig)|test-agent-v2 Redis VPC connector stuck in ERROR (CIDR/network misconfig)]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 Redis deploy blocked by vpcaccess.connectors.create IAM denial]]
- [[Memorystore Redis has no auth-proxy — local access needs an IAP jump VM]]
- [[test-agent-v2 Redis cache port + Memorystore needs a VPC connector]]
- [[test-agent-v2 Redis VPC connector stuck in ERROR (CIDRnetwork misconfig)]]
- [[test-agent-v2 cloud resource and credential map (klara-nonprod)]]

%% ai-graph-end %%