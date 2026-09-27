---
ai_hash: 585b29524cde3686
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Deep Dive EPC Notification User Connection Registry (Helios)'
status: seedling
tags:
- redis
- ttl
- heartbeat
- liveness
- distributed-systems
- confluence-distilled
title: Redis TTL should express liveness and be refreshed by a heartbeat
type: lesson
---

# Redis TTL should express liveness and be refreshed by a heartbeat

A TTL set once at write time is a **maximum lifetime**, not a liveness signal — and using it as the latter breaks in both directions.

The broken version: a WebSocket connection registry wrote each user entry with

```
TTL: 86400 seconds (24 h)      # redis.data.default.expiry.duration
```

applied at write time and **never refreshed**. Two failures follow:

- **Live things disappear.** A user connected for more than 24 hours vanishes from the registry while still connected. Notifications stop routing to a session that is demonstrably alive.
- **Dead things linger.** A pod that crashes leaves its users' entries in place for up to 24 hours. Every read in that window returns a connection that no longer exists.

The fix in the same design — applied to the pod key — is the pattern worth carrying:

```
TTL:  120s   (refreshed every 60s by a gateway heartbeat)
      lastHeartbeat → "2025-05-27T10:00:00Z"
```

**TTL ≈ 2× the heartbeat interval.** The key survives exactly as long as something keeps asserting it is alive, and expires within ~2 minutes of that assertion stopping. Now the TTL *means* liveness: presence in Redis is evidence of a live pod, and absence is evidence of a dead one — neither of which was true before.

**Why 2× and not 1×:** one missed heartbeat — a GC pause, a brief network blip, a slow Redis round trip — must not evict a healthy pod. Two intervals tolerate a single miss; more than that and you are slow to notice real death. Three intervals is a reasonable upper bound for jittery environments.

> [!tip] Choose the TTL from what you are willing to be wrong about
> **How long may a dead thing look alive?** That sets the maximum TTL. **How often can you afford to write?** That sets the heartbeat. Then TTL ≈ 2 × heartbeat. Do *not* start from a round number like 24 h — that number encodes no decision about either question.

> [!warning] Heartbeats are writes, so price them
> A 60-second heartbeat per pod is trivial. A 60-second heartbeat **per user** across 5,000 users per pod is 83 writes/second per pod, for data that does not change. Put the heartbeat on the smallest object that can vouch for the rest — here, the pod — and let user entries inherit liveness by referencing it. That is the other reason to separate pod-level from user-level keys: see [[Store pod-level facts once, not copied into every user key]].

Source: [[Deep Dive EPC Notification - User Connection Registry - Data Model]] (Helios, Confluence).

## Related

- [[Store pod-level facts once, not copied into every user key]]

%% ai-graph-start %%

**Related notes:**
- [[Store pod-level facts once, not copied into every user key]]
- [[Deep Dive EPC Notification - User Connection Registry - Data Model]]
- [[Raising negative-cache TTL turns transient failures into long-lived poison]]
- [[A per-checkpoint lastModified timestamp doubles as a liveness heartbeat for timeout detection]]
- [[Server push choice is decided by proxy idle timeouts and pod affinity, not API elegance]]

%% ai-graph-end %%