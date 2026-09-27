---
ai_hash: f849031f55fb7415
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.79
entities: []
relevance: 0.798
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49450877019/Deep+Dive+EPC+Notification+-+User+Connection+Registry+-+Data+Model
space: Helios
status: reference
tags:
- confluence
- architecture
- space/helios
title: 'Deep Dive: EPC Notification - User Connection Registry - Data Model'
topic: architecture
type: source
updated: 2026-05-27
---

# Deep Dive: EPC Notification - User Connection Registry - Data Model

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-05-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49450877019/Deep+Dive+EPC+Notification+-+User+Connection+Registry+-+Data+Model)
> Relevance 0.798 · topic `architecture`

this page created to follow the EPC notification - User Connection Registry, since the current format provided is not fully cover all the cases we know.

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="27e32494-33ed-454c-870a-7a831549e85c" macro-name="toc">

</div>

------------------------------------------------------------------------

## Deep Dive: User Connection Registry (D) — Data Model

### Current State

**Redis type:** `String` (JSON-serialised object)

**Key pattern:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b7aab2fc-c9a6-428c-aa8f-34851e9ad1c9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{env}:{feature}:{tenantId}:{userId}
e.g.  dev:notify:tenant1:user1
```

</div>

</div>

**Value (**`UserConnectionInfo`**):**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="216c7f90-f42c-4e23-9788-c637fc3309c8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "podId":          "gateway-pod-1",
  "subscriptionId": "sub-gateway-pod-1",
  "topicId":        "dev-epc-ws-gateway-topic",
  "tenantId":       "tenant1",
  "userId":         "user4",
  "connectedAt":    "2025-05-27T03:00:00Z",
  "metadata":       {}
}
```

</div>

</div>

**TTL** (time to live)**:** `86400` seconds (24 h) — configured via `redis.data.default.expiry.duration`, applied at write time but **never refreshed** for active connections.

------------------------------------------------------------------------

### Problem A — `topicId` / `subscriptionId` are pod-level, stored per-user

All users on the same gateway pod share the same `topicId` and `subscriptionId`.  
With 5 000 users on one pod, Redis holds 5 000 copies of the same two strings.

**Consequences:**

- When a pod is redeployed with a new topic, all 5 000 entries become stale simultaneously until users reconnect.

- No way to list "which users are on this pod" without a full `KEYS dev:notify:*` scan.

**Fix — Separate pod metadata into its own key:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4aa23e97-4550-458c-a358-652d9f0bb021" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Key:   {env}:gateway:pod:{podId}
Type:  Hash
Fields:
  topicId        → "dev-epc-ws-gateway-topic"
  subscriptionId → "dev-epc-ws-gateway-sub"
  startedAt      → "2025-05-27T03:00:00Z"
  lastHeartbeat  → "2025-05-27T10:00:00Z"
TTL:   120s  (refreshed every 60s by gateway heartbeat)
```

</div>

</div>

User entry becomes lean — only needs `podId` for routing:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="eb3e70f6-586b-48e9-9c58-3f89ffbcd526" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Key:   {env}:{feature}:conn:{tenantId}:{userId}:{connectionId}
Type:  Hash
Fields:
  podId       → "gateway-pod-1"
  connectedAt → "2025-05-27T03:00:00Z"
  metadata    → "{}"
TTL:   120s (refreshed by heartbeat)
```

</div>

</div>

