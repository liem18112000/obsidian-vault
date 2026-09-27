---
ai_hash: 4ed767ae51b58437
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48955097198'
confluence_path: 'Team Kepler > Developer note > Service Error Analysis Report: FAILED_TO_STORE
  on Production'
created: 2025-12-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
title: Service Reliability Solution
type: source
updated: 2025-12-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48955097198/Service+Reliability+Solution
---

# Service Reliability Solution

*Confluence source · Team Kepler › Developer note › Service Error Analysis Report: FAILED_TO_STORE on Production · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48955097198/Service+Reliability+Solution) · updated 2025-12-10*

------------------------------------------------------------------------

### Executive Summary

This document provides a complete solution to address service reliability issues identified in the klara-prod environment. The implementation is divided into **4 phases** over approximately **4-6 sprints**, prioritizing quick wins that deliver immediate value.

#### Problem Summary

|  |  |  |  |
|----|----|----|----|
| Service | Issue | Impact | Root Cause |
| luz-jsonstore | 25 FAILED_TO_STORE errors/month | Document loss, user complaints | Rolling deployment gaps |
| luz-antivirus | 1000+ HTTP 503/month | Brief scan unavailability | ClamAV definition updates |

#### Solution Overview

|  |  |  |  |
|----|----|----|----|
| Phase | Focus | Key Deliverables | Risk Reduction |
| Phase 1 | Quick Wins (K8s Config) | Graceful shutdown, startup probe | 80% |
| Phase 2 | Readiness Improvements | HTTP readiness probe, health endpoint | 95% |
| Phase 3 | Application Hardening | MongoDB optimization, cache warming | 99% |
| Phase 4 | Monitoring & Resilience | Alerting, retry logic, circuit breakers | 99.9% |

------------------------------------------------------------------------

### Phase 1: Quick Wins (Kubernetes Configuration)

**Timeline:** Sprint 1 **Risk Level:** Low **Downtime Required:** No (rolling update) **Expected Impact:** 80% reduction in FAILED_TO_STORE errors

#### 1.1 Graceful Shutdown (luz-jsonstore)

**Problem:** Pods terminate immediately without draining connections, causing in-flight requests to fail.

**Current State:**

```
# No lifecycle hooks defined
# terminationGracePeriodSeconds: 30 (default)
```

**Solution:**

```
# File: k8s/luz-jsonstore/deployment.yaml
spec:
  terminationGracePeriodSeconds: 60
  containers:
  - name: luz-jsonstore
    lifecycle:
      preStop:
        exec:
          command:
          - /bin/sh
          - -c
          - |
            echo "Initiating graceful shutdown..."
            sleep 15
            echo "Shutdown complete"
```

**Implementation Steps:**

1.  Add `lifecycle.preStop` hook to deployment manifest

2.  Increase `terminationGracePeriodSeconds` to 60

3.  Apply to staging environment

4.  Verify shutdown logs appear: `"Initiating graceful shutdown..."`

5.  Roll out to production

**Verification:**

```
# Check shutdown behavior
kubectl logs -f <pod-name> --previous | grep -i shutdown

# Verify grace period
kubectl get pod <pod-name> -o jsonpath='{.spec.terminationGracePeriodSeconds}'
```

------------------------------------------------------------------------

#### 1.2 Startup Probe (luz-jsonstore)

**Problem:** No protection during application startup; traffic can be routed before WildFly is ready.

**Current State:**

```
# No startupProbe defined
```

**Solution:**

```
# File: k8s/luz-jsonstore/deployment.yaml
startupProbe:
  httpGet:
    path: /luz_jsonstore/api/version
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
  timeoutSeconds: 3
  failureThreshold: 60      # 60 * 5s = 5 minutes max startup time
  successThreshold: 1
```

**Implementation Steps:**

1.  Verify `/luz_jsonstore/api/version` endpoint exists and returns HTTP 200

2.  Add startupProbe to deployment manifest

3.  Test in staging with slow startup scenarios

4.  Roll out to production

**Expected Behavior:**

- Startup probe begins checking after 10 seconds

- WildFly typically ready in 8-17 seconds

- MongoDB connection adds 10-30 seconds

- Total: probe passes within 20-50 seconds

------------------------------------------------------------------------

#### 1.3 Increase Failure Threshold (luz-antivirus)

**Problem:** Single readiness probe failure marks pod as unhealthy during brief ClamAV updates.

**Current State:**

```
readinessProbe:
  failureThreshold: 1    # Too aggressive
  periodSeconds: 2
```

**Solution:**

```
# File: k8s/luz-antivirus/deployment.yaml
readinessProbe:
  httpGet:
    path: /q/health/ready
    port: 9000
  periodSeconds: 2
  failureThreshold: 3     # Allow 6 seconds of brief unavailability
  successThreshold: 1
  timeoutSeconds: 2
```

