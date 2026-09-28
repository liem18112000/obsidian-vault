---
title: "Cloud Run names who changed a service in lastModifier and the UpdateService audit log"
created: 2026-09-28
type: howto
status: seedling
source: "session 2026-09-28 — testing-agent MCP 503"
tags: [gcp, cloud-run, audit-logging, debugging, gcloud]
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
