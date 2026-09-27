---
tags: [gcp, redis, memorystore, cloud-sql, networking, iap, test-agent-v2]
created: 2026-09-16
---

# Reaching Memorystore Redis from local ≠ reaching Cloud SQL from local

**Cloud SQL** has the **Cloud SQL Auth Proxy** — a socketless, IAM-authenticated binary that dials the
instance over TLS from anywhere (no VPC, no public IP needed). That's what `tools/cloudsql_proxy.sh`
uses: download the proxy, run `cloud-sql-proxy <instance>` → `localhost:5432` reaches the DB.

**Memorystore Redis has NO equivalent proxy.** A Basic-tier instance exposes only a **private VPC IP**.
There is no public endpoint and no auth-proxy binary. So you cannot reach it from a laptop directly —
you must hop through a machine that lives *inside the VPC*.

## The working pattern (what `tools/redis_proxy.sh` does)
IAP-tunnel an SSH port-forward through a tiny throwaway jump VM in the same VPC:

```
laptop  --IAP SSH-->  e2-micro (VPC, --no-address)  --TCP-->  <redis host>:6379
localhost:6379  ==================================================> Memorystore
gcloud compute ssh <vm> --tunnel-through-iap -- -N -L 6379:<REDIS_HOST>:6379
```

Load-bearing bits:
- **IAP firewall rule**: allow `tcp:22` from **`35.235.240.0/20`** (the IAP range) on the VPC — without
  it the tunnel can't open. One-time, idempotent.
- **Jump VM**: `e2-micro`, `--no-address` (no public IP; IAP reaches it anyway), same `--network` as the
  Memorystore `authorized_network`. Create-if-missing + a `--teardown` to delete it (it's billable).
- **Redis host/port**: `gcloud redis instances describe <name> --region <r> --format='value(host)'`
  (private IP, changes on recreate — resolve it, don't hardcode).
- **AUTH**: off unless `auth_enabled=true` on the instance (ours is off) → no password; access control IS
  the private-network boundary. If enabled: `gcloud redis instances get-auth-string <name>`.

## Why it matters
Cloud Run reaches Memorystore via a **Serverless VPC Access connector** (egress-only) — that path is NOT
usable from a laptop. Local access is a *separate* problem solved only by the in-VPC hop above.

Related: [[test-agent-v2 Redis cache port + Memorystore needs a VPC connector]]