Routing lookup (notification service):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7d54e0fa-2245-4b8a-8933-4da6201090d2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
1. HGET {env}:{feature}:conn:{tenantId}:{userId}:{connId}  → podId
2. HGET {env}:gateway:pod:{podId} topicId                  → topicId
3. Publish to topicId
```

</div>

</div>

Pod-crash detection is now **automatic**: if a pod dies, its `gateway:pod:{podId}` key expires in 120 s.  
Any message that finds a missing pod key knows not to route there.

------------------------------------------------------------------------

### Problem B — No `connectionId` → only 1 connection per user (multi-tab/browser broken)

**Current key:** `dev:notify:tenant1:user1`  
Every new connection from the same user **overwrites** the previous entry.

#### Real-world scenarios that all fail today

<div>

|  |  |  |
|----|----|----|
| Scenario | What happens now | What should happen |
| **Tab 1 + Tab 2, same browser, same pod** | Tab 2 connect overwrites Tab 1's Redis entry. Tab 1's WS is still alive locally (gateway still streams to it via `openConnections.stream()`), but if the pod restarts Tab 1 is completely lost in registry | Both tabs should receive notifications independently |
| **Tab 1 + Tab 2, same browser, different pods** | Tab 2 overwrites Tab 1's entry. Notification goes only to Tab 2's pod. Tab 1 never receives it | Both pods must receive the publish |
| **Chrome + Firefox, same device** | Last browser to connect wins. Other browser's tab is invisible to the registry | All browsers must receive notifications |
| **Desktop + Mobile** | Same as above — only the last device connected gets notifications | All devices must receive |
| **Tab refresh (reconnect)** | Old connId is removed on close, new connId is registered on open — this works correctly only if we have per-connection keys | Must clean up old entry and register new one atomically |

</div>

#### Why multi-tab within same pod partially "works" today but is fragile

`NotifyPubSubReceiver` scans `openConnections` (in-memory) for all connections matching `tenantId+userId`, so if Tab 1 and Tab 2 are on the **same pod**, both receive the message in-memory.  
But the Redis registry only stores one entry (Tab 2's). If Tab 2 disconnects:

- Redis entry is deleted

- Notification service finds no entry → messages stop for Tab 1 too (even though Tab 1's WS is alive)

If the tabs are on **different pods**, routing goes only to the pod whose entry survived — the other pod never sees the message.

#### Fix — Per-connection key + secondary index + pod-level fan-out deduplication

**Secondary index** (one Set per user, no TTL, managed by connect/disconnect):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c7c9d22f-d5df-4f3f-9d9c-c8195cd9f32b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Key:     {env}:{feature}:user:{tenantId}:{userId}
Type:    Set
Members: {connectionId1}, {connectionId2}, ...
```

</div>

</div>

**Per-connection entry** (lean, routes via `podId`):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dc957960-58cc-4916-892e-3b9c43ae00c8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Key:     {env}:{feature}:conn:{tenantId}:{userId}:{connectionId}
Type:    Hash
Fields:  podId, connectedAt, clientHint (browser/device tag, optional), metadata
TTL:     120s (refreshed by heartbeat)
```

</div>

</div>

**On connect** (gateway generates `connectionId = UUID`):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f0fec112-b8d5-4d86-810f-ef726138f306" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SADD    {env}:notify:user:t1:u1       <connId>
HSET    {env}:notify:conn:t1:u1:<connId>  podId pod-1  connectedAt ...
EXPIRE  {env}:notify:conn:t1:u1:<connId>  120
```

</div>

</div>

**On disconnect:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="59dde689-e4d1-42e6-b36b-504b8afea09c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SREM  {env}:notify:user:t1:u1    <connId>
DEL   {env}:notify:conn:t1:u1:<connId>
```

</div>

</div>

#### Fan-out routing algorithm (notification service)

The key optimization: **group connections by pod** before publishing — if 3 tabs are on the same pod, only 1 Pub/Sub publish is needed (the pod delivers to all 3 tabs locally via `openConnections.stream()`).

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="de11043e-1042-407b-88b3-6a19d9b3858a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
1. SMEMBERS {env}:notify:user:t1:u1
   → [connId1, connId2, connId3]

2. Pipeline: HGET each conn key → podId
   → connId1 → pod-1
   → connId2 → pod-1   (same pod as connId1!)
   → connId3 → pod-2

3. Deduplicate by podId:
   → unique pods = {pod-1, pod-2}
   (skip connIds whose pod key no longer exists → pod crashed)

4. Pipeline: HGET each pod key → topicId
   → pod-1 → "topic-ws-gateway-pod-1"
   → pod-2 → "topic-ws-gateway-pod-2"

5. Publish message once to each unique topicId
   → 2 Pub/Sub publishes for 3 connections (not 3 publishes)
```

</div>

</div>

**Each pod's **`NotifyPubSubReceiver`** then delivers to all local matching connections:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cab63238-4123-446a-9f72-1ce624730b9b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
openConnections.stream()
    .filter(c -> tenantId.equals(c.userData().get(TENANT_ID))
              && userId.equals(c.userData().get(USER_ID)))
    .forEach(c -> c.sendText(eventData)...);
// Delivers to all 2 tabs on pod-1 from a single Pub/Sub message
```

</div>

</div>

Redis pipeline batches steps 2 and 4, so the total round-trips are: **1 SMEMBERS + 1 pipeline** regardless of how many connections the user has.

#### `clientHint` — optional metadata for per-device targeting

Add an optional `clientHint` field to the connection entry to support future use cases like "notify only mobile" or debug logging:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1afb0060-de15-4798-bda6-b855d55389ed" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "podId":       "gateway-pod-1",
  "connectedAt": "2025-05-27T10:00:00Z",
  "clientHint":  "Chrome/125 macOS",
  "metadata":    {}
}
```

</div>

</div>

The gateway populates `clientHint` from the WebSocket handshake `User-Agent` header.

------------------------------------------------------------------------

### Problem C — TTL not refreshed for long-lived connections

