---
title: "Part A: luz-jsonstore Analysis"
created: 2025-12-18
updated: 2025-12-18
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48989044741/Part+A+luz-jsonstore+Analysis
confluence_id: "48989044741"
confluence_path: "Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE on Production"
tags: [confluence, luz-jsonstore]
---

# Part A: luz-jsonstore Analysis

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48989044741/Part+A+luz-jsonstore+Analysis) · updated 2025-12-18*

------------------------------------------------------------------------

### A.1 Case Analytics Summary

#### A.1.1 Overall Statistics

|              |                                |
|--------------|--------------------------------|
| Metric       | Value                          |
| Total Cases  | 25                             |
| Unique Dates | 11                             |
| Root Cause   | 100% luz-jsonstore unavailable |
| Peak Hour    | 11:00 UTC (8 failures)         |

#### A.1.2 Failure Distribution by Date

|            |       |                           |
|------------|-------|---------------------------|
| Date       | Count | Pattern                   |
| 2025-11-10 | 7     | **Major Cluster** (7 min) |
| 2025-11-25 | 5     | Spread across day         |
| 2025-11-18 | 3     | Spread across day         |
| 2025-12-05 | 2     | Isolated                  |
| Others     | 8     | Isolated events           |

#### A.1.3 Failed Operations

|                                    |       |     |
|------------------------------------|-------|-----|
| Operation                          | Count | %   |
| updateOrRemoveMetadataFilterFields | 23    | 96% |
| createDocumentMetadata             | 1     | 4%  |

#### A.1.4 Detailed Case List with Root Cause Evidence

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | DocumentId | Date | Time (UTC) | Pod Restart Evidence | Root Cause |
| 1 | 217599 | 2025-11-10 | 11:04:31 | Pod restart at 11:11:12 | Connection refused |
| 2 | 749107 | 2025-11-10 | 11:04:37 | Pod restart at 11:11:12 | Connection refused |
| 3 | 588 | 2025-11-10 | 11:10:52 | Pod restart at 11:11:12 | Connection refused |
| 4 | 3507 | 2025-11-10 | 11:10:56 | Pod restart at 11:11:12 | Connection refused |
| 5 | 589 | 2025-11-10 | 11:11:11 | Pod restart at 11:11:12 | Connection refused |
| 6 | 590 | 2025-11-10 | 11:11:21 | Pod restart at 11:11:12 | Connection refused |
| 7 | 643 | 2025-11-11 | 17:16:41 | Deployment in progress | Connection refused |
| 8 | 20211 | 2025-11-13 | 11:16:27 | Deployment in progress | Connection refused |
| 9 | 218735 | 2025-11-18 | 00:02:40 | Deployment in progress | Connection refused |
| 10 | 20512 | 2025-11-18 | 08:16:22 | Deployment in progress | Connection refused |
| 11 | 12278 | 2025-11-18 | 11:15:56 | Deployment in progress | Connection refused |
| 12 | 18219 | 2025-11-19 | 17:16:20 | Deployment in progress | Connection refused |
| 13 | 386 | 2025-11-22 | 08:16:28 | Deployment in progress | Connection refused |
| 14 | 222042 | 2025-11-25 | 05:00:28 | Deployment in progress | Connection refused |
| 15 | 222561 | 2025-11-25 | 07:10:26 | Deployment in progress | Connection refused |
| 16 | 223411 | 2025-11-25 | 09:00:15 | Deployment in progress | Connection refused |
| 17 | 223412 | 2025-11-25 | 09:00:19 | Deployment in progress | Connection refused |
| 18 | 1741 | 2025-11-25 | 14:16:21 | Deployment in progress | Connection refused |
| 19 | 778 | 2025-12-01 | 07:24:03 | Deployment in progress | Connection refused |
| 20 | 458224 | 2025-12-02 | 05:04:19 | Deployment in progress | Connection refused |
| 21 | 458627 | 2025-12-02 | 05:37:12 | Deployment in progress | Connection refused |
| 22 | 21579 | 2025-12-03 | 14:16:35 | 2 pods at 14:16:02 | Connection refused |
| 23 | 8250 | 2025-12-05 | 08:16:41 | 6 pods at 08:16:00 | Connection refused |
| 24 | 27748 | 2025-12-05 | 14:15:50 | 7 pods at 14:15:54 | Connection refused |

#### A.1.5 Verified Pod Restart Correlation (Key Cases)

##### Case 27748 (2025-12-05 14:15:50)

