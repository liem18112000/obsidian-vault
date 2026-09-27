---
ai_hash: 82546219df2e2f09
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Service Reliability Solution (2025-12-10)'
status: seedling
tags:
- kubernetes
- graceful-shutdown
- rolling-deployment
- reliability
- prestop
- gotcha
title: A rolling deploy drops in-flight requests unless preStop outlives endpoint
  propagation
type: lesson
---

# A rolling deploy drops in-flight requests unless preStop outlives endpoint propagation

When a pod is deleted, two things happen **concurrently, not in order**: the kubelet sends `SIGTERM` to the container, and the endpoint controller removes the pod from the Service's endpoint list. Endpoint removal then has to propagate to every `kube-proxy` / iptables rule / ingress in the cluster.

The app usually wins that race. It shuts down in milliseconds; propagation takes seconds. For that gap, **nodes are still routing traffic to a socket that is already closed** — and the caller sees `connection refused`, not a graceful 503 it might retry.

The fix is to make the container *refuse to die* until propagation has finished:

```yaml
terminationGracePeriodSeconds: 60
lifecycle:
  preStop:
    exec:
      command: ["/bin/sh", "-c", "sleep 15"]
```

The `preStop` sleep runs **before** `SIGTERM`, so the pod keeps serving normally while its endpoint is being withdrawn everywhere. `terminationGracePeriodSeconds` must exceed `preStop` duration **plus** the app's own drain time, or the kubelet `SIGKILL`s mid-drain and you are back where you started.

Counter-intuitive part worth remembering: **the sleep is not a workaround for a slow app — it is a wait for the rest of the cluster to catch up.** No amount of application-side graceful shutdown fixes it, because the app is not the thing that is late.

## Related

- [[An exec-cat readiness probe reports Ready before the server can serve]]
- [[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]]

## Related

- [[An exec-cat readiness probe reports Ready before the server can serve]]

%% ai-graph-start %%

**Related notes:**
- [[An exec-cat readiness probe reports Ready before the server can serve]]
- [[Every production FAILED_TO_STORE traced back to a rolling deploy, not to load]]
- [[Liveness-probe death spiral killing a thread-pool-saturated pod turns overload into a self-perpetuating outage]]
- [[Self-restarting kubectl port-forward keeps long-running scripts alive through drops]]
- [[kubectl rollout status timeout must cover cold image pull time]]

%% ai-graph-end %%