The 24 h TTL is set once at connect time. A user browsing for 25 h loses their registry entry.  
The notification service then finds no entry and silently drops messages.

**Fix — Gateway heartbeat every 60 s:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="47183f91-b1f5-4345-8377-5d1b83ac050b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// In NotificationSocket or a @Scheduled job
@Scheduled(every = "60s")
void refreshConnections() {
    openConnections.stream().forEach(conn -> {
        String tenantId = conn.userData().get(TypedKey.forString(TENANT_ID));
        String userId   = conn.userData().get(TypedKey.forString(USER_ID));
        String connId   = conn.userData().get(TypedKey.forString(CONN_ID));
        // Re-PUT or EXPIRE the connection key to reset TTL
        userConnectionService.refreshConnection(tenantId, userId, connId);
    });
}
```

</div>

</div>

With a 120 s TTL and 60 s refresh, a 2× safety margin ensures no false expiry.

------------------------------------------------------------------------

### Problem D — `tenantId` + `userId` duplicated in value

These are already encoded in the key. Storing them again in the value wastes memory and risks inconsistency.

**Fix:** Remove `tenantId` and `userId` from the value object.  
Callers that need them can extract from the key itself.

------------------------------------------------------------------------

### Problem E — `KEYS` pattern scan in production

`listKeysByFeature` calls `keyCmd.keys(pattern)` which maps to Redis `KEYS dev:notify:*`.  
`KEYS` blocks the entire Redis server for the duration of the scan — dangerous on large keyspaces.

**Fix:** Replace `KEYS` with `SCAN` (cursor-based, non-blocking):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1af5bca3-f869-4029-8314-557acda17d98" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Use ReactiveKeyCommands.scan() with match pattern
keyCmd.scan(new ScanArgs().match("dev:notify:*").count(100))
```

</div>

</div>

Or use the secondary index Set (see Problem B) to avoid scanning keys at all.

------------------------------------------------------------------------

### Proposed Final Key Schema

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fb9cc14d-8448-42ae-b6c0-cb833f8c1fba" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Pod lifecycle registry  (one entry per running gateway pod)
{env}:gateway:pod:{podId}
  Type:   Hash
  TTL:    120s  ← refreshed every 60s by gateway heartbeat
  Fields: topicId, subscriptionId, startedAt, lastHeartbeat

# User connection index  (fast lookup of all connections for a user)
{env}:{feature}:user:{tenantId}:{userId}
  Type:    Set
  TTL:     none  ← members added/removed on connect/disconnect
  Members: {connectionId}, ...

# Per-connection entry  (one entry per open WebSocket / SSE stream)
{env}:{feature}:conn:{tenantId}:{userId}:{connectionId}
  Type:   Hash
  TTL:    120s  ← refreshed every 60s by gateway heartbeat
  Fields: podId, connectedAt, clientHint, metadata
```

</div>

</div>

#### Connection lifecycle state machine

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cfe3b740-3821-412f-ac9a-8e4dc38564d4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Browser Tab Opens
      │
      ▼
  [CONNECT]
  ┌─────────────────────────────────────────────────────┐
  │ connectionId = UUID.randomUUID()                     │
  │ SADD  user-index  connectionId                       │
  │ HSET  conn-key    podId, connectedAt, clientHint     │
  │ EXPIRE conn-key   120s                               │
  │ Store connectionId in WS UserData                    │
  └─────────────────────────────────────────────────────┘
      │
      ▼
  [HEARTBEAT] every 60s while connection is alive
  ┌─────────────────────────────────────────────────────┐
  │ EXPIRE conn-key  120s   (reset TTL)                  │
  │ HSET   pod-key   lastHeartbeat = now                 │
  │ EXPIRE pod-key   120s   (reset TTL)                  │
  └─────────────────────────────────────────────────────┘
      │
      ▼
  [DISCONNECT] (graceful or detected by TTL expiry)
  ┌─────────────────────────────────────────────────────┐
  │ SREM  user-index  connectionId                       │
  │ DEL   conn-key                                       │
  └─────────────────────────────────────────────────────┘
```

</div>

</div>

#### Routing algorithm with multi-tab/browser fan-out

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="05246cf6-512f-4294-a2e8-87cff18c5c93" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Notification arrives for tenantId=t1, userId=u1
        │
        ▼
SMEMBERS dev:notify:user:t1:u1  → [connId1, connId2, connId3]
        │
        ▼ (Redis pipeline)
HGET conn:t1:u1:connId1 podId → "pod-1"
HGET conn:t1:u1:connId2 podId → "pod-1"   ← same pod (Tab 1 + Tab 2, same browser)
HGET conn:t1:u1:connId3 podId → "pod-2"   ← different pod (mobile or other browser)
        │
        ▼ (group by podId, skip missing/expired pods)
