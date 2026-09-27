---
ai_hash: eb207f55574905e1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48954966029'
confluence_path: Team Kepler > Developer note
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: 'Service Error Analysis Report: FAILED_TO_STORE on Production'
type: source
updated: 2025-12-18
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954966029/Service+Error+Analysis+Report+FAILED_TO_STORE+on+Production
---

# Service Error Analysis Report: FAILED_TO_STORE on Production

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48954966029/Service+Error+Analysis+Report+FAILED_TO_STORE+on+Production) · updated 2025-12-18*

------------------------------------------------------------------------

**Analysis Period:** 30-31 days (Nov 10 - Dec 10, 2025)

**Environment:** klara-prod

------------------------------------------------------------------------

## Executive Summary

This report analyzes service errors in the klara-prod environment over a 30-day period, covering two key services:

|  |  |  |  |  |
|----|----|----|----|----|
| Service | Issue Type | Cases | Severity | Root Cause |
| **luz-jsonstore** | FAILED_TO_STORE | 25 | CRITICAL | Pod deployment gaps |
| **luz-antivirus** | HTTP 503/504 | 1,000+ / 6 | LOW/MEDIUM | ClamAV updates & timeouts |

**Key Findings:**

- **luz-jsonstore:** 25 document storage failures caused by rolling deployment gaps (100% correlation with pod restarts)

- **luz-antivirus:** Brief service unavailability during virus definition updates (expected behavior); 6 scan timeouts from oversized files

------------------------------------------------------------------------

## Overall Error Statistics

|               |                            |        |          |                   |
|---------------|----------------------------|--------|----------|-------------------|
| Service       | Error Type                 | Count  | Severity | Status            |
| luz-jsonstore | FAILED_TO_STORE            | 25     | CRITICAL | Requires fix      |
| luz-jsonstore | Connection refused         | 25     | CRITICAL | Same root cause   |
| luz-antivirus | HTTP 503 (Health Check)    | 1,000+ | LOW      | Expected behavior |
| luz-antivirus | HTTP 504 (Scanner Timeout) | 6      | MEDIUM   | Isolated incident |
| luz-antivirus | Malware Detected           | 0      | \-       | System working    |

------------------------------------------------------------------------

### Part A: luz-jsonstore Analysis (FAILED_TO_STORE)

Please view the content of [Part A: luz-jsonstore Analysis](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48989044741/Part+A+luz-jsonstore+Analysis)

### Part B: luz-antivirus Analysis

Please view the content of [Part B: luz-antivirus Analysis](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48989208583/Part+B+luz-antivirus+Analysis)

------------------------------------------------------------------------

## Recommendations

### Priority Matrix

|  |  |  |  |  |
|----|----|----|----|----|
| Priority | Service | Fix | Effort | Impact |
| **P0** | luz-jsonstore | HTTP readiness probe | Low | 80% reduction |
| **P0** | luz-jsonstore | Add startup probe | Low | 90% reduction |
| **P0** | luz-jsonstore | Graceful shutdown | Low | 95% reduction |
| **P1** | luz-jsonstore | Pre-warm MongoDB cache | Medium | 98% reduction |
| **P1** | luz-jsonstore | Parallel MongoDB + 10s timeout | Medium | 99% reduction |
| **P2** | luz-jsonstore | Vault retry for HTTP 400 | Low | Reduce auth errors |
| **P2** | luz-antivirus | Increase failureThreshold to 3 | Low | Reduce 503 count |
| **P3** | luz-antivirus | Add retry logic for socket | Low | Reduce scan failures |

------------------------------------------------------------------------

### luz-jsonstore Fixes

#### Kubernetes Configuration

```
# k8s.yaml changes
spec:
  terminationGracePeriodSeconds: 60  # NEW
  containers:
  - name: luz-jsonstore
    lifecycle:                        # NEW
      preStop:
        exec:
          command: ["/bin/sh", "-c", "sleep 15"]

    startupProbe:                     # NEW
      httpGet:
        path: /luz_jsonstore/api/version
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 5
      failureThreshold: 60

    readinessProbe:                   # CHANGED
      httpGet:                        # Was: exec cat file
        path: /luz_jsonstore/api/health/ready
        port: 8080
      initialDelaySeconds: 30
      periodSeconds: 5
      failureThreshold: 6

    livenessProbe:
      httpGet:
        path: /luz_jsonstore/api/version
        port: 8080
      initialDelaySeconds: 60         # Increased from 30
      periodSeconds: 30               # Decreased from 120
```

