---
ai_hash: 55987336a60fcac1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.6
entities: []
relevance: 0.726
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49783570438/INC-2026-09-24+-+posapp-android-ed1bd+-+Gemini+consumption+with+no+attributable+user
space: LUZ
status: reference
tags:
- confluence
- ai-ml
- space/luz
title: INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable
  user
topic: ai_ml
type: source
updated: 2026-09-25
---

# INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-09-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49783570438/INC-2026-09-24+-+posapp-android-ed1bd+-+Gemini+consumption+with+no+attributable+user)
> Relevance 0.726 · topic `ai_ml`

## Summary

The Google Cloud project `posapp-android-ed1bd` ("POS DEVELOPMENT") consumed **118,718,395 Vertex AI Gemini tokens across 28,034 model invocations on 24 September 2026**, all inside a single window of about seven hours. No customer was affected, no KLARA or ePost production service was affected, and the POS application, its users and its data were not involved at any point. The impact is financial only: one day of unbudgeted Gemini consumption.

The usage pattern is an AI assistant or agent pointed at the project: a sweep across twelve Gemini model ids at the start, then sustained use of five of them, with roughly thirty times more input than output. Google's own quota enforcement reacted at 04:45 UTC. Team Helios disabled the Vertex AI API at 08:08 UTC and the traffic stopped at 08:14 UTC.

**The finding of this report is not the spend. It is that no person can be named for it.** The model calls carry no identity in any log, and the accounts that hold access to the project are shared and private Google accounts rather than named company accounts, so there is no trail behind them either. The same gap would apply to any future action on this project, including a deliberate one. This is an operational security finding, and the recommendations below address it.

<div>

|  |  |
|----|----|
| Service | Google Cloud project `posapp-android-ed1bd` (project number 619650653324), Vertex AI |
| Environment | Standalone Firebase / GCP project, outside the KLARA organisation. Not part of the klara-nonprod or klara-prod estate. Serves the non-production POS build variants. |
| Severity | Medium. Financial impact only, contained the same day. The attribution gap is the durable part. |
| Reported | 2026-09-24, noticed by Team Invisible from the spend side |
| Started | 2026-09-24 ~01:00 UTC |
| Mitigated | 2026-09-24 08:08 UTC (Vertex AI API disabled) |
| Resolved | 2026-09-24 08:14 UTC (last model call) |
| Duration of user impact | None. No user-facing impact at any point. |
| Responsible Team | Team Helios owns the project. Investigation by Gabe, Team Invisible. |

</div>

## Timeline

All times UTC, 24 September 2026.

<div>

|  |  |
|----|----|
| Time | Event |
| 01:00 - 04:00 | Model enumeration sweep. One to two calls against each of twelve Gemini model ids. |
| 04:45:44 / 04:45:55 | Google applies producer quota overrides on `aiplatform.googleapis.com` (`ImportProducerOverrides`). Google's own quota enforcement, not an action by Helios or Invisible. |
| 05:00 | Heavy usage begins, concentrated on five models. |
| 05:53:04 / 05:53:09 | The service account `firebase-adminsdk-fon1l@posapp-android-ed1bd.iam.gserviceaccount.com` enables `cloudbilling.googleapis.com` from **24.91.133.0**, user agent `undici` (Node.js). The only source address outside Vietnam seen on the project that day. |
| 06:00 - 07:00 | Peak hour: 8,017 invocations against `gemini-3.6-flash` alone. |
| 08:04 - 08:09 | Last spike, 1,675 invocations in five minutes. |
| 08:08:17 / 08:08:20 | `helios.aavn@gmail.com` disables `aiplatform.googleapis.com` from 113.161.76.38. |
| 08:09 - 08:14 | Traffic collapses to 16 invocations, then stops. |
| 08:21:54 | `helios.aavn@gmail.com` deletes the service account key `d7f2f62284...` on `firebase-adminsdk-fon1l@` from 101.99.33.227. |

</div>

## What was consumed

No other day in the preceding 29 days shows any Vertex AI usage on this project at all.

<div>

|  |  |  |
|----|----|----|
| Model | Input tokens | Output tokens |
| `gemini-3.8-flash` | 63,627,458 | 2,076,433 |
| `gemini-3.7-flash` | 19,245,448 | 1,132,497 |
| `gemini-3.1-pro-preview` | 14,056,857 | 3,625,021 |
| `gemini-3.6-flash` | 6,762,981 | 5,472,253 |
| `gemini-3.5-flash` | 2,030,392 | 436,250 |
| `gemini-3-flash-preview` | 126,104 | 121,373 |
| 2.5 family, 3.1-flash-lite, 3.5-flash-lite, flash-latest | sweep probes only, about 5,300 tokens combined |  |
| Total | 118,718,395 tokens across 28,034 invocations |  |