unique pods: {pod-1, pod-2}
        │
        ▼ (Redis pipeline)
HGET gateway:pod:pod-1 topicId → "topic-pod-1"
HGET gateway:pod:pod-2 topicId → "topic-pod-2"
        │
        ▼
Pub/Sub publish × 2  (not × 3 — pod-1 handles its own 2 tabs locally)

pod-1 receives → openConnections.stream() finds connId1 + connId2 → sends to both tabs
pod-2 receives → openConnections.stream() finds connId3            → sends to mobile tab
```

</div>

</div>

**Before vs After comparison:**

<div>

|  |  |  |
|----|----|----|
| Aspect | Current | Proposed |
| Key | `dev:notify:t1:u1` | `dev:notify:conn:t1:u1:{uuid}` |
| Redis type | String (full JSON rewrite) | Hash (field-level updates) |
| `topicId` stored | Per user (N duplicates) | Per pod (1 copy) |
| Multi-tab same browser | ❌ Last-write wins | ✅ All tabs served |
| Multi-browser / multi-device | ❌ Last-write wins | ✅ All browsers served |
| Tab refresh | ❌ Stale connId remains | ✅ Clean via close→open |
| TTL refresh | ❌ None (24h fixed) | ✅ Heartbeat every 60s |
| Pod crash detection | ❌ Stale entries for up to 24h | ✅ Automatic at 120s |
| User lookup | GET 1 key | SMEMBERS + pipeline HGET |
| Pub/Sub publishes per notification | 1 (routes to 1 pod only) | 1 per unique pod (deduped) |
| Key scan | KEYS (blocking Redis) | Set index O(1) |

</div>

------------------------------------------------------------------------

## Solution: Multi-Tab / Multi-Browser Implementation

> Spans 3 repos. Local changes = `luz_epc_ws_gateway`. Remote = show-only.
>
> **Critical behaviour**: closing Tab 1 must NOT destroy Tab 2.  
> Fix: each tab owns its own `connectionId`; `onClose` deletes **only that tab's** Redis entry.

------------------------------------------------------------------------

### luz_epc_ws_gateway changes

#### `Utils/Constants.java`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="546ff1d9-2581-42ff-b856-d8de2b40ab8b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public static final String CONNECTION_ID = "connectionId";
```

</div>

</div>

------------------------------------------------------------------------

#### `models/UserConnectionInfo.java`

Add `connectionId` to the record:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6fe83f28-cbc8-4fdb-b33b-76f19b277d0a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public record UserConnectionInfo(
        String connectionId,   // ← NEW: UUID per WS connection
        String tenantId,
        String userId,
        Instant connectedAt,
        String subscriptionId,
        String topicId,
        Map<String, String> metadata
) { }
```

</div>

</div>

------------------------------------------------------------------------

#### `NotificationSocket.java`

Generate UUID on open; pass it through `UserData`; use it on close so only **this tab** is removed.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4c51c482-7b60-4aee-96e4-97630caf1eb2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@OnOpen
public EventData onOpen(HandshakeRequest handshake,
                        @PathParam(TENANT_ID) String tenantId,
                        @PathParam(USER_ID) String userId) throws IOException {
    String bearerToken = tokenHolder.getBearerToken();
    String connectionId = UUID.randomUUID().toString();     // ← NEW

    var userData = connection.userData();
    userData.put(TypedKey.forString(TENANT_ID), tenantId);
    userData.put(TypedKey.forString(USER_ID), userId);
    userData.put(TypedKey.forString(CONNECTION_ID), connectionId);  // ← NEW
    userData.put(TypedKey.forString("tenantToken"), bearerToken);

    userConnectionService.saveConnection(tenantId, userId, connectionId, bearerToken); // ← connectionId added

    return new EventData("", "connection.opened",
            "WebSocket connection opened for tenant: " + tenantId + " and user: " + userId,
            LocalDateTime.now().toInstant(ZoneOffset.UTC), "");
}

@OnClose
public void onClose(HandshakeRequest handshake, CloseReason closeReason,
                    @PathParam(TENANT_ID) String tenantId,
                    @PathParam(USER_ID) String userId) throws IOException {
    String tenantToken    = connection.userData().get(TypedKey.forString("tenantToken"));
    String connectionId   = connection.userData().get(TypedKey.forString(CONNECTION_ID)); // ← NEW

    // Only removes THIS tab's entry — other tabs unaffected
    userConnectionService.deleteConnection(tenantId, userId, connectionId, tenantToken);

    EventData byeBye = new EventData("", "connection.closed",
            "WebSocket connection closed for tenant: " + tenantId + " and user: " + userId,
            null, "Close reason: " + closeReason.toString());
    connection.broadcast().sendTextAndAwait(byeBye);
}
```

