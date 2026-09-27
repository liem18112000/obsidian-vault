---
ai_hash: 48afdf5ca7a2fa78
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-25
entities: []
source: session 2026-08-25 (leo-customer360 redis viewer)
status: seedling
tags:
- docker
- networking
- host-gateway
- security
- gotcha
title: Loopback-bind a bridge container that must reach a host-network service
type: howto
---

# Loopback-bind a bridge container that must reach a host-network service

To expose an admin web UI **loopback-only** (for SSH-tunnel access) while it still needs to connect to another service that runs with Docker `--network host`, run the UI container on the **default bridge** and:

- publish it with `-p 127.0.0.1:PORT:CPORT` — this genuinely binds the UI to the host's loopback (nothing on the VPC/public can reach it; only `ssh -L PORT:localhost:PORT` does), and
- reach the host-network service via `--add-host host.docker.internal:host-gateway` (Docker >= 20.10), pointing the UI's connection string at `host.docker.internal:<service-port>`.

**Why not just `--network host` for the UI too?** Under host networking `-p` is ignored and most apps bind `0.0.0.0`, so you can't cleanly restrict the UI to loopback — it becomes reachable by every other host on the private subnet. The bridge + `127.0.0.1` publish is the clean way to get true loopback binding while still crossing into a host-net dependency.

Add HTTP basic auth as defense-in-depth regardless. Surfaced deploying redis-commander (loopback) against a `--network host` broker Redis on the CDP tracking box.

## Related

- [[Co-located --network host box hides cross-box firewall hops]]

%% ai-graph-start %%

**Related notes:**
- [[Docker hostname for reaching a service depends on where the caller runs]]
- [[Co-located --network host box hides cross-box firewall hops]]
- [[L4 LB expose own-login UIs directly, gate no-auth UIs behind oauth2-proxy]]
- [[Rancher Desktop don't pin host.docker.internal to host-gateway]]
- [[Scale one uvicorn service into N replicas on one VM with a docker bridge + local nginx LB]]

%% ai-graph-end %%