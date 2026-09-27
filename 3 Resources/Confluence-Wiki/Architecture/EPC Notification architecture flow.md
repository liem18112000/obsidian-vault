---
ai_hash: 48961f465b64af07
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 12
depth: 3
entities: []
relevance: 0.832
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48727457897/EPC+Notification+architecture+flow
space: Helios
status: reference
tags:
- confluence
- architecture
- space/helios
title: EPC Notification architecture flow
topic: architecture
type: source
updated: 2026-05-27
---

# EPC Notification architecture flow

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-05-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48727457897/EPC+Notification+architecture+flow)
> Relevance 0.832 · topic `architecture`

<div id="expander-1540713620" class="expand-container conf-macro output-block" hasbody="true" macro-id="1f1e3ed3-cf78-4ff8-bd3c-2fe2c5163f5c" macro-name="expand">

<div id="expander-control-1540713620" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Quick reference using AI</span>

</div>

<div id="expander-content-1540713620" class="expand-content expand-hidden">

To notify **A (Next.js)** when **B (Quarkus)** receives and processes data (and sends it to **C**), here are a few solid architectural options depending on your needs for **real-time**, **scalability**, and **simplicity**:

------------------------------------------------------------------------

### 🔔 Option 1: WebSocket Connection from A to B

- **Use case**: Real-time updates to A when B receives data.

- **How**:

  - A establishes a WebSocket connection to B.

  - When B receives data and sends it to C, it also pushes a message to A via the WebSocket.

- **Pros**: Real-time, low latency.

- **Cons**: *Requires persistent connection*; more complex to scale.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b52b5e33-8be6-496a-8619-8e166aabfd51" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Client (Next.js) <--> Server (Next.js API Route)
                          ↑
                          | WebSocket connection
                          ↓
                    Quarkus Service B
                          ↓
                    Quarkus Service C
```

</div>

</div>

------------------------------------------------------------------------

### 📬 Option 2: Server-Sent Events (SSE)

- **Use case**: One-way real-time updates from B to A.

- **How**:

  - A subscribes to an SSE endpoint exposed by B.

  - B emits events when data is received and forwarded to C.

- **Pros**: Simpler than WebSockets; works well for push-only scenarios.

- **Cons**: Limited browser support compared to WebSockets; no bi-directional communication.

  - **Next.js API routes aren’t designed for long-lived connections**

    - SSE requires the response to stay open indefinitely.

    - Next.js routes often **auto-close** after sending the response → connection breaks.

  - **Reverse proxies and load balancers buffer or timeout**

    - Nginx, Cloud Run, or GKE ingress may **buffer responses** or **cut idle connections** after 30–60s.

    - SSE needs `proxy_buffering off` and `keep-alive`.

  - **Hot-reload and “body closed” errors in dev mode**

    - You may get `write after end` or `Cannot write to closed stream` because Next.js finishes the response lifecycle early.

  - **Limited browser reconnect handling**

    - Safari and mobile browsers sometimes auto-reconnect inconsistently, leading to duplicate connections.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e83e5390-74e5-411d-b55a-58915faf178e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Client (Next.js) <--> Server (Next.js API Route)
                          ↑
                          | SSE subscription
                          ↓
                    Quarkus Service B
                          ↓
                    Quarkus Service C
```

</div>

</div>

------------------------------------------------------------------------

### 📨 Option 3: Polling from A

- **Use case**: Simpler setup, but not real-time.

- **How**:

  - A periodically polls B for new data or status updates.

- **Pros**: Easy to implement.

