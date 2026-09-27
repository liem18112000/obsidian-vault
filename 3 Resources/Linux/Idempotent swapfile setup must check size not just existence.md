---
ai_hash: df2f0bfd857deb10
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: customer360 UAT 2026-09-10
status: seedling
tags:
- idempotency
- swap
- provisioning
- gotcha
- bash
title: Idempotent swapfile setup must check size not just existence
type: lesson
---

# Idempotent swapfile setup must check size not just existence

A provisioning step that ensures a swapfile with `if ! swapon --show | grep /swapfile; then create 10G; fi` (or `[ -f /swapfile ]`) is a **false-idempotent**: if a smaller `/swapfile` already exists (e.g. a 2G one from cloud-init), the existence check passes and the 10G creation is silently skipped -- you end up stuck at 2G forever.

Gate on the desired SIZE instead:

```
if [ "$(sudo stat -c%s /swapfile 2>/dev/null || echo 0)" -lt 10737418240 ]; then
  sudo swapoff /swapfile 2>/dev/null || true; sudo rm -f /swapfile
  sudo fallocate -l 10G /swapfile || sudo dd if=/dev/zero of=/swapfile bs=1M count=10240
  sudo chmod 600 /swapfile; sudo mkswap /swapfile; sudo swapon /swapfile
  grep -q "^/swapfile " /etc/fstab || echo "/swapfile none swap sw 0 0" | sudo tee -a /etc/fstab
fi
```

General principle: idempotency checks for a resource with a size/version/config attribute must compare that ATTRIBUTE, not merely presence -- otherwise a pre-existing under-spec instance shadows the intended one. Seen on customer360 UAT 2026-09-10 (a pre-existing 2G /swapfile shadowed the intended 10G for the Dagster box).

## Related

- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]

%% ai-graph-start %%

**Related notes:**
- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
- [[Uncapped Dagster on a swapless 2GB box OOMs the whole host (SSH banner-timeout); cap container memory + add swap]]
- [[Dagster run_monitoring key is max_resume_run_attempts not max_resume_attempts]]
- [[Split Dagster webserver and daemon must share fail-closed Postgres storage]]
- [[Dagster runs stuck in QUEUED forever = orphaned runs leak all concurrency slots]]

%% ai-graph-end %%