</div>

</div>

------------------------------------------------------------------------

#### `services/UserConnectionService.java`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="59f8c8c3-b552-4f9d-9260-655d76489b89" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public void saveConnection(String tenantId, String userId, String connectionId, String tenantToken) {
    saveConnection(FEATURE_NOTIFY, tenantId, userId, connectionId, tenantToken);
}

public void saveConnection(String feature, String tenantId, String userId,
                           String connectionId, String tenantToken) {
    var info = new UserConnectionInfo(connectionId, tenantId, userId,
            Instant.now(), pubSubConfig.subscriptionId(), pubSubConfig.topicId(), Map.of());

    // PUT /{feature}/{tenantId}/{userId}/conn/{connectionId}
    // Redis service atomically: SET conn-key + SADD idx-key
    luzEpcRedis.saveUserConnection(tenantToken, feature, tenantId, userId, connectionId, info)
            .log().subscribe().with(
                r -> log.info("Saved conn {} for {}/{}", connectionId, tenantId, userId),
                e -> log.error("Failed to save conn {} for {}/{}", connectionId, tenantId, userId, e));
}

public void deleteConnection(String tenantId, String userId,
                             String connectionId, String tenantToken) {
    deleteConnection(FEATURE_NOTIFY, tenantId, userId, connectionId, tenantToken);
}

public void deleteConnection(String feature, String tenantId, String userId,
                             String connectionId, String tenantToken) {
    // DELETE /{feature}/{tenantId}/{userId}/conn/{connectionId}
    // Redis service atomically: DEL conn-key + SREM idx-key
    luzEpcRedis.deleteUserConnection(tenantToken, feature, tenantId, userId, connectionId)
            .log().subscribe().with(
                r -> log.info("Deleted conn {} for {}/{}", connectionId, tenantId, userId),
                e -> log.error("Failed to delete conn {} for {}/{}", connectionId, tenantId, userId, e));
}

public void refreshConnection(String tenantId, String userId,
                              String connectionId, String tenantToken) {
    // PATCH /{feature}/{tenantId}/{userId}/conn/{connectionId}/heartbeat
    luzEpcRedis.heartbeat(tenantToken, FEATURE_NOTIFY, tenantId, userId, connectionId)
            .log().subscribe().with(
                r -> log.debug("Heartbeat ok for conn {}", connectionId),
                e -> log.warn("Heartbeat failed for conn {}", connectionId, e));
}
```

</div>

</div>

------------------------------------------------------------------------

#### `clients/LuzEpcRedisRestClient.java`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="13c897ad-f9dc-499e-9cf9-8fcf61dc26e2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Path("/")
@RegisterRestClient(configKey = "LUZ_EPC_REDIS_KEY")
@Produces(MediaType.APPLICATION_JSON)
public interface LuzEpcRedisRestClient {

    // ── existing endpoints (unchanged, backward compat) ──────────────────────

    @PUT
    @Path("/{feature}/{tenantId}/{userId}")
    Uni<Response> saveUserConnection(
            @RestHeader(HttpHeaders.AUTHORIZATION) String auth,
            @PathParam("feature") String feature,
            @PathParam("tenantId") String tenantId,
            @PathParam("userId") String userId,
            UserConnectionInfo userConnectionInfo);

    @DELETE
    @Path("/{feature}/{tenantId}/{userId}")
    Uni<Response> deleteUserConnection(
            @RestHeader(HttpHeaders.AUTHORIZATION) String auth,
            @PathParam("feature") String feature,
            @PathParam("tenantId") String tenantId,
            @PathParam("userId") String userId);

    // ── new per-connection endpoints ──────────────────────────────────────────

    /** PUT /{feature}/{tenantId}/{userId}/conn/{connectionId}
     *  Redis: SET conn-key + SADD idx-key (atomic in redis service) */
    @PUT
    @Path("/{feature}/{tenantId}/{userId}/conn/{connectionId}")
    Uni<Response> saveUserConnection(
            @RestHeader(HttpHeaders.AUTHORIZATION) String auth,
            @PathParam("feature") String feature,
            @PathParam("tenantId") String tenantId,
            @PathParam("userId") String userId,
            @PathParam("connectionId") String connectionId,
            UserConnectionInfo userConnectionInfo);

    /** DELETE /{feature}/{tenantId}/{userId}/conn/{connectionId}
     *  Redis: DEL conn-key + SREM idx-key (atomic in redis service) */
    @DELETE
    @Path("/{feature}/{tenantId}/{userId}/conn/{connectionId}")
    Uni<Response> deleteUserConnection(
            @RestHeader(HttpHeaders.AUTHORIZATION) String auth,
            @PathParam("feature") String feature,
            @PathParam("tenantId") String tenantId,
            @PathParam("userId") String userId,
            @PathParam("connectionId") String connectionId);

    /** PATCH /{feature}/{tenantId}/{userId}/conn/{connectionId}/heartbeat
     *  Redis: EXPIRE conn-key 120s */
    @PATCH
    @Path("/{feature}/{tenantId}/{userId}/conn/{connectionId}/heartbeat")
    Uni<Response> heartbeat(
            @RestHeader(HttpHeaders.AUTHORIZATION) String auth,
            @PathParam("feature") String feature,
            @PathParam("tenantId") String tenantId,
            @PathParam("userId") String userId,
            @PathParam("connectionId") String connectionId);

    @GET
    @Path("/redis/ping")
    Uni<String> ping(@RestHeader(HttpHeaders.AUTHORIZATION) String auth);
}
```