#### Application Code Fixes

|  |  |  |
|----|----|----|
| File | Line | Fix |
| `Constants.java` | 37 | `connectTimeoutMS=10000` (was 60000) |
| `VaultService.java` | 98 | Add HTTP 400 to retry conditions |
| `JsonStoreMongoDbBase.java` | 84 | Parallel MongoDB connection attempts |
| NEW: `HealthResource.java` | \- | Create readiness endpoint with MongoDB check |
| NEW: `CacheWarmer.java` | \- | Pre-warm MongoDB cache on startup |

------------------------------------------------------------------------

### luz-antivirus Fixes

#### Kubernetes Probe Adjustment

**Current:**

```
readinessProbe:
  failureThreshold: 1  # Too aggressive
```

**Suggested:**

```
readinessProbe:
  failureThreshold: 3  # Allow brief ClamAV unavailability
  periodSeconds: 2
  # Total: 6 seconds before marking unready
```

#### Potential Fix: Retry Logic

**Location:** `ClamAVClient.java:68-72`

```
private Socket getSocket(int timeout) throws IOException {
    int maxRetries = 3;
    int delay = 1000; // 1 second
    for (int i = 0; i < maxRetries; i++) {
        try {
            var socket = new Socket();
            socket.setSoTimeout(timeout);
            socket.connect(new InetSocketAddress(
                this.config.host().orElseThrow(), this.config.port()), timeout);
            return socket;
        } catch (IOException e) {
            if (i == maxRetries - 1) throw e;
            Thread.sleep(delay * (i + 1));
        }
    }
    throw new IOException("Failed to connect after retries");
}
```

------------------------------------------------------------------------

## Summary

### Root Cause Chain (luz-jsonstore)

```
Rolling Deployment
```

### Expected Outcomes After Fix

|               |                     |            |                      |
|---------------|---------------------|------------|----------------------|
| Service       | Metric              | Before Fix | After Fix (Expected) |
| luz-jsonstore | Failures/month      | 25         | \< 3                 |
| luz-jsonstore | Deployment downtime | 30-60s     | 0s (zero-downtime)   |
| luz-jsonstore | Failover time       | 60s        | \< 15s               |
| luz-antivirus | 503 per update      | 7-11       | 2-3                  |
| luz-antivirus | Scan failures       | 6          | 0 (with retry)       |

### Current State Assessment

|  |  |  |
|----|----|----|
| Service | Status | Notes |
| luz-jsonstore | **REQUIRES FIX** | Critical deployment issues |
| luz-antivirus | **HEALTHY** | Expected behavior, minor improvements possible |

------------------------------------------------------------------------

## Appendix

### Log Queries Used

#### luz-jsonstore

```
# FAILED_TO_STORE errors
gcloud logging read 'labels."k8s-pod/app"="luz-eletter" AND "FAILED_TO_STORE"' \
  --project=klara-prod --freshness=31d

# Pod restarts
gcloud logging read 'labels."k8s-pod/app"="luz-jsonstore" AND "JBoss Modules"' \
  --project=klara-prod --freshness=31d
```

#### luz-antivirus

```
# Health check 503 errors
gcloud logging read 'labels."k8s-pod/app"="luz-antivirus" AND "status-code=503"' \
  --project=klara-prod --freshness=31d

# Scanner API errors
gcloud logging read 'labels."k8s-pod/app"="luz-antivirus" AND "scanner" AND "status-code" AND NOT "status-code=200"' \
  --project=klara-prod --freshness=31d

# Malware detections
gcloud logging read 'labels."k8s-pod/app"="luz-antivirus" AND ("FOUND" OR "infected" OR "Trojan")' \
  --project=klara-prod --freshness=31d
```

%% ai-graph-start %%

**Related notes:**
- [[Service Reliability Solution]]
- [[Part B - luz-antivirus Analysis]]
- [[Part A - luz-jsonstore Analysis]]
- [[Case Report - DocumentId 386]]
- [[Case Report - DocumentId 588]]

%% ai-graph-end %%