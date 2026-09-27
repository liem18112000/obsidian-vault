---
ai_hash: 073eb027d680ab26
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities:
- 10000-IOPS standalone PostgreSQL
- vServer-enabled zone
- Gen2-NVMe2-IOPS10000
- HCM03-1A
- GreenNode/VNG Cloud vDB
- instance's subnet
- vServer subnets
- HCM03-1C
- standalone volumes
- 3200 IOPS
- ssd-iops{200..3200}-HCM03-1C
- cluster topology
- SSD-IOPS10000 (cluster)
- db.s2-general-8x16
- vServer
- single-node 10000 IOPS
- GreenNode support ticket
- pro-8986f5c6
- vDB cluster in 1C
- Backup-Center backups
- standalone@3200 in 1C
- Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary
- 'GreenNode vDB create constraints: instance name 6-20 chars, password start-with-letter,
  package family s2-general'
source: session 2026-08-17
status: seedling
tags:
- greennode
- vngcloud
- vdb
- iops
- zone
- decision
title: 10000-IOPS standalone vDB PostgreSQL needs a vServer-enabled zone that offers
  Gen2-NVMe2-IOPS10000 (HCM03-1A)
type: observation
---

# 10000-IOPS standalone vDB PostgreSQL needs a vServer-enabled zone that offers Gen2-NVMe2-IOPS10000 (HCM03-1A)

Decision (leo-customer360 postgres deploy): to run a **10000-IOPS single-node** PostgreSQL on GreenNode/VNG Cloud vDB, the instance's subnet must live in a **vServer-ENABLED zone that ALSO offers the Gen2-NVMe2-IOPS10000 standalone volume**.

The bind for this account:
- vServer subnets can only be created in **HCM03-1C** (1A/1B disabled).
- HCM03-1C **standalone** volumes cap at **3200 IOPS** (`ssd-iops{200..3200}-HCM03-1C`). 10000 IOPS in 1C exists only for the **cluster** topology (`SSD-IOPS10000`).
- HCM03-1A **standalone** DOES offer `Gen2-NVMe2-IOPS10000` (+ package `db.s2-general-8x16`), but vServer is disabled there, so the subnet can't be created.

So single-node 10000 IOPS is impossible until **HCM03-1A is enabled for vServer** (GreenNode support ticket for project pro-8986f5c6...). Chosen path: keep the standalone config pinned to HCM03-1A (db.s2-general-8x16 + Gen2-NVMe2-IOPS10000) and treat apply as BLOCKED until support enables the AZ. Alternatives considered & rejected: vDB cluster in 1C (HA, higher cost, Backup-Center backups) and standalone@3200 in 1C. See [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]] and [[GreenNode vDB create constraints instance name 6-20 chars, password start-with-letter, package family s2-general|GreenNode vDB create constraints: instance name 6-20 chars, password start-with-letter, package family s2-general]].

## Related

- [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]]

%% ai-graph-start %%

**Related notes:**
- [[GreenNode vDB package family + IOPS tier are per-zone (HCM03-1A vs 1C)]]
- [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]]
- [[vDB volume_type cannot be changed on a live instance (no change-type API; not ForceNew so TF won't recreate)]]
- [[VNG vServer default catalog endpoints return the DISABLED default AZ; use zoneId=AZ and pin UUIDs]]
- [[Running post-deploy SQL against a managed vDB (psql gexec, dockerized client, private-IP caveat)]]

**Relations:**
- 10000-IOPS standalone PostgreSQL — *needs* — vServer-enabled zone
- vServer-enabled zone — *offers* — Gen2-NVMe2-IOPS10000
- 10000-IOPS standalone PostgreSQL — *runs on* — GreenNode/VNG Cloud vDB
- instance's subnet — *must live in* — vServer-enabled zone
- vServer subnets — *can only be created in* — HCM03-1C
- HCM03-1C — *standalone volumes cap at* — 3200 IOPS
- HCM03-1C — *offers* — ssd-iops{200..3200}-HCM03-1C
- SSD-IOPS10000 (cluster) — *exists for* — cluster topology
- HCM03-1A — *does offer* — Gen2-NVMe2-IOPS10000
- HCM03-1A — *offers package* — db.s2-general-8x16
- vServer — *is disabled in* — HCM03-1A
- single-node 10000 IOPS — *is impossible until* — HCM03-1A
- HCM03-1A — *is enabled for* — vServer
- GreenNode support ticket — *for project* — pro-8986f5c6
- standalone config — *pinned to* — HCM03-1A
- apply — *treated as* — BLOCKED
- vDB cluster in 1C — *considered & rejected as alternative* — 10000-IOPS standalone PostgreSQL
- vDB cluster in 1C — *has characteristic* — HA
- vDB cluster in 1C — *has characteristic* — higher cost
- vDB cluster in 1C — *uses* — Backup-Center backups
- standalone@3200 in 1C — *considered & rejected as alternative* — 10000-IOPS standalone PostgreSQL
- Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary — *related to* — GreenNode/VNG Cloud vDB
- GreenNode vDB create constraints: instance name 6-20 chars, password start-with-letter, package family s2-general — *related to* — GreenNode/VNG Cloud vDB

%% ai-graph-end %%