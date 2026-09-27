---
ai_hash: 3b73abb8559b3868
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: EPC Notification architecture flow (Helios)'
status: seedling
tags:
- websockets
- sse
- polling
- kubernetes
- nextjs
- real-time
- confluence-distilled
title: Server push choice is decided by proxy idle timeouts and pod affinity, not
  API elegance
type: lesson
---

# Server push choice is decided by proxy idle timeouts and pod affinity, not API elegance

Choosing between WebSocket, Server-Sent Events, and polling for server→client updates looks like an API-design question. In a Kubernetes/CDN deployment it is mostly an **infrastructure** question — the same two constraints break all three options.

**Constraint 1 — everything between you and the browser cuts idle connections.**

- Nginx, Cloud Run and GKE ingress **buffer responses** or **close idle connections after 30–60 s**. SSE needs `proxy_buffering off` plus keep-alive to survive at all.
- GCP Load Balancer idle timeouts (~30 s by default) end a long-poll request prematurely just as readily.
- So "keep the connection open" is not a decision you make in application code; it is a decision your proxy chain has to agree to.

**Constraint 2 — in Kubernetes the client does not stay on one pod.**
Each poll may land on a **different backend pod**, so the pod holding the event is not the pod answering the request. That forces **shared state or a message broker** (Redis, Kafka, Pub/Sub) regardless of which transport you choose. WebSocket and SSE pin a client to one pod for the connection's life, which trades the sharing problem for a sticky-routing and rebalancing problem.

**Per-option gotchas worth knowing before you commit:**

| Option | The specific trap |
|---|---|
| **WebSocket** | Bi-directional and low latency, but a persistent connection is "more complex to scale" — pod affinity, reconnect storms on deploy |
| **SSE** | Simpler, push-only — but **Next.js API routes are not designed for long-lived connections**; the route finishes its response lifecycle early, producing `write after end` / `Cannot write to closed stream`, especially with hot reload in dev. Safari and mobile browsers reconnect inconsistently, creating **duplicate connections** |
| **Polling** | Easiest to build; every request re-sends auth headers and cookies, N clients × short interval becomes many concurrent connections and real CPU/memory on backend pods, and there is always a 1–5 s floor on latency |

> [!warning] The SSE-on-Next.js trap is the sharpest one here
> SSE is the natural choice for one-way push and the code is trivial — which is exactly why teams pick it and then discover the framework closes the response. If your edge is a serverless or request/response-shaped runtime, verify it supports streaming responses **before** designing around SSE, not after.

> [!tip] The question that orders the decision
> Ask **"what is the shortest idle timeout anywhere between my server and the browser?"** first. If that number is small and not yours to change, persistent-connection designs are out and you are choosing between polling intervals — no matter which option reads best on a whiteboard.

Related: [[Store pod-level facts once, not copied into every user key]] — the registry problem you inherit once connections are pinned to pods.

Source: [[EPC Notification architecture flow]] (Helios, Confluence).

## Related

- [[Store pod-level facts once, not copied into every user key]]

%% ai-graph-start %%

**Related notes:**
- [[EPC Notification architecture flow]]
- [[Deep Dive EPC Notification - User Connection Registry - Data Model]]
- [[Score async API designs on crash recovery and multi-instance, not latency]]
- [[Redis TTL should express liveness and be refreshed by a heartbeat]]
- [[A webhook receiver deploys as an always-on service, not a scheduled job]]

%% ai-graph-end %%