</div>

The ratio is the informative part. 63.6 million input tokens against 2.1 million output on `gemini-3.8-flash` is a caller resending a large context on every turn for a short answer, which is what an agentic coding tool does. A product feature serving end users would show the inverse ratio, a single model rather than twelve, no startup sweep across model ids, and a recurring daily pattern rather than one burst after years of no usage.

The cost of this consumption is still to be established, from billing account `018A78-E02800-A0A0D1`.

## What this was not

The POS Android application was ruled out directly, not by inference. The repository was cloned and searched at commit `0f30d5a87a`:

- The application contains **no generative AI code of any kind**. A case-insensitive search of the whole tree for `vertexai`, `generativeai`, `gemini`, `firebase-ai` and `aiplatform` returns nothing. No dependency, no import, no call site.

- Vertex AI does not accept API key authentication, so a key extracted from an app binary cannot call it in any case.

- The two Firebase services that would let a client application reach Gemini on a Firebase key and bill it to the project, `firebasevertexai.googleapis.com` and `firebaseml.googleapis.com`, are **both disabled** on this project.

- The production build variants do not use this project at all. `app/src/release/` and `app/src/prodWithDebug/` point at a separate project. `posapp-android-ed1bd` serves the `.bdd`, `.debug`, `.po` and `.stk` variants.

Whatever made the calls held an OAuth2 credential: either a service account key, or a developer's own login through `gcloud auth application-default login`.

## What worked

Three things went right and are worth recording, because they are the pattern to keep.

- **Containment took thirteen minutes.** Team Helios disabled the Vertex AI API at 08:08 and deleted the service account key at 08:21, without being asked and before anyone outside the team raised it. Traffic stopped six minutes after the first action.

- **A previously exposed key was handled correctly.** A service account key committed to the Android repository in January 2025 was rotated and revoked on the same day it was removed from the repository, with the replacement created nine minutes before the removal commit landed. Because git history keeps removed material permanently, revocation is the only thing that makes a committed key safe, and revocation is what happened.

- **The Firestore rules on this project are correctly scoped.** All three databases require an authenticated caller with matching `tenantId` and `workplaceId` claims, and close with an explicit deny on everything else. Data access was never in question in this incident.

## Why no user can be identified

Two separate gaps compound. Either one alone would prevent attribution.

**1. The model calls carry no identity at all.** Vertex AI requests are recorded in Cloud Audit Data Access logs, and Data Access logging is not enabled on this project. Admin Activity logging is always on and cannot be switched off, which is why the API enable and disable, the billing API enable and the key deletion are all visible with their principal. The 28,034 model invocations are not, because they are data plane calls. They exist only as aggregate numbers in Cloud Monitoring, which records the model name but no caller.

**2. The accounts that hold access are shared or private, not named company accounts.** Even with perfect logging, the identity recorded would in most cases not resolve to a person.

<div>

|  |  |
|----|----|
| Principal with `roles/owner` | Resolves to a person? |
| `helios.aavn@gmail.com` | No. Private Google account, shared by more than one person. |
| `android@klara.ch` | No. Shared account. |
| `posapp-android-ed1bd@appspot.gserviceaccount.com` | No. App Engine default service account holding owner. |
| `robin.engbersen@klara.ch` | Yes. |
| `gabriel.tanguay@epostservice.ch` | Yes. |

</div>

For a private Google account there is no login audit, no device or session record, no way to revoke a session centrally, and no way to map the account to an employee. For a shared account the question is unanswerable by construction: even a perfect log naming `helios.aavn@gmail.com` would not identify a person. Under a named company account, the login audit and the administrative controls that come with a managed identity would answer which person, from which device, at what time, and would allow the session to be revoked.

Three further conditions are part of the same picture:

- The project has **no organisation parent**. It sits outside the KLARA organisation, so organisation policies do not apply and cannot be applied without migrating it first. The relevant ones would restrict project members to our own domains and disable service account key creation.

- **Six user-managed service account keys remain**, the oldest from 2018. No key creation event exists in the last twelve months, so every live key predates the audit window and cannot be correlated to a holder after the fact.

- **No budget alert exists.** A full day of Gemini consumption surfaced because a person happened to look.

## Current credential inventory

Stated as found on 24 September 2026, read-only. The last column answers whether each credential is technically capable of having made the Gemini calls. Capable is not the same as used: with Data Access logging off, nothing in the logs attributes the calls to any of them.

<div>