- **Cons**: Latency and unnecessary load if polling too frequently.

  - **High backend load under many clients**

    - Each client repeatedly opens a new HTTP request to the backend (every few seconds).

    - This causes **a high number of concurrent connections** and **increased CPU/memory usage** on your BE pods.

  - **Unnecessary network overhead**

    - Every polling request re-sends authentication headers, cookies, and payload.

    - This wastes bandwidth and adds latency compared to persistent connections like WebSocket.

  - **Latency between updates**

    - Long polling waits until a new event or a timeout occurs, then the client reconnects.

    - There’s always a **small delay (e.g. 1–5 seconds)** before the client gets the next update.

  - **Harder to scale horizontally**

    - In Kubernetes, each poll may hit a different backend pod.

    - You’ll need **shared state or a message broker** (e.g. Redis, Kafka, Pub/Sub) to synchronize events between pods.

  - **Timeouts and dropped connections**

    - If the backend or load balancer closes idle HTTP connections too early, the long-poll request ends prematurely.

    - Must configure **keep-alive** and **idle timeout** carefully (e.g., GCP Load Balancer defaults are often ~30s).

  - **Higher cost and less responsiveness**

    - Frequent polling increases the total number of HTTP requests.

    - Compared to WebSocket or SSE, it’s less efficient and slower for realtime use cases like chat or notifications.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9568e5c3-d0d9-4696-a3d3-fd1ab2ef19ed" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Client (Next.js) <--> Server (Next.js API Route)
                          ↑
                          | Polling (e.g., every 5s)
                          ↓
                    Quarkus Service B
                          ↓
                    Quarkus Service C
```

</div>

</div>

------------------------------------------------------------------------

### 🧩 Option 4: Event Queue (Kafka, RabbitMQ, etc.)

- **Use case**: Decoupled, scalable architecture.

- **How**:

  - B publishes an event to a message broker when it receives data.

  - A subscribes to relevant events and updates UI accordingly.

- **Pros**: Scalable, decoupled, supports multiple consumers.

- **Cons**: Requires infrastructure and setup.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c28dc483-c2ec-49ee-9598-198119bf01f9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Client (Next.js) <--> Server (Next.js API Route)
                          ↑
                          | Subscribes to event stream
                          ↓
                    Event Broker (Kafka / RabbitMQ)
                          ↑
                          | B publishes event
                          ↓
                    Quarkus Service B
                          ↓
                    Quarkus Service C
```

</div>

</div>

------------------------------------------------------------------------

### 📡 Option 5: Push Notification via API Call

- **Use case**: A is running a backend that can receive notifications.

- **How**:

  - B sends an HTTP request to A’s backend (e.g., `/api/notify`) when it processes data.

  - A updates its state/UI accordingly.

- **Pros**: Simple, direct.

- **Cons**: Requires A to ***expose an endpoint and handle incoming requests***.

  - =\> This option is related to deploy a backend api in luz-next server. Which is refuse by Miracle team, because of security issue. So we decide to not take over this solution for now.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2c3a1c1f-26f8-4f9d-8b8a-f207170dcb40" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Client (Next.js) <--> Server (Next.js API Route)
                          ↑
                          | Client fetches updates
                          ↓
                    Quarkus Service B
                          ↑
                          | HTTP POST /api/notify
                          ↓
                    Quarkus Service C