**Implementation Steps:**

1.  Update failureThreshold from 1 to 3

2.  Apply to staging

3.  Trigger ClamAV update manually: `kubectl exec <pod> -c luz-clamav -- freshclam`

4.  Verify reduced 503 count during update

5.  Roll out to production

**Expected Outcome:**

- 503 responses per ClamAV update: 7-11 → 2-3

------------------------------------------------------------------------

### Phase 2: Readiness Improvements

**Timeline:** Sprint 2 **Risk Level:** Medium **Downtime Required:** No (rolling update) **Expected Impact:** 95% reduction in FAILED_TO_STORE errors

#### 2.1 HTTP Readiness Probe (luz-jsonstore)

**Problem:** File-based readiness probe (`cat .war.deployed`) passes before application is truly ready.

**Current State (BROKEN):**

```
readinessProbe:
  exec:
    command:
    - cat
    - /opt/jboss/wildfly/standalone/deployments/luz_jsonstore.war.deployed
```

**Problem Analysis:**

```
Timeline of pod startup:
  0s   - Container starts
  8s   - WAR deployed, file created
  8s   - Readiness probe PASSES ❌ (too early!)
  10s  - WildFly starts accepting HTTP
  40s  - MongoDB connection established
  40s  - App actually ready ✓
```

**Solution:**

```
# File: k8s/luz-jsonstore/deployment.yaml
readinessProbe:
  httpGet:
    path: /luz_jsonstore/api/health/ready
    port: 8080
  initialDelaySeconds: 30     # Wait for startup probe
  periodSeconds: 5
  timeoutSeconds: 5
  failureThreshold: 6
  successThreshold: 1
```

**Dependency:** Requires Phase 2.2 (Health Endpoint)

------------------------------------------------------------------------

#### 2.2 Health Endpoint Implementation (luz-jsonstore)

**Problem:** No dedicated health endpoint that checks MongoDB connectivity.

**Solution - New File:** `HealthResource.java`

```
// File: src/main/java/ch/klara/luz/jsonstore/rest/HealthResource.java
package ch.klara.luz.jsonstore.rest;

import javax.enterprise.context.ApplicationScoped;
import javax.inject.Inject;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;

import ch.klara.luz.jsonstore.service.JsonStoreMongoDbService;

@Path("/api/health")
@ApplicationScoped
@Produces(MediaType.APPLICATION_JSON)
public class HealthResource {

    @Inject
    private JsonStoreMongoDbService mongoService;

    /**
     * Readiness check - verifies MongoDB is connected and responsive
     * Used by Kubernetes readinessProbe
     */
    @GET
    @Path("/ready")
    public Response ready() {
        try {
            // Quick MongoDB ping (timeout: 3 seconds)
            if (mongoService.isConnected()) {
                return Response.ok()
                    .entity("{\"status\":\"UP\",\"mongodb\":\"connected\"}")
                    .build();
            } else {
                return Response.status(503)
                    .entity("{\"status\":\"DOWN\",\"mongodb\":\"disconnected\"}")
                    .build();
            }
        } catch (Exception e) {
            return Response.status(503)
                .entity("{\"status\":\"DOWN\",\"error\":\"" + e.getMessage() + "\"}")
                .build();
        }
    }

    /**
     * Liveness check - verifies JVM is running
     * Used by Kubernetes livenessProbe
     */
    @GET
    @Path("/live")
    public Response live() {
        return Response.ok()
            .entity("{\"status\":\"UP\"}")
            .build();
    }
}
```

**MongoDB Connection Check Method:**

```
// File: src/main/java/ch/klara/luz/jsonstore/service/JsonStoreMongoDbService.java
// Add this method to existingj class

/**
 * Quick connectivity check for health endpoint
 * @return true if MongoDB is connected and responsive
 */
public boolean isConnected() {
    try {
        MongoClient client = getClient();
        if (client == null) return false;

        // Execute ping command with 3 second timeout
        Document pingResult = client.getDatabase("admin")
            .runCommand(new Document("ping", 1));

        return pingResult.getDouble("ok") == 1.0;
    } catch (Exception e) {
        logger.warn("MongoDB health check failed: " + e.getMessage());
        return false;
    }
}
```

------------------------------------------------------------------------

#### 2.3 Liveness Probe Update (luz-jsonstore)

**Current State:**

```
livenessProbe:
  httpGet:
    path: /luz_jsonstore/api/version
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 120
```

**Solution:**

```
livenessProbe:
  httpGet:
    path: /luz_jsonstore/api/health/live
    port: 8080
  initialDelaySeconds: 60      # Increased to allow startup
  periodSeconds: 30            # More frequent checks
  timeoutSeconds: 5
  failureThreshold: 3
```

------------------------------------------------------------------------

#### Phase 2 Complete Kubernetes Configuration