|  |  |  |  |
|----|----|----|----|
| Credential | Role | Created | Could it have made the Gemini calls? |
| `firebase-adminsdk-fon1l@`, key deleted during the incident | `roles/editor` | At least one year old | **Yes.** This is the account seen acting from 24.91.133.0 at 05:53. |
| `firebase-adminsdk-2mt5n@`, 3 keys | `roles/editor` | 2018-12-17, 2018-12-19, 2018-12-28 | **Yes.** Same capability. Nothing indicates it was used. |
| `logging@`, 2 keys | `roles/logging.admin` | 2020-03-20, 2022-12-19 | **No.** The role carries no Vertex AI permission. |
| `firebase-app-distribution@`, 1 key | `roles/firebaseappdistro.admin` | 2025-02-04 | **No.** The role carries no Vertex AI permission. |
| API key "Server key", `7956378578274876773` | No API restriction, no IP restriction | 2018-08-09 | **No.** Vertex AI does not accept API key authentication. |
| API key "Android key", `476571066140309208` | No application restriction, 68 API targets | 2018-08-09 | **No.** Vertex AI does not accept API key authentication. |
| API key "Browser key", `5650771259787495079` | API targets set | 2018-08-09 | **No.** Vertex AI does not accept API key authentication. |

</div>

The two "Yes" rows are the service accounts holding `roles/editor`, which grants 470 `aiplatform.*` permissions including `aiplatform.endpoints.predict`. A service account key mints the OAuth2 token that Vertex AI requires, so an editor key is sufficient on its own. The narrower roles grant no Vertex AI permission at all, and the three API keys are excluded by the authentication method rather than by their restrictions, which is why the unrestricted ones are still a "No" in this column.

**This table is not the full list of things that could have made the calls.** A person's own login through `gcloud auth application-default login` under any of the five owner principals would work equally well and leaves no key behind to inventory. That population is the reason the recommendations start by reducing it rather than by rotating keys.

## Recommended measures

These are Team Invisible's recommendations following the investigation. They are in dependency order: rotating credentials before the population of holders is known and reduced re-issues the same access to the same unbounded set.

1.  **Enable Data Access audit logging for** `aiplatform.googleapis.com`**.** Independent of everything else, cheap, and without it the next occurrence is equally unattributable no matter what else changes.

2.  **Reduce the set of principals that hold access.** Five owners today, of which three do not resolve to a person. Establishing who genuinely needs owner, and moving everyone else to least privilege, is what makes the following steps finite.

3.  **Replace shared and private Google accounts with named company accounts.** This is the single change that would have answered "who" in this incident. A named company identity carries a login audit, device and session records, central session revocation, and a real name against every action. Shared accounts cannot provide any of those regardless of how they are logged.

4.  **Rotate every long-lived credential, after steps 2 and 3.** Six service account keys and three API keys, listed above. The two API keys with no effective restriction and the three 2018 keys on a `roles/editor` account are the ones that matter most.

5.  **Decide who implements this.** Two options, and this is a decision rather than a recommendation: Team Helios implements with Team Invisible advising, or the project's access configuration is handed to Team Invisible to implement and hold. The second option requires Team Invisible to agree to take it on, which has not yet been discussed, so it is recorded here as an open option and not as an offer.

Two further items sit with Team Invisible regardless of the above: budget alerting that covers projects billing outside the main account, and a proposal on whether this project should be migrated into the KLARA organisation so that organisation policies reach it.

## Log Explorer filters

Reusable for the same question on any project. Replace `<PROJECT>`.

Who performed administrative actions, with source address:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="578a7c4f-b500-4463-b666-31fa3a6bb3a6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
logName="projects/<PROJECT>/logs/cloudaudit.googleapis.com%2Factivity"
AND timestamp>="2026-09-24T00:00:00Z"
```

</div>

</div>

Fields to read: `protoPayload.authenticationInfo.principalEmail`, `protoPayload.requestMetadata.callerIp`, `protoPayload.requestMetadata.callerSuppliedUserAgent`, `protoPayload.methodName`.

Service enable and disable events, which is how the start and end of the window were established:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d7023c1a-2213-48de-b7f4-99e4885be514" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
logName="projects/<PROJECT>/logs/cloudaudit.googleapis.com%2Factivity"
AND protoPayload.methodName:"ServiceUsage"
AND protoPayload.resourceName:"aiplatform.googleapis.com"
```

</div>

</div>

