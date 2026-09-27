---
ai_hash: 13b5a7ecba17bd01
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: INC-2026-09-24 Gemini consumption (LUZ)'
status: seedling
tags:
- iam
- identity
- attribution
- cloud-governance
- service-accounts
- confluence-distilled
title: Shared and personal accounts make attribution impossible by construction
type: lesson
---

# Shared and personal accounts make attribution impossible by construction

Logging tells you which **principal** acted. Whether that principal maps to a **person** is a separate question, decided by how the account was created — and no amount of logging can fix a shared or personal account after the fact.

An owner-role inventory from a real cloud project makes the distinction concrete:

| Principal | Resolves to a person? |
|---|---|
| `helios.aavn@gmail.com` | **No** — private Google account, shared by more than one person |
| `android@klara.ch` | **No** — shared account |
| `posapp-…@appspot.gserviceaccount.com` | **No** — default service account holding owner |
| `robin.engbersen@klara.ch` | **Yes** |
| `gabriel.tanguay@epostservice.ch` | **Yes** |

**What a private (non-managed) account costs you**, even when the log names it:

- no login audit — you cannot see who signed in, from where, or when
- no device or session record
- no central session revocation
- no mapping from account to employee

**A shared account is worse: the question is unanswerable by construction.** A perfect log naming `helios.aavn@gmail.com` still does not identify a human, because several people hold it. You have not lost the evidence — the evidence was never capable of answering.

Under a **managed company identity**, the same log entry supports login audit, device and session history, and central revocation: which person, from which device, at what time, and the ability to cut the session immediately.

**Three structural conditions that make this worse**, all present in the source incident:

- **No organisation parent.** The project sat outside the company's cloud organisation, so org policies could not apply — specifically the ones restricting members to owned domains and disabling service-account key creation. Those policies cannot be applied without migrating the project first.
- **Long-lived keys predating the audit window.** Six user-managed service account keys remained, the oldest from 2018, with no key-creation event in the last twelve months. Every live key was older than the retained logs, so none could be correlated to a holder afterwards.
- **A service account holding `roles/owner`.** A default service account with owner is a principal nobody owns.

> [!tip] The finding that outlasts the incident
> The report's framing is worth copying: *"the finding is not the spend, it is that no person can be named for it."* Financial impact was one day and contained. The attribution gap applies to **every future action on that project, including a deliberate one** — which is why it is an operational security finding rather than a billing story.

> [!warning] Removing a committed secret does not revoke it
> The same project had a service account key committed to a repository. Git history is permanent, so deleting the file changes nothing — **rotation and revocation** are the only actions that make a leaked key safe. Handled correctly here: the replacement was created nine minutes before the removal commit landed.

Related: [[GCP Data Access logs are off by default, so data-plane calls are unattributable]] — the other half of the same failure.

Source: [[INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user]] (LUZ, Confluence).

## Related

- [[GCP Data Access logs are off by default, so data-plane calls are unattributable]]

%% ai-graph-start %%

**Related notes:**
- [[GCP Data Access logs are off by default, so data-plane calls are unattributable]]
- [[INC-2026-09-24 - posapp-android-ed1bd - Gemini consumption with no attributable user]]
- [[Per-account credential store should only hold per-identity secrets]]
- [[A wiki export can carry live credentials into git; redact before the first commit]]
- [[Pipe a GCP service-account key straight into a GitHub secret without leaking it]]

%% ai-graph-end %%