```
# File: k8s/luz-jsonstore/deployment.yaml (complete probe section)
spec:
  terminationGracePeriodSeconds: 60
  containers:
  - name: luz-jsonstore
    image: gcr.io/klara-prod/luz-jsonstore:latest

    lifecycle:
      preStop:
        exec:
          command: ["/bin/sh", "-c", "sleep 15"]

    startupProbe:
      httpGet:
        path: /luz_jsonstore/api/version
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 5
      timeoutSeconds: 3
      failureThreshold: 60

    readinessProbe:
      httpGet:
        path: /luz_jsonstore/api/health/ready
        port: 8080
      initialDelaySeconds: 30
      periodSeconds: 5
      timeoutSeconds: 5
      failureThreshold: 6

    livenessProbe:
      httpGet:
        path: /luz_jsonstore/api/health/live
        port: 8080
      initialDelaySeconds: 60
      periodSeconds: 30
      timeoutSeconds: 5
      failureThreshold: 3
```

------------------------------------------------------------------------

#### Phase 2 Checklist

|                                     |         |        |                   |
|-------------------------------------|---------|--------|-------------------|
| Task                                | Owner   | Status | Notes             |
| Create HealthResource.java          | Backend | ☐      | New REST endpoint |
| Add isConnected() to MongoDbService | Backend | ☐      | MongoDB ping      |
| Unit tests for health endpoints     | Backend | ☐      |                   |
| Update readinessProbe to HTTP       | DevOps  | ☐      |                   |
| Update livenessProbe                | DevOps  | ☐      |                   |
| Integration testing                 | QA      | ☐      | Simulate failures |
| Staging validation                  | QA      | ☐      |                   |
| Production rollout                  | DevOps  | ☐      |                   |

------------------------------------------------------------------------

### Phase 3: Application Hardening

**Timeline:** Sprint 3-4 **Risk Level:** Medium-High **Downtime Required:** No (rolling update) **Expected Impact:** 99% reduction in FAILED_TO_STORE errors

#### 3.1 MongoDB Connection Timeout Optimization

**Problem:** 60-second connection timeout causes extended startup and slow failover.

**Current State:**

```
// File: Constants.java:37
public static final String MONGODB_CONN_PARAM =
    "&connectTimeoutMS=60000&maxPoolSize=10";  // 60 SECONDS!
```

**Solution:**

```
// File: Constants.java:37
public static final String MONGODB_CONN_PARAM =
    "&connectTimeoutMS=10000"    // 10 seconds (was 60)
    + "&serverSelectionTimeoutMS=10000"  // 10 seconds for replica set selection
    + "&socketTimeoutMS=30000"   // 30 seconds for operations
    + "&maxPoolSize=10"
    + "&minPoolSize=2"           // Keep minimum connections warm
    + "&maxIdleTimeMS=300000";   // 5 minutes idle timeout
```

**Impact:**

- Startup time: 40-60s → 15-25s

- Failover time: 60s → 10s

------------------------------------------------------------------------

#### 3.2 Parallel MongoDB Connection

**Problem:** Sequential connection attempts to replica set members.

**Current State:**

```
// File: JsonStoreMongoDbBase.java:84
// Sequential connection attempts (slow)
for (String host : replicaSetHosts) {
    try {
        connect(host);  // Blocks for 60s on failure
        break;
    } catch (Exception e) {
        continue;
    }
}
```

**Solution:**

```
// File: JsonStoreMongoDbBase.java
import java.util.concurrent.*;
import java.util.List;
import java.util.ArrayList;

/**
 * Connect to MongoDB replica set with parallel connection attempts
 */
private MongoClient connectParallel(List<String> hosts) throws Exception {
    ExecutorService executor = Executors.newFixedThreadPool(hosts.size());
    CompletionService<MongoClient> completionService =
        new ExecutorCompletionService<>(executor);

    try {
        // Submit parallel connection attempts
        for (String host : hosts) {
            completionService.submit(() -> {
                try {
                    return createClient(host);
                } catch (Exception e) {
                    logger.debug("Failed to connect to " + host + ": " + e.getMessage());
                    return null;
                }
            });
        }

        // Return first successful connection
        for (int i = 0; i < hosts.size(); i++) {
            Future<MongoClient> future = completionService.poll(10, TimeUnit.SECONDS);
            if (future != null) {
                MongoClient client = future.get();
                if (client != null) {
                    logger.info("Connected to MongoDB: " + client.getClusterDescription());
                    return client;
                }
            }
        }

        throw new RuntimeException("Failed to connect to any MongoDB host");
    } finally {
        executor.shutdownNow();
    }
}
```

------------------------------------------------------------------------

#### 3.3 MongoDB Cache Warming

**Problem:** Cold cache on startup causes slow first requests.

**Solution - New File:** `CacheWarmer.java`

