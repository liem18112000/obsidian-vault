---
title: "Route user-edited business rules through git and CI instead of a database"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: CICD for Kogito (LUZ)"
tags: [gitops, business-rules, kogito, audit, ci-cd, confluence-distilled]
---

# Route user-edited business rules through git and CI instead of a database

Business rules edited by non-developers usually live in a database and take effect immediately. An alternative: **the rule editor commits to git**, and the change travels the same CI/CD path as code.

The loop, for a Kogito rules service:

1. A user edits a rule in the UI.
2. **The UI pushes the change to the `luz-kogito-rule` repository** — the branch depends on the target environment.
3. The push **triggers Cloud Build**, which builds a new image.
4. The build **updates the image hash in the infrastructure repository** (`luz_kubernetes`) — again, branch and YAML file per environment.
5. The deployment picks up the new image.

**What you gain by routing rules through git:**

- **Every rule change is a commit** — author, timestamp, diff, and the ability to revert. For rules with financial or legal effect, that audit trail is often the requirement, not a nice-to-have.
- **Rules get environments.** Branch-per-environment means a rule is exercised in dev and test before production, which database-backed rules almost never are.
- **Rollback is a deploy**, using the same mechanism as application rollback, rather than a manual data fix.
- **One promotion path.** Nobody has to reason about whether rules and code are in sync, because they move together.

**What you pay:**

- **Latency.** A rule change is now a build and a deploy — minutes, not seconds. If the business expects to tune a threshold and see it live immediately, this is the wrong model.
- **A commit-capable editor.** The UI needs repository credentials and has to produce clean, reviewable diffs. Machine-generated commits that churn formatting destroy the audit value you built this for.
- **Two repositories to keep aligned** — the rules repo and the infra repo whose image hash it updates. That is the same seam described in [[Two IaC surfaces need an explicit naming contract at the seam]].

> [!tip] The deciding question is who is accountable for a rule change
> If a wrong rule means a wrong invoice, a wrong payment, or a compliance breach, you want the commit, the review and the revert. If rules are presentational or low-stakes and change hourly, the build latency is not worth it.

> [!warning] Branch-per-environment drifts unless promotion is copying, not re-editing
> If a user "edits the rule in prod" by pushing to the prod branch directly, environments diverge and the tested artefact is not the deployed one. Promotion must move a *reviewed change* forward, never re-author it per branch.

Source: [[CICD for Kogito]] (LUZ, Confluence).

## Related

- [[Two IaC surfaces need an explicit naming contract at the seam]]