Service account key lifecycle:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1f07dfea-a931-4022-9f66-d63f898b4e6f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
logName="projects/<PROJECT>/logs/cloudaudit.googleapis.com%2Factivity"
AND protoPayload.serviceName="iam.googleapis.com"
```

</div>

</div>

Gemini consumption is not in Cloud Logging unless Data Access logging is enabled. It is read from Cloud Monitoring instead, metric `aiplatform.googleapis.com/publisher/online_serving/model_invocation_count` for call counts and `.../token_count` for tokens, grouped by `resource.label.model_user_id` and `metric.label.method`.

## Suggested action items

These are suggestions from the investigation, not agreed work. Nothing here should be carried out on the strength of this page alone. Each item needs to be discussed and decided with the people who own the project and the systems it touches, and the suggested owner below is a starting point for that discussion rather than an assignment. Several of the items interact, and the order matters more than the individual steps.

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p>Suggestion</p></th>
<th><p>Suggested owner</p></th>
<th><p>Status</p></th>
<th><p><strong>Team answers</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Confirm whether the 24 September Gemini run was started by the team, and on which credential</p></td>
<td><p>Team Helios</p></td>
<td><p>Open</p></td>
<td><ul>
<li><p>No</p></li>
</ul></td>
</tr>
<tr>
<td>2</td>
<td><p>Confirm whether 24.91.133.0 is a known address for the team</p></td>
<td><p>Team Helios</p></td>
<td><p>Open</p></td>
<td><ul>
<li><p>No</p></li>
<li><p>Team does NOT know this IP address.</p></li>
<li><p>Team checked → “mid-attack from a US home-internet IP (<code>24.91.133.0</code>, Comcast, New Jersey).“<br />
</p></li>
</ul></td>
</tr>
<tr>
<td>3</td>
<td><p>Enable Data Access audit logging for <code>aiplatform.googleapis.com</code></p></td>
<td><p>Team Helios</p></td>
<td><p>Open</p></td>
<td><ul>
<li><p>No</p></li>
<li><p>Screenshot: It was enabled by <code>principal_email: "firebase-adminsdk-fon1l@posapp-android-ed1bd.iam.gserviceaccount.com"</code><br />
</p>

![[49783570438-image-20260925-011213-20260925-080619.png]]

</li>
</ul></td>
</tr>
<tr>
<td>4</td>
<td><p>Reduce the owner set and move remaining principals to least privilege</p></td>
<td><p>Team Helios</p></td>
<td><p>Open</p></td>
<td><ul>
<li><p>Sep 25: <strong>Reduced</strong> helios.aavn@gmailcom’role to <strong>Viewer</strong></p></li>
</ul></td>
</tr>
<tr>
<td>5</td>
<td><p><span class="inline-comment-marker" data-ref="0b52c8c4-e763-4299-b8cf-35fab0079627">Replace shared and private Google accounts with named company accounts</span></p></td>
<td><p>Team Helios</p></td>
<td><p>Open</p></td>
<td><ul>
<li></li>
</ul></td>
</tr>
<tr>
<td>6</td>
<td><p>Rotate the six service account keys and three API keys, after the two items above</p></td>
<td><p>Team Helios</p></td>
<td><p>Open</p></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td><p>Decide whether implementation stays with Helios or is handed to Team Invisible</p></td>
<td><p>Team Helios and Team Invisible</p></td>
<td><p>Open</p></td>
<td><ul>
<li><p>MT POS &amp; Payment: In the past, we had a discussion with Team Invisible to move to klara-nonprod, but we could NOT → <strong>Stop</strong> at that time<br />
-&gt; Should we recheck and rediscuss?</p></li>
</ul></td>
</tr>
<tr>
<td>8</td>
<td><p>Establish the actual cost from billing account <code>018A78-E02800-A0A0D1</code></p></td>
<td><p>Billing account owner</p></td>
<td><p>Open</p></td>
<td><p>Total Vertex 211,55 CHF - Google Quota 76,86 CHF = 134,69 CHF</p></td>
</tr>
<tr>
<td>9</td>
<td><p>Budget alerting covering projects billing outside the main account</p></td>
<td><p>Team Invisible</p></td>
<td><p>Open</p></td>
<td></td>
</tr>
<tr>
<td>10</td>
<td><p>Proposal on migrating the project into the KLARA organisation</p></td>
<td><p>Team Invisible</p></td>
<td><p>Open</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

## References

- Affected resource: Google Cloud project `posapp-android-ed1bd`, project number 619650653324, created 9 August 2018

- Billing account `018A78-E02800-A0A0D1`

- Service account involved in the billing API enable: `firebase-adminsdk-fon1l@posapp-android-ed1bd.iam.gserviceaccount.com`

- Application repository checked: `luz_pos_android` at `0f30d5a87a`

- Cloud Monitoring metrics: `aiplatform.googleapis.com/publisher/online_serving/model_invocation_count`, `aiplatform.googleapis.com/publisher/online_serving/token_count`

Investigation and write-up: Gabe, Team Invisible.

%% ai-graph-start %%

**Related notes:**
- [[Token input-output ratio fingerprints whether an LLM caller is an agent or a feature]]
- [[GCP Data Access logs are off by default, so data-plane calls are unattributable]]
- [[Shared and personal accounts make attribution impossible by construction]]
- [[Vertex AI Claude usage query - klara-nonprod]]
- [[LUZ-92314 - AI Data Feed Migration issue - Investigate the cache mechanism from Postgresql]]

%% ai-graph-end %%