```
// File: src/main/java/ch/klara/luz/jsonstore/startup/CacheWarmer.java
package ch.klara.luz.jsonstore.startup;

import javax.annotation.PostConstruct;
import javax.ejb.Singleton;
import javax.ejb.Startup;
import javax.inject.Inject;

import org.jboss.logging.Logger;
import ch.klara.luz.jsonstore.service.JsonStoreMongoDbService;

/**
 * Warms up MongoDB connection and caches on application startup.
 * Ensures the pod is ready before Kubernetes routes traffic.
 */
@Singleton
@Startup
public class CacheWarmer {

    private static final Logger logger = Logger.getLogger(CacheWarmer.class);

    @Inject
    private JsonStoreMongoDbService mongoService;

    @PostConstruct
    public void init() {
        logger.info("Starting cache warm-up...");
        long startTime = System.currentTimeMillis();

        try {
            // 1. Establish MongoDB connection
            warmMongoConnection();

            // 2. Pre-load frequently accessed collections
            warmCollections();

            // 3. Execute sample queries to warm query cache
            warmQueryCache();

            long duration = System.currentTimeMillis() - startTime;
            logger.info("Cache warm-up completed in " + duration + "ms");

        } catch (Exception e) {
            logger.error("Cache warm-up failed: " + e.getMessage(), e);
            // Don't throw - let readiness probe handle it
        }
    }

    private void warmMongoConnection() {
        logger.info("Warming MongoDB connection...");
        // Force connection by calling isConnected()
        boolean connected = mongoService.isConnected();
        logger.info("MongoDB connected: " + connected);
    }

    private void warmCollections() {
        logger.info("Warming collections...");
        // List collections to populate collection cache
        mongoService.getDatabase().listCollectionNames().into(new ArrayList<>());
    }

    private void warmQueryCache() {
        logger.info("Warming query cache...");
        // Execute a lightweight find to warm query executor
        mongoService.getDatabase()
            .getCollection("documents")
            .find()
            .limit(1)
            .first();
    }
}
```

------------------------------------------------------------------------

#### 3.4 Vault Retry Logic for HTTP 400

**Problem:** JWT token expiry causes HTTP 400 errors without retry.

**Current State:**

```
// File: VaultService.java:98
// Only retries on 5xx errors
if (response.getStatus() >= 500) {
    retry();
}
```

**Solution:**

```
// File: VaultService.java:98
/**
 * Execute Vault request with retry logic
 * Retries on: 5xx (server errors), 400 (token expired), 429 (rate limited)
 */
private Response executeWithRetry(Invocation.Builder request, int maxRetries) {
    int retries = 0;
    Exception lastException = null;

    while (retries < maxRetries) {
        try {
            Response response = request.invoke();
            int status = response.getStatus();

            // Success
            if (status >= 200 && status < 300) {
                return response;
            }

            // Retry on specific errors
            if (status == 400 || status == 429 || status >= 500) {
                logger.warn("Vault request failed with status " + status +
                    ", retry " + (retries + 1) + "/" + maxRetries);

                // Refresh JWT token on 400
                if (status == 400) {
                    refreshJwtToken();
                }

                retries++;
                Thread.sleep(1000 * retries);  // Exponential backoff
                continue;
            }

            // Non-retryable error
            return response;

        } catch (Exception e) {
            lastException = e;
            retries++;
            logger.warn("Vault request exception: " + e.getMessage() +
                ", retry " + retries + "/" + maxRetries);
        }
    }

    throw new RuntimeException("Vault request failed after " + maxRetries +
        " retries", lastException);
}
```

------------------------------------------------------------------------

#### 3.5 ClamAV Socket Retry (luz-antivirus)

**Problem:** Socket connection fails immediately during ClamAV update without retry.

**Solution:**

```
// File: src/main/java/ch/klara/luz/antivirus/ClamAVClient.java

/**
 * Get ClamAV socket with retry logic for brief unavailability during updates
 */
private Socket getSocket(int timeout) throws IOException {
    int maxRetries = 3;
    int baseDelay = 1000; // 1 second

    for (int attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            Socket socket = new Socket();
            socket.setSoTimeout(timeout);
            socket.connect(new InetSocketAddress(
                this.config.host().orElse("localhost"),
                this.config.port()
            ), timeout);

            logger.debug("ClamAV socket connected on attempt " + attempt);
            return socket;

        } catch (ConnectException e) {
            if (attempt == maxRetries) {
                logger.error("ClamAV socket connection failed after " +
                    maxRetries + " attempts");
                throw e;
            }

            logger.warn("ClamAV socket unavailable (attempt " + attempt +
                "/" + maxRetries + "), retrying in " +
                (baseDelay * attempt) + "ms...");

            try {
                Thread.sleep(baseDelay * attempt);
            } catch (InterruptedException ie) {
                Thread.currentThread().interrupt();
                throw new IOException("Interrupted during retry", ie);
            }
        }
    }

    throw new IOException("Failed to connect to ClamAV after " + maxRetries + " retries");
}
```

