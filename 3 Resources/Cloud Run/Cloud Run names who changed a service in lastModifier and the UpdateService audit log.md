---
ai_hash: 1e056b798588bb88
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-28'
created: 2026-09-28
entities:
- Cloud Run
- service
- serving.knative.dev/lastModifier
- gcloud run services describe
- serving.knative.dev/creator
- admin-activity audit log
- gcloud logging read
- protoPayload.serviceName="run.googleapis.com"
- protoPayload.methodName:"Services.UpdateService"
- protoPayload.resourceName:"<svc>"
- principalEmail
- Terraform
- gcloud run services update
- A disabled Cloud Run service 503s at the edge and never reaches your app
- A scaled-to-zero service in a shared cloud project is a decision, not a fault
- '503'
- cost decision
- control-plane mutation
- identity
- person
- configuration
- timestamp
- repo
- infrastructure
- manual change
- UpdateService (method)
source: session 2026-09-28 — testing-agent MCP 503
status: seedling
tags:
- gcp
- cloud-run
- audit-logging
- debugging
- gcloud
title: Cloud Run names who changed a service in lastModifier and the UpdateService
  audit log
type: howto
---

# Cloud Run names who changed a service in lastModifier and the UpdateService audit log

Every Cloud Run service carries the identity of whoever last touched it in the `serving.knative.dev/lastModifier` annotation. That one field turns "why is this service configured like this?" into a person you can ask, before you start guessing at causes or reverting someone else's deliberate change.

```bash
gcloud run services describe "$svc" --region "$r" --project "$p" \
  --format='value(metadata.annotations["serving.knative.dev/lastModifier"])'
```

`serving.knative.dev/creator` is the complementary field — who created the service originally.

The annotation gives you *who* but not *when* or *what*. For that, query the admin-activity audit log, which records every control-plane mutation:

```bash
gcloud logging read 'protoPayload.serviceName="run.googleapis.com"
  AND protoPayload.methodName:"Services.UpdateService"
  AND protoPayload.resourceName:"<svc>"' \
  --project "$p" --limit 5 --freshness 30d \
  --format="value(timestamp,protoPayload.authenticationInfo.principalEmail)"
```

Admin-activity logs are on by default and free, so this works without anyone having enabled anything. `principalEmail` is only populated on the first entry of a burst — the follow-up entries from the same operation come back blank — so read the whole list, not just the newest row.

Worth reaching for whenever shared-project infrastructure behaves differently than your Terraform says it should: a manual `gcloud run services update` leaves no trace in the repo, but it always leaves these two.

Concrete use: diagnosing a service that had been [[A disabled Cloud Run service 503s at the edge and never reaches your app|disabled by manual scale-to-zero]] — the annotation named the colleague and the audit log dated it to three days earlier, which reframed the 503 from a breakage into [[A scaled-to-zero service in a shared cloud project is a decision, not a fault|somebody else's cost decision]].

## Related

- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[A scaled-to-zero service in a shared cloud project is a decision]]
- [[not a fault]]

%% ai-graph-start %%

**Related notes:**
- [[A scaled-to-zero service in a shared cloud project is a decision, not a fault]]
- [[A disabled Cloud Run service 503s at the edge and never reaches your app]]
- [[Terraform-managed Cloud Run set env flags in TF, not gcloud run update]]
- [[Cloud Run 401 response body distinguishes GFEIAM rejection from app-level auth]]
- [[Cloud Run won't redeploy on a latest digest change — apply by immutable digest]]

**Relations:**
- Cloud Run — *manages* — service
- service — *carries annotation* — serving.knative.dev/lastModifier
- serving.knative.dev/lastModifier — *identifies* — identity
- identity — *is a* — person
- gcloud run services describe — *retrieves* — serving.knative.dev/lastModifier
- service — *carries annotation* — serving.knative.dev/creator
- serving.knative.dev/creator — *is complementary to* — serving.knative.dev/lastModifier
- admin-activity audit log — *records* — control-plane mutation
- gcloud logging read — *queries* — admin-activity audit log
- gcloud logging read — *filters by* — protoPayload.serviceName="run.googleapis.com"
- gcloud logging read — *filters by* — protoPayload.methodName:"Services.UpdateService"
- gcloud logging read — *filters by* — protoPayload.resourceName:"<svc>"
- admin-activity audit log — *contains field* — principalEmail
- principalEmail — *provides* — identity
- admin-activity audit log — *contains field* — timestamp
- timestamp — *provides* — when
- admin-activity audit log — *provides* — what
- gcloud run services update — *leaves trace in* — admin-activity audit log
- Terraform — *manages* — infrastructure
- gcloud run services update — *is a type of* — manual change
- manual change — *does not leave trace in* — repo
- A disabled Cloud Run service 503s at the edge and never reaches your app — *is related to* — A scaled-to-zero service in a shared cloud project is a decision, not a fault
- A disabled Cloud Run service 503s at the edge and never reaches your app — *describes* — 503
- A scaled-to-zero service in a shared cloud project is a decision, not a fault — *explains* — cost decision
- service — *has* — configuration
- UpdateService (method) — *is recorded in* — admin-activity audit log
- service — *can be* — disabled
- disabled service — *causes* — 503
- scaled-to-zero service — *is a* — cost decision
- scaled-to-zero service — *causes* — 503
- UpdateService (method) — *is a type of* — control-plane mutation

%% ai-graph-end %%