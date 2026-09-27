---
ai_hash: afa8e127207e1902
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49594335233'
confluence_path: Team Kepler > Developer note > Count Fan-out (K) Benchmark on Performance
  Env
created: 2026-07-17
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- performance
title: Test parallelize executor
type: source
updated: 2026-07-17
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49594335233/Test+parallelize+executor
---

# Test parallelize executor

*Confluence source · Team Kepler › Developer note › Count Fan-out (K) Benchmark on Performance Env · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49594335233/Test+parallelize+executor) · updated 2026-07-17*

## Before execute:

Tenant: d0783310-d67f-4ab7-9aab-dcaef3f17f48

Environment: DEV + local debug

Count total document: 100.000

![[image-20260717-062047.png]]

Count have \_shard: 0

![[image-20260717-062832.png]]

## Run Executor:

Run 100k execution take 66108 ms

> [!note]- Log details
>
>
>
> ```
> 08:43:29,268 INFO  [javax.ws.rs.client.ClientResponseFilter] (default task-1) [PATCH] - http://host.docker.internal:8080/luz_jsonstore/api/mdb/d0783310-d67f-4ab7-9aab-dcaef3f17f48/documents headers=[Connection=close,Content-Length=46,Content-Type=application/json,Date=Fri, 17 Jul 2026 06:43:29 GMT,Server=nginx/1.31.2,x-envoy-upstream-service-time=65264] status-code=200 time-consuming=66017
> 2026-07-17T06:43:29.271480768Z 08:43:29,270 INFO  [ch.klara.luz.docs.interceptor.LogExecutionTimeInterceptor] (default task-1) [ParallelizeMigrationExecutor.execute] took 66034 ms
> 2026-07-17T06:43:29.299518288Z 08:43:29,298 INFO  [io.undertow.accesslog] (default task-1) 192.168.143.2 [17/Jul/2026:08:43:29 +0200] luz-uri=POST /luz_docs/api/migration/d0783310-d67f-4ab7-9aab-dcaef3f17f48/parallelize HTTP/1.1 status-code=200 bytes-sent=78 time-consuming=66108
> ```
>
>
>

![[image-20260717-065618.png]]

Count total document: 100.000

![[image-20260717-062047.png]]

Count have \_shard: 100.000

![[image-20260717-064714.png]]

## After execute:

> [!note]- Scripts
>
>
>
> ```
> db.documents.aggregate([
>     { $group: {
>             _id: { $switch: {
>                     branches: [
>                         { case: { $in: [ { $type: "$_shard" }, ["missing","null"] ] }, then: "null" },
>                         { case: { $lt: ["$_shard", 178956970] }, then: 0 },
>                         { case: { $lt: ["$_shard", 357913941] }, then: 1 },
>                         { case: { $lt: ["$_shard", 536870912] }, then: 2 },
>                         { case: { $lt: ["$_shard", 715827882] }, then: 3 },
>                         { case: { $lt: ["$_shard", 894784853] }, then: 4 }
>                     ],
>                     default: 5
>                 }},
>             count: { $sum: 1 }
>         }},
>     { $sort: { _id: 1 } }
> ])
>
> db.documents.aggregate([
>     { $group: {
>             _id: { $switch: {
>                     branches: [
>                         { case: { $in: [ { $type: "$_shard" }, ["missing","null"] ] }, then: "null" },
>                         { case: { $lt: ["$_shard", 89478485] },  then: 0 },
>                         { case: { $lt: ["$_shard", 178956970] }, then: 1 },
>                         { case: { $lt: ["$_shard", 268435456] }, then: 2 },
>                         { case: { $lt: ["$_shard", 357913941] }, then: 3 },
>                         { case: { $lt: ["$_shard", 447392426] }, then: 4 },
>                         { case: { $lt: ["$_shard", 536870912] }, then: 5 },
>                         { case: { $lt: ["$_shard", 626349397] }, then: 6 },
>                         { case: { $lt: ["$_shard", 715827882] }, then: 7 },
>                         { case: { $lt: ["$_shard", 805306368] }, then: 8 },
>                         { case: { $lt: ["$_shard", 894784853] }, then: 9 },
>                         { case: { $lt: ["$_shard", 984213338] }, then: 10 }
>                     ],
>                     default: 11
>                 }},
>             count: { $sum: 1 }
>         }},
>     { $sort: { _id: 1 } }
> ])
> ```
>
>
>

Case K=6

![[image-20260717-065657.png]]

Case K=12

![[image-20260717-065747.png]]

%% ai-graph-start %%

**Related notes:**
- [[Count Fan-out (K) Benchmark on Performance Env]]
- [[Divide-and-Conquer Visible-Document Count]]
- [[luz-docs - MongoDB aggregate slow query analyze]]
- [[Dev benchmark _shard count fan-out ~1.8x, diminishing past K=12; local port-forward hid the gain]]
- [[eArchive Performance measurement & scalability assessment at 800000 documents]]

%% ai-graph-end %%