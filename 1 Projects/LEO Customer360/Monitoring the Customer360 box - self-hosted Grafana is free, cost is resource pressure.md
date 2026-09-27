---
ai_hash: d3de5d644d675f27
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities:
- Customer360 box
- Grafana
- Prometheus
- Grafana OSS
- cAdvisor
- node_exporter
- redis_exporter
- blackbox_exporter
- Grafana Cloud
- Grafana Enterprise
- managed Prometheus
- Keycloak
- Redis
- FastAPI apps
- Netdata
- Portainer
- VM
- licence cost
- resource cost
- Customer360 UAT box
- full monitoring stack
- Prometheus TSDB
- cgroups
- VM flavor upsize
- Apache-2.0
- AGPL
- RAM
- CPU
- swap/OOM
- containers
- money
- software
- 1 vCPU
source: session 2026-08-19
status: seedling
tags:
- monitoring
- observability
- grafana
- prometheus
- netdata
- portainer
- cost
- customer360
title: Monitoring the Customer360 box - self-hosted Grafana is free, cost is resource
  pressure
type: note
---

# Monitoring the Customer360 box: self-hosted Grafana is free, the cost is resource pressure

When asked "does a full Grafana/Prometheus stack cost more money?" the honest split is
**licence cost vs resource cost**:

- **Licence: $0.** Prometheus, Grafana OSS, cAdvisor, node_exporter, redis_exporter,
  blackbox_exporter are all free (Apache-2.0 / AGPL), self-hosted on a VM you already
  pay for. Only **Grafana Cloud / Grafana Enterprise / managed Prometheus** cost money —
  and you wouldn't be using those.
- **Resource cost is the real one.** The full stack is ~6 containers adding **~0.5–1 GB
  RAM + steady CPU** (cAdvisor continuously walks cgroups — painful on 1 vCPU) + a
  growing Prometheus TSDB on disk.

**Why it matters here:** the Customer360 UAT box is only `s-general-1x2` (1 vCPU / 2 GB /
20 GB) and already runs Keycloak (JVM) + Redis + 3 FastAPI apps — see
[[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]. Adding the
full stack would trigger swap/OOM and force a **VM flavor upsize** — and that upsize is
the only place money actually appears.

**Rule of thumb:** on a tiny/loaded box, prefer a single lightweight agent —
**Netdata** (~100–200 MB, no separate TSDB, real-time dashboard + alarms) or **Portainer**
(~50 MB, container ops UI). Reserve Prometheus+Grafana for when monitoring gets its own
small VM or the box is grown for other reasons. Self-hosting = free software, never free
RAM/CPU.

%% ai-graph-start %%

**Related notes:**
- [[Customer360 UAT api box is a shared 1vCPU-2GB vServer running 5 containers]]
- [[Portainer vs Netdata - both are web UIs, ops vs metrics]]
- [[leo-customer360 deploys as Docker containers on VNG vServer VMs over SSH]]
- [[One Portainer manages many Docker hosts via portaineragent, not a second Portainer]]
- [[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]

**Relations:**
- Customer360 box — *is_monitored_by* — Grafana
- Grafana — *is* — self-hosted
- self-hosted Grafana — *causes* — resource pressure
- licence cost — *for* — full monitoring stack
- licence cost — *is* — $0
- resource cost — *is_the* — real cost
- full monitoring stack — *includes* — Prometheus
- full monitoring stack — *includes* — Grafana OSS
- full monitoring stack — *includes* — cAdvisor
- full monitoring stack — *includes* — node_exporter
- full monitoring stack — *includes* — redis_exporter
- full monitoring stack — *includes* — blackbox_exporter
- Prometheus — *has_license* — Apache-2.0
- Grafana OSS — *has_license* — AGPL
- cAdvisor — *has_license* — Apache-2.0
- node_exporter — *has_license* — Apache-2.0
- redis_exporter — *has_license* — Apache-2.0
- blackbox_exporter — *has_license* — Apache-2.0
- full monitoring stack — *is_hosted_on* — VM
- Grafana Cloud — *requires* — money
- Grafana Enterprise — *requires* — money
- managed Prometheus — *requires* — money
- full monitoring stack — *consists_of* — ~6 containers
- full monitoring stack — *consumes* — ~0.5–1 GB RAM
- full monitoring stack — *consumes* — steady CPU
- cAdvisor — *monitors* — cgroups
- cAdvisor — *is* — painful on 1 vCPU
- Prometheus — *uses* — Prometheus TSDB
- Prometheus TSDB — *stores_data_on* — disk
- Customer360 UAT box — *is_a* — VM
- Customer360 UAT box — *has_specifications* — 1 vCPU / 2 GB / 20 GB
- Customer360 UAT box — *runs* — Keycloak
- Customer360 UAT box — *runs* — Redis
- Customer360 UAT box — *runs* — 3 FastAPI apps
- Adding full monitoring stack — *to* — Customer360 UAT box
- Adding full monitoring stack — *causes* — swap/OOM
- Adding full monitoring stack — *causes* — VM flavor upsize
- VM flavor upsize — *is_a_source_of* — money cost
- Netdata — *is_a* — lightweight agent
- Netdata — *consumes* — ~100–200 MB RAM
- Netdata — *lacks* — separate TSDB
- Netdata — *provides* — real-time dashboard
- Netdata — *provides* — alarms
- Portainer — *is_a* — lightweight agent
- Portainer — *consumes* — ~50 MB RAM
- Portainer — *provides* — container ops UI
- full monitoring stack — *is_recommended_for* — dedicated VM
- full monitoring stack — *is_recommended_for* — grown box
- Self-hosting — *implies* — free software
- Self-hosting — *does_not_imply* — free RAM
- Self-hosting — *does_not_imply* — free CPU
- Customer360 UAT box — *is_a* — tiny/loaded box

%% ai-graph-end %%