------------------------------------------------------------------------

#### Phase 3 Checklist

|  |  |  |  |
|----|----|----|----|
| Task | Owner | Status | Notes |
| Update MongoDB connection timeout | Backend | ☐ | 60s → 10s |
| Implement parallel MongoDB connection | Backend | ☐ | JsonStoreMongoDbBase.java |
| Create CacheWarmer.java | Backend | ☐ | Startup singleton |
| Add Vault retry logic for HTTP 400 | Backend | ☐ | VaultService.java |
| Add ClamAV socket retry | Backend | ☐ | ClamAVClient.java |
| Performance testing | QA | ☐ | Measure startup time |
| Load testing | QA | ☐ | Concurrent connections |
| Staging validation | QA | ☐ |  |
| Production rollout | DevOps | ☐ |  |

------------------------------------------------------------------------

### Phase 4: Monitoring & Resilience

**Timeline:** Sprint 5-6 **Risk Level:** Low **Downtime Required:** No **Expected Impact:** 99.9% reliability, proactive alerting

#### 4.1 Prometheus Metrics

**New metrics to expose:**

```
// File: src/main/java/ch/klara/luz/jsonstore/metrics/CustomMetrics.java
package ch.klara.luz.jsonstore.metrics;

import io.micrometer.core.instrument.*;
import javax.enterprise.context.ApplicationScoped;
import javax.annotation.PostConstruct;
import javax.inject.Inject;

@ApplicationScoped
public class CustomMetrics {

    @Inject
    private MeterRegistry registry;

    private Counter mongoConnectionFailures;
    private Timer mongoQueryLatency;
    private Gauge podReadyDuration;

    @PostConstruct
    public void init() {
        // MongoDB connection failures
        mongoConnectionFailures = Counter.builder("mongodb_connection_failures_total")
            .description("Total MongoDB connection failures")
            .register(registry);

        // MongoDB query latency
        mongoQueryLatency = Timer.builder("mongodb_query_latency_seconds")
            .description("MongoDB query latency")
            .publishPercentiles(0.5, 0.95, 0.99)
            .register(registry);

        // Pod ready duration
        podReadyDuration = Gauge.builder("pod_ready_duration_seconds", this,
            CustomMetrics::getPodReadyDuration)
            .description("Time from pod start to ready")
            .register(registry);
    }

    public void recordMongoFailure() {
        mongoConnectionFailures.increment();
    }

    public Timer.Sample startQueryTimer() {
        return Timer.start(registry);
    }

    public void stopQueryTimer(Timer.Sample sample) {
        sample.stop(mongoQueryLatency);
    }

    private double getPodReadyDuration() {
        // Implementation to track pod ready time
        return 0;
    }
}
```

#### 4.2 Alerting Rules

```
# File: monitoring/alerting-rules.yaml
groups:
- name: luz-jsonstore-alerts
  rules:
  - alert: LuzJsonstoreHighFailureRate
    expr: |
      sum(rate(http_server_requests_seconds_count{
        app="luz-jsonstore",
        status=~"5.."
      }[5m])) /
      sum(rate(http_server_requests_seconds_count{
        app="luz-jsonstore"
      }[5m])) > 0.01
    for: 2m
    labels:
      severity: critical
    annotations:
      summary: "luz-jsonstore failure rate > 1%"
      description: "luz-jsonstore is returning errors at {{ $value | humanizePercentage }}"

  - alert: LuzJsonstorePodRestarting
    expr: |
      increase(kube_pod_container_status_restarts_total{
        container="luz-jsonstore"
      }[1h]) > 5
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "luz-jsonstore pods restarting frequently"
      description: "{{ $value }} restarts in the last hour"

  - alert: LuzJsonstoreStartupSlow
    expr: |
      histogram_quantile(0.95,
        sum(rate(container_start_time_seconds_bucket{
          container="luz-jsonstore"
        }[1h])) by (le)
      ) > 60
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "luz-jsonstore startup time > 60s (p95)"

  - alert: LuzJsonstoreMongoConnectionFailure
    expr: |
      increase(mongodb_connection_failures_total{
        app="luz-jsonstore"
      }[5m]) > 0
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: "MongoDB connection failures detected"

- name: luz-antivirus-alerts
  rules:
  - alert: LuzAntivirusHighUnavailability
    expr: |
      sum(rate(http_server_requests_seconds_count{
        app="luz-antivirus",
        status="503"
      }[5m])) > 100
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "luz-antivirus returning excessive 503s"
      description: "More than 100 503 responses per second"

  - alert: LuzAntivirusScanTimeout
    expr: |
      increase(antivirus_scan_timeout_total[1h]) > 3
    for: 5m
    labels:
      severity: medium
    annotations:
      summary: "Multiple antivirus scan timeouts"
```

