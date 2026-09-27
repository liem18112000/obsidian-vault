---
title: "Store pod-level facts once, not copied into every user key"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Deep Dive EPC Notification User Connection Registry (Helios)"
tags: [redis, data-modelling, denormalisation, websockets, cache-invalidation, confluence-distilled]
---

# Store pod-level facts once, not copied into every user key

Copying data that belongs to a *pod* into every *user's* key looks harmless — until you count the copies and try to invalidate them.

A WebSocket gateway stored one Redis `String` per connected user:

```
Key:   {env}:{feature}:{tenantId}:{userId}      e.g. dev:notify:tenant1:user1
Value: { podId, subscriptionId, topicId, tenantId, userId, connectedAt, metadata }
```

`topicId` and `subscriptionId` are **properties of the pod**, identical for every user on it. With 5,000 users on one pod, Redis holds **5,000 copies of the same two strings**.

**Two consequences, both structural:**

1. **Invalidation is fan-out.** Redeploy the pod with a new topic and all 5,000 entries go stale at once, staying wrong until each user reconnects. There is no single place to update.
2. **There is no reverse index.** Answering *"which users are on this pod?"* requires `KEYS dev:notify:*` and a scan of every entry — an O(n) operation that blocks the Redis event loop and is explicitly discouraged in production.

**The fix is to model each fact at the level it belongs to.** Pod data gets its own key:

```
Key:   {env}:gateway:pod:{podId}
Type:  Hash
       topicId        → "dev-epc-ws-gateway-topic"
       subscriptionId → "dev-epc-ws-gateway-sub"
       startedAt      → "2025-05-27T03:00:00Z"
       lastHeartbeat  → "2025-05-27T10:00:00Z"
```

and the user entry keeps only what identifies its owner — `podId` for routing. One write updates the topic for all 5,000 users; the user records never go stale, because they no longer carry pod facts.

> [!tip] The test for a key-value schema
> For each field, ask **what does this actually describe?** If a field describes something other than the key's subject, it is duplicated N times and you have no way to change it atomically. Normalising it out costs one extra lookup and removes an entire class of staleness.

> [!warning] Denormalising is still sometimes right — just do it deliberately
> Copying pod data into user keys does save a round trip on the read path, which matters if that path is hot. The failure here was not the duplication itself; it was duplicating **mutable** data with no invalidation strategy and no reverse index. Denormalise values that do not change, or accept that you must own the invalidation.

If you need "which users are on this pod", add a `Set` keyed by pod and maintain it on connect/disconnect — never reach for `KEYS`.

Related: [[Redis TTL should express liveness and be refreshed by a heartbeat]] — the other flaw in the same data model.

Source: [[Deep Dive EPC Notification - User Connection Registry - Data Model]] (Helios, Confluence).

## Related

- [[Redis TTL should express liveness and be refreshed by a heartbeat]]
