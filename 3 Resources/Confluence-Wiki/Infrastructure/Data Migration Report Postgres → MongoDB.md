---
title: "Data Migration Report: Postgres → MongoDB"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48911712278/Data+Migration+Report+Postgres+MongoDB
space: "HACKA"
topic: infra
relevance: 0.757
depth: 2.84
updated: 2025-12-02
attachments: 18
tags:
  - confluence
  - infra
  - space/hacka
---

# Data Migration Report: Postgres → MongoDB

> [!info] Imported from Confluence
> Space **HACKA** · updated 2025-12-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48911712278/Data+Migration+Report+Postgres+MongoDB)
> Relevance 0.757 · topic `infra`

# 1. Dev-vn

Schema: 2225

Fist schema start time: 2025-11-26 07:36:00.000 UTC

Last schema end time: 2025-11-26 11:12:21.958 UTC

Total time: ~3h36

Total migrated: 546.495 documents

Local data size: 1.21 Gb (uncompressed)

Storage size : 255.98 MB (compressed)


![[48911712278-image-20251128-035530.png]]

![[48911712278-image-20251202-075431.png]]



## 1.1 MongoDB

**Cluster Tier**: M10 (General)


![[48911712278-image-20251126-072105.png]]

![[48911712278-image-20251127-022530.png]]



### **Insert per second:**

- High load (~200 schemas) : ~1238 documents/ second


![[48911712278-image-20251126-074211.png]]



- After high load ( ~2 schema): ~113 documents/ second


![[48911712278-image-20251126-073721.png]]



### Connection

- Config: connectionPoll = 100

- From 95 → 197 = ~100 connection created


![[48911712278-image-20251126-074517.png]]



## 1.2 luz-mongodb-relational-migrator

**Memory:**

- Limit : 6Gb → JVM can use = 6 \* 0.8 = 4.8 Gb

- Used: 4.12 → ~0.68 GB free


![[48911712278-image-20251126-075742.png]]



**CPU:**

- Limit: 6

- Use: 3.41


![[48911712278-image-20251126-075950.png]]



## 1.3 Postgres

### luz-database

- Limit: 14 Gb

- From 7.38 Gb -\> max 13.55 Gb = ~6.1 Gb → 0.45Gb free


![[48911712278-image-20251126-075201.png]]