|  |  |  |
|----|----|----|
| Event | Time (UTC) | Details |
| **FAILED_TO_STORE** | 14:15:50.839 | DocumentId 27748 |
| **Connection Refused** | 14:15:50.468 | luz-docs-batch → luz-jsonstore |
| Pod Starting | 14:15:54.871 | `luz-jsonstore-7d4f9476c7-7nnt8` |
| Pod Starting | 14:15:55.224 | `luz-jsonstore-7d4f9476c7-pdq4d` |
| Pod Starting | 14:15:56.030 | `luz-jsonstore-7d4f9476c7-g8zt7` |
| Pod Starting | 14:15:56.674 | `luz-jsonstore-7d4f9476c7-4pnf2` |
| Pod Starting | 14:16:23.075 | `luz-jsonstore-7d4f9476c7-r9fft` |
| Pod Starting | 14:16:23.479 | `luz-jsonstore-7d4f9476c7-hrb9m` |
| Pod Starting | 14:16:23.952 | `luz-jsonstore-7d4f9476c7-268kz` |
| OLD Pod Last | 14:16:52.666 | `luz-jsonstore-6fb8b5b785-9tmh7` terminated |

**Gap Analysis:** Failure at 14:15:50, first NEW pod started at 14:15:54 = **4 seconds with 0 pods available**

##### Case 8250 (2025-12-05 08:16:41)

|                        |              |                                  |
|------------------------|--------------|----------------------------------|
| Event                  | Time (UTC)   | Details                          |
| **FAILED_TO_STORE**    | 08:16:41.871 | DocumentId 8250                  |
| **Connection Refused** | 08:16:30.463 | luz-docs-batch → luz-jsonstore   |
| Pod Starting           | 08:16:00.245 | `luz-jsonstore-6d6c6b9c64-nx6jq` |
| Pod Starting           | 08:16:00.838 | `luz-jsonstore-6d6c6b9c64-xf9n9` |
| Pod Starting           | 08:16:03.541 | `luz-jsonstore-6d6c6b9c64-rc5fp` |
| Pod Starting           | 08:16:12.322 | `luz-jsonstore-6d6c6b9c64-9x8rs` |
| Pod Starting           | 08:16:17.802 | `luz-jsonstore-6d6c6b9c64-r798z` |
| Pod Starting           | 08:16:33.886 | `luz-jsonstore-6bb4cb68f4-mbx2w` |

**Gap Analysis:** 6 pods starting, failure at 08:16:30 = **pods still initializing (not ready)**

##### November 10 Major Cluster (7 Failures in 7 minutes)

|                 |              |                                  |
|-----------------|--------------|----------------------------------|
| Event           | Time (UTC)   | Details                          |
| FAILED_TO_STORE | 11:04:31.042 | DocumentId 217599                |
| FAILED_TO_STORE | 11:04:37.232 | DocumentId 749107                |
| FAILED_TO_STORE | 11:10:52.743 | DocumentId 588                   |
| FAILED_TO_STORE | 11:10:56.477 | DocumentId 3507                  |
| FAILED_TO_STORE | 11:11:11.550 | DocumentId 589                   |
| FAILED_TO_STORE | 11:11:21.765 | DocumentId 590                   |
| Pod Starting    | 11:11:12.858 | `luz-jsonstore-77cfd496bc-q7m4r` |

**Analysis:** Extended outage - 7 documents failed over 7 minutes during deployment

#### A.1.6 Cluster Analysis

|             |            |                   |       |          |                    |
|-------------|------------|-------------------|-------|----------|--------------------|
| Cluster     | Date       | Time Range        | Count | Duration | Notes              |
| **Nov10-A** | 2025-11-10 | 11:04:31-11:04:37 | 2     | 6 sec    | First wave         |
| **Nov10-B** | 2025-11-10 | 11:10:52-11:11:21 | 4     | 29 sec   | Second wave        |
| Nov18       | 2025-11-18 | 00:02-11:15       | 3     | 11 hrs   | Spread             |
| Nov25       | 2025-11-25 | 05:00-14:16       | 5     | 9 hrs    | Spread             |
| Dec02-A     | 2025-12-02 | 05:04-05:37       | 2     | 33 min   | Morning            |
| Dec05-A     | 2025-12-05 | 08:16:41          | 1     | \-       | Morning            |
| **Dec05-B** | 2025-12-05 | 14:15:50          | 1     | \-       | Analyzed in detail |
| Isolated    | Various    | Various           | 5     | \-       | Single events      |

**All 24 cases share identical root cause:** `java.net.ConnectException: Connection refused` to `luz-jsonstore:8080`

#### A.1.7 Pod Restart Frequency (30 Days)

|               |              |             |
|---------------|--------------|-------------|
| Time Period   | Pod Restarts | Average     |
| Last 24 hours | 50+          | ~2/hour     |
| Last 7 days   | 200+         | ~29/day     |
| Last 30 days  | 500+         | **~17/day** |