</div>

</div>

------------------------------------------------------------------------

#### `services/HeartbeatService.java` (new file)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3aa374ea-0e63-4645-9a79-8309ed62a8e7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
package ch.epost.com.luz.epc.services;

import io.quarkus.scheduler.Scheduled;
import io.quarkus.websockets.next.OpenConnections;
import io.quarkus.websockets.next.UserData.TypedKey;
import jakarta.enterprise.context.ApplicationScoped;
import jakarta.inject.Inject;
import lombok.extern.slf4j.Slf4j;

import static ch.epost.com.luz.epc.Utils.Constants.*;

@Slf4j
@ApplicationScoped
public class HeartbeatService {

    @Inject
    OpenConnections openConnections;

    @Inject
    UserConnectionService userConnectionService;

    /**
     * Refresh TTL for every active connection every 60s.
     * Conn-key TTL = 120s, heartbeat every 60s → 2× safety margin.
     * Prevents live connections from expiring in Redis between a 24h session.
     */
    @Scheduled(every = "60s")
    void refreshAll() {
        openConnections.stream().forEach(conn -> {
            String tenantId     = conn.userData().get(TypedKey.forString(TENANT_ID));
            String userId       = conn.userData().get(TypedKey.forString(USER_ID));
            String connectionId = conn.userData().get(TypedKey.forString(CONNECTION_ID));
            String tenantToken  = conn.userData().get(TypedKey.forString("tenantToken"));

            if (tenantId != null && userId != null && connectionId != null) {
                userConnectionService.refreshConnection(tenantId, userId, connectionId, tenantToken);
            }
        });
        log.debug("Heartbeat refreshed {} connection(s)", openConnections.stream().count());
    }
}
```

</div>

</div>

------------------------------------------------------------------------

### luz_epc_redis_service changes (show code — apply in that repo)

#### `model/ConnectionEntry.java` (new)

Lean per-connection model — does NOT duplicate `tenantId`/`userId` (already in key).

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1895a581-0167-4247-8701-0ab4842bbda1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
package ch.epost.epc.redis.model;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;
import lombok.Getter;
import lombok.Setter;
import java.time.Instant;
import java.util.Map;

@Getter @Setter
@JsonIgnoreProperties(ignoreUnknown = true)
public class ConnectionEntry {
    private String connectionId;
    private String podId;
    private String topicId;
    private String subscriptionId;
    private Instant connectedAt;
    private String clientHint;          // User-Agent hint e.g. "Chrome/125 macOS"
    private Map<String, String> metadata;
}
```

</div>

</div>

------------------------------------------------------------------------

#### `service/RedisService.java` — add Set operations

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="90745580-9563-4f56-82c8-c0b5ccafd396" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Add ReactiveSetCommands field
private final ReactiveSetCommands<String, String> setCmd;

@Inject
public RedisService(ReactiveRedisDataSource redis) {
    this.redis = redis;
    this.keyCmd  = redis.key();
    this.valueCmd = redis.value(String.class);
    this.setCmd  = redis.set(String.class);   // ← NEW
}

/** SADD — add a member to a Set key */
public Uni<Void> sadd(String key, String member) {
    return performRedisOperation("sadd", () ->
            setCmd.sadd(key, member).replaceWithVoid());
}

/** SREM — remove a member from a Set key */
public Uni<Void> srem(String key, String member) {
    return performRedisOperation("srem", () ->
            setCmd.srem(key, member).replaceWithVoid());
}

/** SMEMBERS — get all members of a Set key */
public Uni<Set<String>> smembers(String key) {
    return performRedisOperation("smembers", () ->
            setCmd.smembers(key));
}