#### 4.3 Grafana Dashboard

```
{
  "title": "luz-jsonstore Health Dashboard",
  "panels": [
    {
      "title": "Request Success Rate",
      "type": "stat",
      "targets": [{
        "expr": "sum(rate(http_server_requests_seconds_count{app='luz-jsonstore',status=~'2..'}[5m])) / sum(rate(http_server_requests_seconds_count{app='luz-jsonstore'}[5m]))"
      }]
    },
    {
      "title": "Pod Startup Time",
      "type": "graph",
      "targets": [{
        "expr": "histogram_quantile(0.95, sum(rate(container_start_time_seconds_bucket{container='luz-jsonstore'}[1h])) by (le))"
      }]
    },
    {
      "title": "MongoDB Connection Status",
      "type": "stat",
      "targets": [{
        "expr": "mongodb_up{app='luz-jsonstore'}"
      }]
    },
    {
      "title": "FAILED_TO_STORE Errors",
      "type": "graph",
      "targets": [{
        "expr": "sum(increase(failed_to_store_total[1h]))"
      }]
    }
  ]
}
```

#### 4.4 Circuit Breaker Pattern (Optional)

```
// File: src/main/java/ch/klara/luz/jsonstore/resilience/CircuitBreaker.java
package ch.klara.luz.jsonstore.resilience;

import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicLong;

/**
 * Simple circuit breaker for MongoDB operations
 */
public class CircuitBreaker {

    public enum State { CLOSED, OPEN, HALF_OPEN }

    private State state = State.CLOSED;
    private final AtomicInteger failureCount = new AtomicInteger(0);
    private final AtomicLong lastFailureTime = new AtomicLong(0);

    private final int failureThreshold = 5;
    private final long resetTimeoutMs = 30000; // 30 seconds

    public boolean allowRequest() {
        switch (state) {
            case CLOSED:
                return true;
            case OPEN:
                if (System.currentTimeMillis() - lastFailureTime.get() > resetTimeoutMs) {
                    state = State.HALF_OPEN;
                    return true;
                }
                return false;
            case HALF_OPEN:
                return true;
            default:
                return false;
        }
    }

    public void recordSuccess() {
        failureCount.set(0);
        state = State.CLOSED;
    }

    public void recordFailure() {
        failureCount.incrementAndGet();
        lastFailureTime.set(System.currentTimeMillis());

        if (failureCount.get() >= failureThreshold) {
            state = State.OPEN;
        }
    }
}
```

------------------------------------------------------------------------

#### Phase 4 Checklist

|                                      |           |        |                     |
|--------------------------------------|-----------|--------|---------------------|
| Task                                 | Owner     | Status | Notes               |
| Add Prometheus metrics               | Backend   | ☐      | CustomMetrics.java  |
| Create alerting rules                | DevOps    | ☐      | alerting-rules.yaml |
| Build Grafana dashboard              | DevOps    | ☐      | JSON template       |
| Implement circuit breaker (optional) | Backend   | ☐      | For MongoDB         |
| PagerDuty/Slack integration          | DevOps    | ☐      | Alert routing       |
| Documentation                        | Tech Lead | ☐      | Runbook             |
| Staging validation                   | QA        | ☐      |                     |
| Production rollout                   | DevOps    | ☐      |                     |

------------------------------------------------------------------------

### Implementation Timeline

```
Sprint 1 (Phase 1)
├── Week 1: Implement K8s changes
│   ├── Day 1-2: preStop hook, terminationGracePeriod
│   ├── Day 3-4: startupProbe
│   └── Day 5: luz-antivirus failureThreshold
└── Week 2: Testing & Rollout
    ├── Day 1-3: Staging validation
    └── Day 4-5: Production rollout

Sprint 2 (Phase 2)
├── Week 1: Health endpoint development
│   ├── Day 1-2: HealthResource.java
│   ├── Day 3-4: isConnected() method
│   └── Day 5: Unit tests
└── Week 2: Integration & Rollout
    ├── Day 1-2: HTTP readinessProbe
    ├── Day 3-4: Staging validation
    └── Day 5: Production rollout

Sprint 3-4 (Phase 3)
├── Week 1: MongoDB optimization
│   ├── Day 1-2: Connection timeout (10s)
│   └── Day 3-5: Parallel connection
├── Week 2: Cache warming
│   ├── Day 1-3: CacheWarmer.java
│   └── Day 4-5: Testing
├── Week 3: Retry logic
│   ├── Day 1-2: Vault retry (HTTP 400)
│   └── Day 3-5: ClamAV socket retry
└── Week 4: Testing & Rollout
    ├── Day 1-3: Performance testing
    └── Day 4-5: Production rollout

Sprint 5-6 (Phase 4)
├── Week 1-2: Monitoring
│   ├── Prometheus metrics
│   ├── Alerting rules
│   └── Grafana dashboard
└── Week 3-4: Documentation & Training
    ├── Runbook creation
    └── Team training
```

