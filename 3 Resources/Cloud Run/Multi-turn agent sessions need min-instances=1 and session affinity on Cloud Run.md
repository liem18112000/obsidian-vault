---
ai_hash: c5d0c17a45993e16
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27
status: seedling
tags:
- cloud-run
- state
- scaling
- agents
- gotcha
title: Multi-turn agent sessions need min-instances=1 and session affinity on Cloud
  Run
type: lesson
---

# Multi-turn agent sessions need min-instances=1 and session affinity on Cloud Run

Any request flow that keeps conversation state **in process memory** breaks on Cloud Runs default autoscaling (many instances) and scale-to-zero. Concretely, a multi-turn A2A + MCP-bridge flow holds state in three single-instance places at once: the A2A agents `InMemoryTaskStore`, the paused A2A task, and the bridges in-memory `context_id -> task_id` map. A follow-up request routed to a different instance (or after a cold start) loses all three and the turn fails.

Fix for such stateful flows: run the service(s) with `--min-instances 1 --session-affinity --no-cpu-throttling` so a session stays pinned to one warm instance; or externalize the state (Redis/Firestore, a persistent A2A task store). Stateless one-shot calls (e.g. a single gather) need none of this and scale freely. Note the MCP `stateless_http=True` flag only drops the MCP session layer — it does not fix app-level session state.

See [[Cloud Run one-port limit forces co-located HTTP servers into separate services]], [[A2A input-required tasks must be answered on the same taskId and contextId]].

## Related

- [[Cloud Run one-port limit forces co-located HTTP servers into separate services]]
- [[A2A input-required tasks must be answered on the same taskId and contextId]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run one-port limit forces co-located HTTP servers into separate services]]
- [[Co-locating a stateful MCP bridge as an agent sidecar couples their scaling]]
- [[Cloud Run service-to-service with an app bearer needs the callee public (Authorization header collision)]]
- [[A2A to_a2a task_store and runner are separate persistence params]]
- [[A2A input-required tasks must be answered on the same taskId and contextId]]

%% ai-graph-end %%