/** EXPIRE — reset TTL on an existing key */
public Uni<Void> expireKey(String key, Duration ttl) {
    return performRedisOperation("expire", () ->
            keyCmd.expire(key, ttl).replaceWithVoid());
}

// ── New feature-based index helpers ─────────────────────────────────────────

/** Key for the user connection Set index */
public String toUserIndexKey(String feature, String tenantId, String userId) {
    return "%s:%s:idx:%s:%s".formatted(environmentName, feature, tenantId, userId);
}

/** Key for a specific connection entry */
public String toConnectionKey(String feature, String tenantId, String userId, String connectionId) {
    return "%s:%s:conn:%s:%s:%s".formatted(environmentName, feature, tenantId, userId, connectionId);
}
```

</div>

</div>

------------------------------------------------------------------------

#### `resource/UserConnectionResource.java` — new `/conn` endpoints (add alongside existing)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e14b4f8d-a382-4456-8816-a197ccf3a9cd" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
private static final long CONN_TTL_SECONDS = 120L;

// ── New per-connection endpoints ─────────────────────────────────────────────

/**
 * PUT /{feature}/{tenantId}/{userId}/conn/{connectionId}
 * Atomically: SET conn-key (TTL 120s) + SADD user index
 */
@PUT
@Path("/{feature}/{tenantId}/{userId}/conn/{connectionId}")
public Uni<Response> saveConnection(
        @PathParam("feature") String feature,
        @PathParam("tenantId") String tenantId,
        @PathParam("userId") String userId,
        @PathParam("connectionId") String connectionId,
        ConnectionEntry entry) {

    String connKey  = redisService.toConnectionKey(feature, tenantId, userId, connectionId);
    String indexKey = redisService.toUserIndexKey(feature, tenantId, userId);

    return redisService.set(RequestType.FEATURE_USER, tenantId, userId, connKey, entry,
                            Duration.ofSeconds(CONN_TTL_SECONDS))
            .chain(() -> redisService.sadd(indexKey, connectionId))
            .onItem().transform(v -> Response.status(Response.Status.NO_CONTENT).build());
}

/**
 * DELETE /{feature}/{tenantId}/{userId}/conn/{connectionId}
 * Atomically: DEL conn-key + SREM user index
 * Only removes THIS connection — other tabs unaffected.
 */
@DELETE
@Path("/{feature}/{tenantId}/{userId}/conn/{connectionId}")
public Uni<Response> deleteConnection(
        @PathParam("feature") String feature,
        @PathParam("tenantId") String tenantId,
        @PathParam("userId") String userId,
        @PathParam("connectionId") String connectionId) {

    String connKey  = redisService.toConnectionKey(feature, tenantId, userId, connectionId);
    String indexKey = redisService.toUserIndexKey(feature, tenantId, userId);

    return redisService.deleteRaw(connKey)
            .chain(() -> redisService.srem(indexKey, connectionId))
            .onItem().transform(v -> Response.status(Response.Status.NO_CONTENT).build());
}

/**
 * PATCH /{feature}/{tenantId}/{userId}/conn/{connectionId}/heartbeat
 * Resets TTL to 120s — called every 60s by gateway HeartbeatService.
 */
@PATCH
@Path("/{feature}/{tenantId}/{userId}/conn/{connectionId}/heartbeat")
public Uni<Response> heartbeat(
        @PathParam("feature") String feature,
        @PathParam("tenantId") String tenantId,
        @PathParam("userId") String userId,
        @PathParam("connectionId") String connectionId) {

    String connKey = redisService.toConnectionKey(feature, tenantId, userId, connectionId);
    return redisService.expireKey(connKey, Duration.ofSeconds(CONN_TTL_SECONDS))
            .onItem().transform(v -> Response.status(Response.Status.NO_CONTENT).build());
}

/**
 * GET /{feature}/{tenantId}/{userId}/conn
 * Returns all live connections for a user.
 * Lazy-cleans stale connectionIds whose conn-key has expired (pod crash).
 * Used by luz_epc_notification to fan-out to all pods.
 */
@GET
@Path("/{feature}/{tenantId}/{userId}/conn")
public Uni<Response> listConnections(
        @PathParam("feature") String feature,
        @PathParam("tenantId") String tenantId,
        @PathParam("userId") String userId) {

    String indexKey = redisService.toUserIndexKey(feature, tenantId, userId);

    return redisService.smembers(indexKey)
            .chain(connIds -> {
                if (connIds.isEmpty()) return Uni.createFrom().item(List.<ConnectionEntry>of());

                // Fetch each conn-key; null = expired (pod crashed) → lazy-clean index
                List<Uni<Optional<ConnectionEntry>>> fetches = connIds.stream()
                        .map(cid -> {
                            String connKey = redisService.toConnectionKey(feature, tenantId, userId, cid);
                            return redisService.getRaw(connKey, ConnectionEntry.class)
                                    .onItem().invoke(opt -> {
                                        if (opt.isEmpty()) {  // stale entry
                                            redisService.srem(indexKey, cid)
                                                    .subscribe().with(v -> {}, e -> {});
                                        }
                                    });
                        }).toList();

                return Uni.combine().all().unis(fetches)
                        .with(results -> results.stream()
                                .filter(o -> ((Optional<?>) o).isPresent())
                                .map(o -> ((Optional<ConnectionEntry>) o).get())
                                .toList());
            })
            .onItem().transform(entries -> Response.ok(entries).build());
}
```