------------------------------------------------------------------------

### Testing Strategy

#### Unit Tests

```
// File: src/test/java/ch/klara/luz/jsonstore/rest/HealthResourceTest.java
@Test
public void testReadinessWhenMongoConnected() {
    when(mongoService.isConnected()).thenReturn(true);

    Response response = healthResource.ready();

    assertEquals(200, response.getStatus());
    assertTrue(response.getEntity().toString().contains("UP"));
}

@Test
public void testReadinessWhenMongoDisconnected() {
    when(mongoService.isConnected()).thenReturn(false);

    Response response = healthResource.ready();

    assertEquals(503, response.getStatus());
    assertTrue(response.getEntity().toString().contains("DOWN"));
}
```

#### Integration Tests

```
#!/bin/bash
# File: tests/integration/test_rolling_update.sh

echo "Testing rolling update with zero downtime..."

# Start continuous requests
while true; do
    curl -s -o /dev/null -w "%{http_code}" \
        http://luz-jsonstore:8080/luz_jsonstore/api/version
    sleep 0.1
done &
REQUEST_PID=$!

# Trigger rolling update
kubectl set image deployment/luz-jsonstore \
    luz-jsonstore=gcr.io/klara-prod/luz-jsonstore:new-version

# Wait for rollout
kubectl rollout status deployment/luz-jsonstore

# Stop requests and check results
kill $REQUEST_PID

# Analyze results
echo "Checking for 503 errors during rollout..."
```

<img src="http://localhost:63342/markdownPreview/1723177220/commandRunner/runrun.png" class="confluence-embedded-image confluence-external-resource image-center" data-image-src="http://localhost:63342/markdownPreview/1723177220/commandRunner/runrun.png" loading="lazy" width="250" />

#### Load Tests

```
# File: tests/load/k6-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
    stages: [
        { duration: '2m', target: 100 },  // Ramp up
        { duration: '5m', target: 100 },  // Steady state
        { duration: '2m', target: 200 },  // Peak load
        { duration: '2m', target: 0 },    // Ramp down
    ],
    thresholds: {
        http_req_failed: ['rate<0.01'],   // <1% errors
        http_req_duration: ['p(95)<500'], // 95% under 500ms
    },
};

export default function() {
    const res = http.get('http://luz-jsonstore:8080/luz_jsonstore/api/health/ready');
    check(res, {
        'status is 200': (r) => r.status === 200,
    });
    sleep(0.1);
}
```

------------------------------------------------------------------------

### Rollback Plan

#### Quick Rollback (Kubernetes)

```
# Rollback to previous deployment
kubectl rollout undo deployment/luz-jsonstore

# Rollback to specific revision
kubectl rollout undo deployment/luz-jsonstore --to-revision=3

# Verify rollback
kubectl rollout status deployment/luz-jsonstore
```

#### Application Rollback

```
# Revert to previous image
kubectl set image deployment/luz-jsonstore \
    luz-jsonstore=gcr.io/klara-prod/luz-jsonstore:previous-version

# Scale down if issues persist
kubectl scale deployment/luz-jsonstore --replicas=0
```

#### Feature Flags (Optional)

```
// Disable new features via environment variable
if (System.getenv("ENABLE_CACHE_WARMER") != null) {
    cacheWarmer.init();
}
```

------------------------------------------------------------------------

### Success Metrics

#### Key Performance Indicators (KPIs)

|                            |         |        |             |
|----------------------------|---------|--------|-------------|
| Metric                     | Current | Target | Measurement |
| FAILED_TO_STORE/month      | 25      | \< 3   | GCP Logging |
| Pod startup time (p95)     | 40-60s  | \< 25s | Prometheus  |
| Deployment downtime        | 30-60s  | 0s     | Monitoring  |
| MongoDB failover time      | 60s     | \< 15s | Prometheus  |
| HTTP 503 per ClamAV update | 7-11    | 2-3    | GCP Logging |
| Scan timeout failures      | 6/month | 0      | GCP Logging |

#### Monitoring Queries

```
# FAILED_TO_STORE count (weekly)
gcloud logging read 'labels."k8s-pod/app"="luz-eletter" AND "FAILED_TO_STORE"' \
    --project=klara-prod --freshness=7d --format=json | jq length

# Pod startup time
kubectl get pods -l app=luz-jsonstore -o json | \
    jq '.items[].status.conditions[] |
        select(.type=="Ready") | .lastTransitionTime'

# HTTP 503 during updates
gcloud logging read 'labels."k8s-pod/app"="luz-antivirus" AND "status-code=503"' \
    --project=klara-prod --freshness=1d --format=json | jq length
```