------------------------------------------------------------------------

### A.2 Root Cause Analysis

#### A.2.1 Service Flow

![[3 Resources/Confluence/Team Kepler/Developer note/attachments/part-a-luz-jsonstore-analysis/image-20251210-100153.png]]

#### A.2.2 Root Causes Identified

|     |                              |          |                              |
|-----|------------------------------|----------|------------------------------|
| \#  | Root Cause                   | Severity | Impact                       |
| 1   | File-based readiness probe   | CRITICAL | Pod ready before app ready   |
| 2   | No startup probe             | CRITICAL | No protection during startup |
| 3   | No graceful shutdown         | CRITICAL | Abrupt connection drop       |
| 4   | Cold MongoDB cache           | HIGH     | Slow first requests          |
| 5   | 60s MongoDB timeout          | HIGH     | Slow failover                |
| 6   | JWT token expiry (Vault 400) | MEDIUM   | Auth failures                |

#### A.2.3 Complete Root Cause Chain

![[3 Resources/Confluence/Team Kepler/Developer note/attachments/part-a-luz-jsonstore-analysis/image-20251210-100340.png]]

#### A.2.4 Error Propagation Chain

|  |  |  |  |  |
|----|----|----|----|----|
| Step | Service | Receives | Throws | Returns |
| 1 | luz-jsonstore | Request | \- | Connection refused |
| 2 | luz-docs-batch | ConnectException | DocumentException | HTTP 503 |
| 3 | luz-docs-view-controller-batch | HTTP 503 | ServiceUnavailableException | HTTP 500 |
| 4 | luz-eletter | HTTP 500 | InternalServerError | FAILED_TO_STORE |

#### A.2.5 Failure Mechanism

![[3 Resources/Confluence/Team Kepler/Developer note/attachments/part-a-luz-jsonstore-analysis/image-20251210-100604.png]]

 

------------------------------------------------------------------------

### A.3 Evidence

#### A.3.1 File-Based Readiness Probe

**Current Configuration (k8s.yaml:38-42):**

```
readinessProbe:
  exec:
    command:
    - cat
    - /opt/jboss/wildfly/standalone/deployments/luz_jsonstore.war.deployed
```

**Problem:** File exists ~10s after pod start, but MongoDB not connected until ~40s later.

#### A.3.2 No Graceful Shutdown

**Log Evidence - Abrupt Termination:**

```
# No shutdown logs found - pods terminate without cleanup
# Last activity before termination (Case 27748):
14:16:52.666Z luz-jsonstore-6fb8b5b785-9tmh7 [last log entry - then silence]
# No "shutting down" or "closing connections" messages
```

#### A.3.3 Cold MongoDB Cache

**Log Evidence - Cache Miss Pattern:**

```
10:57:53,682 INFO [JsonStoreMongoDbService] getClient connected to luz-mongodb0@-cluster-rs...
10:57:53,691 INFO [JsonStoreMongoDbService] getClient connected to null    # FAILURE
10:57:54,172 INFO [JsonStoreMongoDbService] getClient connected to null    # FAILURE
10:57:54,526 INFO [JsonStoreMongoDbService] getClient connected to null    # FAILURE
```

 

#### A.3.4 Connection Refused Error

**Exception Stack Trace (Case 27748):**

```
javax.ws.rs.ProcessingException: RESTEASY004655: Unable to invoke request:
org.apache.http.conn.HttpHostConnectException: Connect to luz-jsonstore:8080
[luz-jsonstore/10.8.6.234] failed: Connection refused
Caused by: java.net.ConnectException: Connection refused
    at java.base/sun.nio.ch.Net.pollConnect(Native Method)
    at java.base/sun.nio.ch.Net.pollConnectNow(Net.java:672)
    at java.base/sun.nio.ch.NioSocketImpl.timedFinishConnect(NioSocketImpl.java:542)
```

 

#### A.3.5 60s MongoDB Timeout

**Configuration (Constants.java:37):**

```
public static final String MONGODB_CONN_PARAM =
    "&connectTimeoutMS=60000&maxPoolSize=10";  // 60 SECONDS!
```

#### A.3.6 Vault JWT Expiry (HTTP 400)

**Statistics:**

- HTTP 400 errors: 200+ per day

- All from `loginJWT` endpoint

- Cause: JWT tokens expired before Vault validates them

#### A.3.7 WildFly Startup Times

|        |       |        |
|--------|-------|--------|
| Range  | Count | Notes  |
| 8-10s  | 40%   | Fast   |
| 10-12s | 30%   | Normal |
| 12-17s | 30%   | Slow   |

**Note:** MongoDB discovery adds 10-30s additional time.
