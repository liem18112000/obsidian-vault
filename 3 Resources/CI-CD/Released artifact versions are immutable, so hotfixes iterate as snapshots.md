---
ai_hash: 8bde7ad917a9dbfa
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: GCP Release Process with Google Cloud Build (LUZ)'
status: seedling
tags:
- maven
- artifactory
- versioning
- hotfix
- release-process
- confluence-distilled
title: Released artifact versions are immutable, so hotfixes iterate as snapshots
type: lesson
---

# Released artifact versions are immutable, so hotfixes iterate as snapshots

A well-configured artifact repository **refuses to overwrite a released version**. That single rule reshapes the hotfix process, and teams discover it mid-incident if they have not planned for it.

The workaround adopted when a new Maven artifactory enforced it:

> We have to release a hotfix for `luz-compensation 0.1.0.0` → publish **`0.1.0.1-SNAPSHOT`** instead of the release version `0.1.0.1` as before. **Reason: the new artifactory server doesn't allow overriding the release version.**
>
> When the hotfix is OK: **remove `-SNAPSHOT` (manually)**, then **create the tag.**

So the fix iterates as a snapshot — which *can* be republished repeatedly — and is promoted to an immutable release only once it is confirmed good. The same flow applies to application modules and platform modules alike.

**Why the repository is right to refuse.** If `0.1.0.1` can be overwritten, then "we deployed 0.1.0.1" stops identifying a specific artifact. Two environments can run different bytes under the same version, a cached build and a fresh one disagree, and no rollback is trustworthy because the thing you roll back to may have changed. Immutability is what makes a version a fact.

**The cost is that hotfixes need somewhere mutable to iterate**, and `-SNAPSHOT` is that place. Snapshot semantics are the inverse of release semantics — republishable, resolved freshly, not to be trusted as an identity.

> [!warning] The manual promotion step is the weak point
> *"Remove -SNAPSHOT (manually)"* followed by *"create TAG"* is two hand-performed steps under incident pressure, in the wrong order to be safe: the artifact is promoted before the tag exists, so a mistake leaves a released version with no commit to trace it to. Worth scripting — cut the tag first, build the release from the tag, and never hand-edit a version during an incident.

> [!tip] Never deploy a snapshot to production, even briefly
> The temptation during a hotfix is to ship the `-SNAPSHOT` straight to prod "just to check". Then production is running an artifact that can be silently replaced, and you have lost the very property the repository was protecting. Promote first, deploy second.

Source: [[Discussion GCP Release Process with Google Cloud Build|Discussion  GCP Release Process with Google Cloud Build]] (LUZ, Confluence).

%% ai-graph-start %%

**Related notes:**
- [[Confirm the deployed artifact contains the fix before judging an env test]]
- [[Discussion GCP Release Process with Google Cloud Build]]
- [[Cloud Run won't redeploy on a latest digest change — apply by immutable digest]]
- [[Cloud Run latest does not roll a new revision on terraform apply — deploy by digest]]
- [[Cloud Build $COMMIT_SHA is the full 40-char git SHA, not the short one]]

%% ai-graph-end %%