------------------------------------------------------------------------

### Risk Assessment

|  |  |  |  |
|----|----|----|----|
| Risk | Probability | Impact | Mitigation |
| Health endpoint impacts performance | Low | Medium | Lightweight ping, 5s timeout |
| Cache warmer delays startup | Medium | Low | Async execution, timeout |
| Parallel MongoDB breaks existing logic | Low | High | Extensive testing, feature flag |
| preStop hook extends deployment time | Low | Low | 15s is acceptable |
| Circuit breaker false positives | Medium | Medium | Conservative thresholds |

------------------------------------------------------------------------

### Documentation Updates Required

|                                   |           |                       |
|-----------------------------------|-----------|-----------------------|
| Document                          | Owner     | Changes               |
| k8s/luz-jsonstore/deployment.yaml | DevOps    | Probes, lifecycle     |
| k8s/luz-antivirus/deployment.yaml | DevOps    | failureThreshold      |
| Runbook: luz-jsonstore            | Ops       | Troubleshooting steps |
| Architecture diagram              | Tech Lead | Health endpoints      |
| Incident response playbook        | Ops       | New alerts            |

------------------------------------------------------------------------

### Appendix A: Complete Kubernetes Manifest (luz-jsonstore)

```
# File: k8s/luz-jsonstore/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: luz-jsonstore
  namespace: klara-prod
  labels:
    app: luz-jsonstore
spec:
  replicas: 7
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 2
  selector:
    matchLabels:
      app: luz-jsonstore
  template:
    metadata:
      labels:
        app: luz-jsonstore
    spec:
      terminationGracePeriodSeconds: 60

      containers:
      - name: luz-jsonstore
        image: gcr.io/klara-prod/luz-jsonstore:latest
        ports:
        - containerPort: 8080
          name: http

        resources:
          requests:
            memory: "512Mi"
            cpu: "250m"
          limits:
            memory: "1Gi"
            cpu: "1000m"

        env:
        - name: MONGODB_CONNECTION_TIMEOUT
          value: "10000"
        - name: ENABLE_CACHE_WARMER
          value: "true"

        lifecycle:
          preStop:
            exec:
              command:
              - /bin/sh
              - -c
              - |
                echo "$(date) - Starting graceful shutdown..."
                sleep 15
                echo "$(date) - Shutdown complete"

        startupProbe:
          httpGet:
            path: /luz_jsonstore/api/version
            port: 8080
          initialDelaySeconds: 10
          periodSeconds: 5
          timeoutSeconds: 3
          failureThreshold: 60
          successThreshold: 1

        readinessProbe:
          httpGet:
            path: /luz_jsonstore/api/health/ready
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 5
          timeoutSeconds: 5
          failureThreshold: 6
          successThreshold: 1

        livenessProbe:
          httpGet:
            path: /luz_jsonstore/api/health/live
            port: 8080
          initialDelaySeconds: 60
          periodSeconds: 30
          timeoutSeconds: 5
          failureThreshold: 3
```

------------------------------------------------------------------------

### Appendix B: Complete Kubernetes Manifest (luz-antivirus)

```
# File: k8s/luz-antivirus/deployment.yaml (probe section only)
readinessProbe:
  httpGet:
    path: /q/health/ready
    port: 9000
  periodSeconds: 2
  timeoutSeconds: 2
  failureThreshold: 3      # Changed from 1
  successThreshold: 1
```

------------------------------------------------------------------------

### Appendix C: File Change Summary

|  |  |  |
|----|----|----|
| File | Change Type | Description |
| k8s/luz-jsonstore/deployment.yaml | Modify | Add probes, lifecycle, grace period |
| k8s/luz-antivirus/deployment.yaml | Modify | Increase failureThreshold |
| Constants.java:37 | Modify | MongoDB timeout 60s → 10s |
| JsonStoreMongoDbService.java | Modify | Add isConnected() method |
| JsonStoreMongoDbBase.java:84 | Modify | Parallel MongoDB connection |
| VaultService.java:98 | Modify | Add retry for HTTP 400 |
| ClamAVClient.java:68 | Modify | Add socket retry logic |
| HealthResource.java | **New** | Health endpoints |
| CacheWarmer.java | **New** | MongoDB cache warming |
| CustomMetrics.java | **New** | Prometheus metrics |
| CircuitBreaker.java | **New** | Resilience pattern |

%% ai-graph-start %%

**Related notes:**
- [[Service Error Analysis Report - FAILED_TO_STORE on Production]]
- [[Part B - luz-antivirus Analysis]]
- [[Part A - luz-jsonstore Analysis]]
- [[Case Report - DocumentId 386]]
- [[Case Report - DocumentId 588]]

%% ai-graph-end %%