```

</div>

</div>

------------------------------------------------------------------------

### 🧠 Recommendation

If A is a browser-based app and you want **real-time updates**, go with **WebSockets** or **SSE**. If you prefer a more **decoupled and scalable** approach, use **Kafka or RabbitMQ**. For **simplicity**, an HTTP callback from B to A works well.  
  
**Comparison between options**


![[48727457897-image-20251027-040819.png]]

![[48727457897-image-20251027-052526.png]]



**For our system, which needs to support 2 million clients and scale easily, WebSocket Gateway is a good candidate.**

**Introduction to WebSocket Gateway**

A **WebSocket Gateway** acts as an intermediary layer between clients (e.g., browsers or mobile apps) and backend services. Its main purposes are:

1.  **Manage large-scale client connections** – handle thousands or millions of concurrent WebSocket sessions efficiently.

2.  **Decouple clients and backend** – backend services don’t need to maintain persistent connections with every client.

3.  **Enable real-time messaging** – push notifications, chat messages, or live updates through a centralized gateway.

4.  **Support horizontal scaling** – multiple gateway nodes can share connection states via a message broker like Redis Pub/Sub.

In short, the WebSocket Gateway allows your system to **scale efficiently**, **reduce backend load**, and **deliver real-time updates** reliably to all connected clients.


![[48727457897-image-20251027-052929.png]]



# 1. Websocket Gateway

**Our current modules are running on a blocking I/O model, where each request or WebSocket connection occupies a dedicated thread during I/O operations.**

### Quarkus (blocking I/O) WebSocket performance

A blocking WebSocket implementation (using `javax.websocket` or JSR 356)  
spawns or shares threads from the servlet engine (e.g., Undertow).

➡️ Each WebSocket connection consumes **1 thread** when active,  
or is **managed by a thread pool** that blocks during I/O.

That means:

- Memory footprint is higher (~150–250 KB per connection)

- Thread pool limits concurrency (usually 1k–5k threads safely)

- CPU context switching increases with many open sockets

**Therefore, in the WebSocket Gateway module, we should use non-blocking I/O to handle a large number of concurrent connections efficiently.**

### Quarkus Reactive (Non-Blocking I/O) WebSocket Performance

A **non-blocking WebSocket implementation** in Quarkus (using **Vert.x** or **Reactive Messaging**) runs on an **event-loop model** instead of creating one thread per connection.

➡️ Each connection is handled asynchronously, allowing thousands of active WebSocket sessions to share a small number of threads efficiently.

This design leads to:

- 💾 **Lower memory usage** (~50–80 KB per connection)

- ⚙️ **Higher concurrency** (10k–30k connections per node)

- 🚀 **Reduced CPU overhead** since there’s minimal context switching

**In short, Quarkus Reactive WebSocket offers much better scalability and performance for real-time, high-concurrency applications compared to the traditional blocking model.**

# 2. Redis

**Why we choose Redis?**


![[48727457897-image-20251027-060200.png]]



# 3. Some common issues using Websocket Gateway 


![[48727457897-image-20251029-040602.png]]



# 4. Flow expected with Websocket Gateway 


![[48727457897-image-20251029-092151.png]]



### **Do I need to create a separate Redis container?**

Short answer:

> ⚙️ **No, you don’t need a separate container if you’re using a managed Redis service (like Google Cloud Memorystore).**

<div>

|  |  |  |  |
|----|----|----|----|
| Option | Environment | Pros | Cons |
| **Memorystore (Managed)** | Production | High availability, auto-scaling, Private IP | Paid service |
| **Redis container (Self-hosted)** | Dev / Testing | Free, customizable | No HA, manual maintenance |

</div>

</div>

</div>

Discussed with team Future: this is the thing we want to have


![[48727457897-image-20251031-074026.png]]



As AI support: we generate some idea & plan for the implementation:

- [Research about Websocket Gateway structure](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48809017533/Research+about+Websocket+Gateway+structure)

<!-- -->

- Alternative - another research:

  - INDEX - start here: <a href="https://bitbucket.org/axonivy-prod/luz_unified_inbox/src/3491c66dafce91e51b44a143427c6b835578a58f/memory_bank/solutions/notification/INDEX.md?at=helios%2Fplan_notification_raw_idea" class="external-link" data-card-appearance="inline" data-local-id="eb783a867c2e" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_unified_inbox/src/3491c66dafce91e51b44a143427c6b835578a58f/memory_bank/solutions/notification/INDEX.md?at=helios%2Fplan_notification_raw_idea</a>

  - Questions

    1.  WebSocket, GKE or CloudRun?

        - <a href="https://bitbucket.org/axonivy-prod/luz_unified_inbox/src/9e1acef00416594dec47ea16808a78c4e35b2265/memory_bank/solutions/notification/GKE-VS-CLOUDRUN-DEPLOYMENT.md" class="external-link" data-card-appearance="inline" data-local-id="fd9aefa46785" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_unified_inbox/src/9e1acef00416594dec47ea16808a78c4e35b2265/memory_bank/solutions/notification/GKE-VS-CLOUDRUN-DEPLOYMENT.md</a>

    2.  How does notification service send directly to specific WS pod, no every pods need to receive

        - <a href="https://bitbucket.org/axonivy-prod/luz_unified_inbox/src/9e1acef00416594dec47ea16808a78c4e35b2265/memory_bank/solutions/notification/POD-ROUTING-SOLUTION-GKE.md?at=helios%2Fplan_notification_raw_idea" class="external-link" data-card-appearance="inline" data-local-id="bad9a9c5b6a8" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_unified_inbox/src/9e1acef00416594dec47ea16808a78c4e35b2265/memory_bank/solutions/notification/POD-ROUTING-SOLUTION-GKE.md?at=helios%2Fplan_notification_raw_idea</a>

        - To do it, we will need service account that have permission to connect into gcp and get pods info

        - Use library “io.fabric8.kubernetes-client“ to get pod info (Example code, might need to try with available service account)

          <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a944c5ad-6a1b-4b9f-bb1e-538d135b653d" macro-name="code" style="border-width: 1px;">

          <div class="codeContent panelContent pdl">

          ``` syntaxhighlighter-pre
          // block of code below might not works as expected, just reference code
          private void sampleCode() {
              // need to know which client should be picked, default client is just an example
              try (KubernetesClient client = new DefaultKubernetesClient()) {
                  Service service = client.services().inNamespace("klara-nonprod").withName("luz-websocket-gateway").get();
                  if (service != null && service.getSpec() != null && service.getSpec().getSelector() != null) {
                      Map<String, String> selector = service.getSpec().getSelector();
                      // List pods matching the selector
                      List<Pod> pods = client.pods().inNamespace("klara-nonprod").withLabels(selector).list().getItems();
                      for (Pod pod : pods) {
                          System.out.println("Pod Name: " + pod.getMetadata().getName());
                      }
                  } else {
                      System.out.println("Service not found or has no spec.");
                  }
              }
          }
          ```

          </div>

          </div>

        - Example of notification to specific pod (not test yet)

          <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="201ce2f5-95f9-4f54-ad63-0d1e57a2f4a7" macro-name="code" style="border-width: 1px;">

          <div class="codeContent panelContent pdl">

          ``` syntaxhighlighter-pre
          @ApplicationScoped
          public class NotificationRouter {

              @Inject SessionRegistry sessionRegistry;
              @Inject RestClient httpClient;
              
              public void routeNotification(OperationStatus status) {
                  String userId = status.getUserId();
                  String tenantId = status.getTenantId();
                  
                  // Look up which pod has this user's WebSocket
                  Optional<PodLocation> pod = sessionRegistry.lookup(tenantId, userId);
                  
                  if (pod.isEmpty()) {
                      // User is offline, skip (they'll get update on reconnect)
                      log.debug("User {} offline, skipping notification", userId);
                      return;
                  }
                  
                  // Call internal HTTP endpoint on that specific pod
                  String url = "http://" + pod.get().getInternalIP() + ":" 
                             + pod.get().getPort() + "/internal/notify";
                  
                  try {
                      httpClient.post(url, new NotifyRequest(status));
                      log.info("Routed notification to pod {}", pod.get().getPodId());
                      
                  } catch (Exception e) {
                      log.error("Failed to route to pod, cleaning up stale entry", e);
                      // Pod might have crashed, clean up Redis
                      sessionRegistry.unregister(tenantId, userId);
                  }
              }
          }

          /*
          We still face this kind of exception

          RESTEASY004655: Unable to invoke request: org.apache.http.conn.HttpHostConnectException: Connect to 10.128.3.113:8080 [/10.128.3.113] failed: Connection timed out: connect
          */
          ```

          </div>

          </div>

        - 

    3.  Store mapping: what kind of datastore to have fast lookup?

  - 

-

%% ai-graph-start %%

**Related notes:**
- [[Server push choice is decided by proxy idle timeouts and pod affinity, not API elegance]]
- [[Deep Dive EPC Notification - User Connection Registry - Data Model]]
- [[Performance pain points]]
- [[Using Google PubSub for events between Serverless workflow and Data Index]]
- [[Recipe Best practices implementing scalable distributed applications on cloud]]

%% ai-graph-end %%