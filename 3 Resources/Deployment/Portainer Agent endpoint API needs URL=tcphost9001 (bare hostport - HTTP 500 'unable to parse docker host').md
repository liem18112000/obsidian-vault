---
ai_hash: 91e2e5ae030315bc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-23
entities: []
source: leo-customer360 deployments/monitoring, session 2026-08-23
status: seedling
tags:
- portainer
- api
- gotcha
- leo-customer360
title: Portainer Agent endpoint API needs URL=tcp://host:9001 (bare host:port -> HTTP
  500 'unable to parse docker host')
type: gotcha
---

# Portainer Agent endpoint API needs URL=tcp://host:9001 (bare host:port -> HTTP 500 'unable to parse docker host')

When registering a Portainer **Agent** environment via the API (`POST /api/endpoints`, `EndpointCreationType=2`), the `URL` field MUST include the `tcp://` scheme: `URL=tcp://<host>:9001`. A bare `host:9001` makes Portainer return **HTTP 500** with body `{"message":"Unable to initiate communications with environment","details":"Unable to parse docker host `<host>:9001`"}`. In the Portainer UI the Agent field takes a bare `host:9001` (the UI adds the scheme for you), which is why the API requirement is easy to miss. Other required fields for a self-signed agent: `TLS=true TLSSkipVerify=true TLSSkipClientVerify=true`. Diagnosis tip: a 500 gives no detail in the HTTP code alone — re-run the curl and print the RESPONSE BODY to see the real cause. Also: `curl https://<host>:9001/` returning 403 is the agent WORKING (it rejects non-agent requests), not a failure. Source: leo-customer360 deployments/monitoring/deploy-monitoring.sh, 2026-08.

## Related

- [[One Portainer manages many Docker hosts via portaineragent, not a second Portainer]]

%% ai-graph-start %%

**Related notes:**
- [[Portainer agent self-shuts its API after 72h if no server associates]]
- [[One Portainer manages many Docker hosts via portaineragent, not a second Portainer]]
- [[Portainer admin-password bootstrap skips local Docker env - pass -H socket]]
- [[leo-customer360 push to main skips the monitoring step; deploy Portainer agents manually]]
- [[Portainer CSRF origin-invalid behind a reverse proxy - expose it directly]]

%% ai-graph-end %%