</div>

</div>

------------------------------------------------------------------------

### luz_epc_notification changes (show code — apply in that repo)

#### `restclient/EpcRedisRestClient.java` — add list endpoint

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b4629868-11d4-481a-a8b5-0ec790978fd2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/**
 * GET /{feature}/{tenantId}/{userId}/conn
 * Returns all live ConnectionEntry objects for a user across all pods.
 */
@GET
@Path("/{feature}/{tenantId}/{userId}/conn")
List<UserConnectionInfo> listUserConnections(
        @RestHeader(HttpHeaders.AUTHORIZATION) String auth,
        @PathParam("feature") String feature,
        @PathParam("tenantId") String tenantId,
        @PathParam("userId") String userId);
```

</div>

</div>

------------------------------------------------------------------------

#### `service/WSDeliveryService.java` — fan-out by unique topicId

Replace `notifyToWS` with a fan-out version:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0c8e6c92-4d28-4d12-9dd0-e23b10bbc4a4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public void notifyToWS(IncomingMessage message) throws JsonProcessingException {
    if (message == null) { LOG.warn("Null message"); return; }

    String tenantId = message.getTenantId();
    String userId   = message.getUserId();
    String token    = jwtRestCaller.getTenantToken(tenantId, TenantType.COMPANY, true);

    // 1. Get all live connections for this user
    List<UserConnectionInfo> connections =
            client.listUserConnections(token, "notify", tenantId, userId);

    if (connections.isEmpty()) {
        LOG.warnf("No connections found for %s:%s — message dropped", tenantId, userId);
        return;
    }

    // 2. Deduplicate by topicId — same pod handles all its local tabs in one publish
    Map<String, UserConnectionInfo> uniqueByTopic = connections.stream()
            .collect(Collectors.toMap(
                    UserConnectionInfo::getTopicId,
                    c -> c,
                    (existing, duplicate) -> existing));  // keep first if same topic

    LOG.infof("Fan-out for %s:%s → %d connection(s), %d unique pod(s)",
            tenantId, userId, connections.size(), uniqueByTopic.size());

    // 3. Publish once to each unique topicId (pod)
    for (UserConnectionInfo connInfo : uniqueByTopic.values()) {
        message.setUserConnectionInfo(connInfo);
        String payload = ObjectMapperFactory.getObjectMapper().writeValueAsString(message);
        messageBrokerClient.pushMessage(runAsToken, token, connInfo.getTopicId(), payload);
        LOG.infof("Published to topicId %s for %s:%s", connInfo.getTopicId(), tenantId, userId);
    }
}
```

</div>

</div>

------------------------------------------------------------------------

### Tab close safety proof

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ea7b06fd-956b-4697-9725-bc5682e88b4b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Tab 1 (connId-A) + Tab 2 (connId-B) both open on pod-1
Redis:
  idx:t1:u1         = {connId-A, connId-B}
  conn:t1:u1:connId-A = {topicId: topic-pod-1, ...}  TTL 120s
  conn:t1:u1:connId-B = {topicId: topic-pod-1, ...}  TTL 120s

User closes Tab 1:
  onClose(connectionId = connId-A)
  → DEL  conn:t1:u1:connId-A
  → SREM idx:t1:u1  connId-A

Redis after close:
  idx:t1:u1         = {connId-B}            ← connId-A removed, connId-B untouched ✅
  conn:t1:u1:connId-B = {topicId: ...}      ← still alive ✅

Next notification for t1:u1:
  SMEMBERS idx → [connId-B]
  GET conn-B   → topicId = topic-pod-1
  Publish to topic-pod-1
  pod-1 openConnections.stream() finds connId-B WS → delivers ✅
  Tab 2 receives notification ✅
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Store pod-level facts once, not copied into every user key]]
- [[Redis TTL should express liveness and be refreshed by a heartbeat]]
- [[EPC Notification]]
- [[EPC Notification architecture flow]]
- [[EPC API - Load Test]]

%% ai-graph-end %%