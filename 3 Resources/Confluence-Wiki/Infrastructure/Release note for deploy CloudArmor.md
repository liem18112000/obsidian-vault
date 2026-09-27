---
ai_hash: 080ad9c7e64da388
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.85
entities: []
relevance: 0.734
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49437605898/Release+note+for+deploy+CloudArmor
space: TS
status: reference
tags:
- confluence
- infra
- space/ts
title: Release note for deploy CloudArmor
topic: infra
type: source
updated: 2026-08-12
---

# Release note for deploy CloudArmor

> [!info] Imported from Confluence
> Space **TS** · updated 2026-08-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49437605898/Release+note+for+deploy+CloudArmor)
> Relevance 0.734 · topic `infra`

### Release Note – Rules Deployment (Updated Tool)

This section documents the updated deployment process for rules using the `deploy_cloud_armor_rulesets_regional.sh` script with the new `import`, `validate`, `plan`, and `apply` commands.

#### Deployment Flow

The deployment follows a strict sequential flow. Each step must be completed before proceeding to the next.

**Step 1: Validate**

Run `validate` to check the rules for syntax or configuration errors before planning or applying.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="acd9ba01-976c-4e6c-b4f8-839df9b51593" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh <gcp_project> <environment> validate
```

</div>

</div>

**Note:** ***ask Gabe if you have doubts about the script:*** deploy_cloud_armor_rulesets_regional.sh

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8734be6e-9224-45b0-8df7-a056d63dafe0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh gcp-dev-vn dev-vn validate
```

</div>

</div>

Fix any validation errors before proceeding.

**Step 2: Plan** — Preview changes

Run `plan` to review which rules will be **added**, **changed**, or **destroyed**. This step is **required** before running `apply`.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="95d65592-4802-4db0-9394-23459d8573f6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh <gcp_project> <environment> plan
```

</div>

</div>

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a076bf57-21c6-49ae-b2f7-4a6861092979" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh gcp-dev-vn dev-vn plan
```

</div>

</div>

Sample output:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="65b18ebd-2421-4ede-9c53-e17f6540ce3f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
❯ sh ./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh gcp-nonprod dev plan
Cluster config: [gcp-nonprod], env: [dev], command: [plan].

Exported security policy to [./backup/dev-security-policy-regional_latest.bak.yaml].

Successfully create policy backup at ./backup/dev-security-policy-regional_latest.bak.yaml
Successfully load expression set at ./backup/dev-expression-set_latest.bak.yaml
Remove undeployed rules [].

Cloud Armor execution plan
Target project: klara-nonprod
Target region: europe-west6
Target policy: dev-security-policy-regional
Ruleset file: /c/work/projects/luz_kebernetes/cluster/gcp/cloud_armor/dev-rulesets-regional.yaml

Plan: 0 to add, 0 to change, 0 to destroy.
Note: 'change' means at least one managed field differs from the exported GCP rule.
No changes.
Plan only. No changes applied.
```

</div>

</div>

> **Review the plan output carefully before proceeding:**
>
> - ✅ Only new rules to add (e.g., `X to add, 0 to change, 0 to destroy`) — Safe to proceed directly to `apply`.
>
> - ⚠️Rules to change — Review the changes to ensure they are expected. If the number of changes is unexpectedly high, re-run `import` (**Step 4 - Optional**) first to sync the current GCP state, then update your YAML (**Step 5 - Optional**) and run `plan` again.
>
> - 💣Rules to destroy — Do not apply immediately. Carefully verify that the rules marked for deletion are not legitimate existing rules in GCP. Re-run `import` (**Step 4 - Optional**) to ensure the local state is up to date, then update (**Step 5 - Optional**) review your YAML config before running `plan` again.

**Step 3: Apply** — Deploy to GCP

Run `apply` to deploy the rules to GCP. This requires appropriate **GCP IAM permissions** and must be preceded by a successful `plan`. Only proceed with `apply` if the plan shows only additions (no unexpected changes or deletions), or if you have carefully reviewed and confirmed that all changes and deletions are intentional.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="407488b8-8684-41bf-a640-ace2dbce15c1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh <gcp_project> <environment> apply
```

</div>

</div>

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9b52c67c-8338-44ed-9f4d-cf679a747419" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh gcp-dev-vn dev-vn apply
```

</div>

</div>

#### Release Commands (per environment)

For each environment, run the full sequence:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="211ec078-39c7-4217-9471-95c1b6c3be37" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh <gcp_project> <environment> import
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh <gcp_project> <environment> validate
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh <gcp_project> <environment> plan
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh <gcp_project> <environment> apply
```

</div>

</div>

#### Environment Rollout Order

Follow the standard release flow — apply changes progressively:

1.  **dev / dev-vn** — Deploy, then verify with tests (e.g., Grafana k6)

2.  **dev-staging** — Merge changes to master branch, then apply

3.  **performance** — Apply and validate under load

4.  **prod** — Final production deployment

<div>

|             |                                                              |
|-------------|--------------------------------------------------------------|
| Environment | YAML Config File                                             |
| dev         | `cluster/gcp/cloud_armor/dev-rulesets-regional.yaml`         |
| dev-vn      | `cluster/gcp/cloud_armor/dev-vn-rulesets-regional.yaml`      |
| dev-staging | `cluster/gcp/cloud_armor/dev-staging-rulesets-regional.yaml` |
| test        | `cluster/gcp/cloud_armor/test-rulesets-regional.yaml`        |
| performance | `cluster/gcp/cloud_armor/performance-rulesets-regional.yaml` |
| prod        | `cluster/gcp/cloud_armor/prod-rulesets-regional.yaml`        |

</div>

------------------------------------------------------------------------

**Step 4 - Optional: Import** — Sync current GCP state to local YAML

Run `import` to synchronize the current configuration from GCP into the local YAML config file. This ensures your local configuration reflects the actual state in GCP before making any changes.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d6d2b733-835d-44a7-a5ff-7f23e59ccd6b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh <gcp_project> <environment> import
```

</div>

</div>

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f2bad277-42c3-4329-8e9c-11c24bb0183c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./cluster/gcp/cloud_armor/deploy_cloud_armor_rulesets_regional.sh gcp-dev-vn dev-vn import
```

</div>

</div>

> ⚠️ **Always run** `import` first before updating rules. This prevents accidental removal of existing rules already deployed in GCP.

**Step 5 - Optional: Update YAML config files**

After import, update the YAML ruleset config files with the new rules. Replace the content of the YAML files from import and ensure they are correct for each target environment.

###  Example: The new rate limit rules apply to luz-epost webclient v2

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9eacc98e-7609-40ca-86c4-47cb2aa7f6e5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# ==============================================================
# ePost WebClient V2 (luz-next) — Rate Limiting Rules
# Host: ^client(?:-[a-z0-9]+)*\.(?:klara-epost|epost)\.(?:tech|ch)$
# Priorities: 21550–21568
# ==============================================================
- description: Rate based ban - ePost WebClient V2 | luz-next Auth credential endpoints
  preview: false
  priority: 21560
  expression: has(request.headers['Host']) && request.headers['Host'].lower().matches('client(?:-[a-z0-9]+)*\\.(?:klara-epost|epost)\\.(?:tech|ch)') && request.path.matches('/(?:api/auth)') && !request.path.matches('/(?:api/auth/(?:callback|session|_log|csrf|providers))')
  action: rate-based-ban
  exceed-action: deny-403
  rate-limit-threshold-count: 120
  rate-limit-threshold-interval-sec: 60
  ban-duration-sec: 900
  enforce-on-key-configs: ip
- description: Rate based ban - ePost WebClient V2 | luz-next Auth session polling endpoints
  preview: false
  priority: 21561
  expression: has(request.headers['Host']) && request.headers['Host'].lower().matches('client(?:-[a-z0-9]+)*\\.(?:klara-epost|epost)\\.(?:tech|ch)') && request.path.matches('/(?:api/auth/(?:session|_log|csrf|providers))')
  action: rate-based-ban
  exceed-action: deny-429
  rate-limit-threshold-count: 600
  rate-limit-threshold-interval-sec: 60
  ban-duration-sec: 300
  enforce-on-key-configs: ip
- description: Rate based ban - ePost WebClient V2 | luz-next Server Actions
  preview: false
  priority: 21562
  expression: has(request.headers['Host']) && request.headers['Host'].lower().matches('client(?:-[a-z0-9]+)*\\.(?:klara-epost|epost)\\.(?:tech|ch)') && request.method == 'POST' && has(request.headers['next-action'])
  action: rate-based-ban
  exceed-action: deny-429
  rate-limit-threshold-count: 300
  rate-limit-threshold-interval-sec: 60
  ban-duration-sec: 900
  enforce-on-key-configs: ip
- description: Rate based ban - ePost WebClient V2 | luz-next API endpoints (smartletter, unified-inbox, fetch-eml)
  preview: false
  priority: 21563
  expression: has(request.headers['Host']) && request.headers['Host'].lower().matches('client(?:-[a-z0-9]+)*\\.(?:klara-epost|epost)\\.(?:tech|ch)') && request.path.matches('/(?:api/(?:smart-?letter|unified-inbox|fetch-eml))')
  action: rate-based-ban
  exceed-action: deny-429
  rate-limit-threshold-count: 300
  rate-limit-threshold-interval-sec: 60
  ban-duration-sec: 900
  enforce-on-key-configs: ip
- description: Rate based ban - ePost WebClient V2 | luz-next Internal APIs
  preview: false
  priority: 21565
  expression: has(request.headers['Host']) && request.headers['Host'].lower().matches('client(?:-[a-z0-9]+)*\\.(?:klara-epost|epost)\\.(?:tech|ch)') && request.path.matches('/(?:api/internal)')
  action: rate-based-ban
  exceed-action: deny-429
  rate-limit-threshold-count: 60
  rate-limit-threshold-interval-sec: 60
  ban-duration-sec: 1800
  enforce-on-key-configs: ip
- description: Rate based ban - ePost WebClient V2 | luz-next API fallback (unclassified routes)
  preview: false
  priority: 21567
  expression: has(request.headers['Host']) && request.headers['Host'].lower().matches('client(?:-[a-z0-9]+)*\\.(?:klara-epost|epost)\\.(?:tech|ch)') && request.path.startsWith('/api/')
  action: rate-based-ban
  exceed-action: deny-429
  rate-limit-threshold-count: 600
  rate-limit-threshold-interval-sec: 60
  ban-duration-sec: 600
  enforce-on-key-configs: ip
- description: Rate based ban - ePost WebClient V2 | luz-next UI page routes (low risk)
  preview: false
  priority: 21568
  expression: has(request.headers['Host']) && request.headers['Host'].lower().matches('client(?:-[a-z0-9]+)*\\.(?:klara-epost|epost)\\.(?:tech|ch)') && request.path.matches('/(?:en|de|fr|it)/') && request.method == 'GET'
  action: rate-based-ban
  exceed-action: deny-429
  rate-limit-threshold-count: 1200
  rate-limit-threshold-interval-sec: 60
  ban-duration-sec: 300
  enforce-on-key-configs: ip
# ==============================================================
```

</div>

</div>

#### Key Reminders

- **Permissions:** Ensure you have the required GCP IAM permissions before running `apply`.

- **Import first:** Always run `import` before making changes to avoid overwriting existing GCP rules.

- **Validate before plan:** Use `validate` to catch rule errors early.

- **Plan before apply:** Never run `apply` without reviewing the `plan` output first.

- **Add-only plans are safe:** If the plan only shows new rules to add (0 to change, 0 to destroy), you can proceed to `apply` directly.

- **Changes require review:** If the plan shows rules to change, verify the changes are expected. A high number of unexpected changes usually means you need to re-run `import` first.

- **Deletions require extra caution:** If the plan indicates rules will be destroyed, do not apply. Double-check they are not legitimate existing rules in GCP. Re-run `import` if necessary, then update your YAML and re-plan.

- **Testing:** After deploying to dev/dev-vn, run tests (e.g., Grafana k6) to verify the rules behave as expected before promoting to higher environments.

------------------------------------------------------------------------

%% ai-graph-start %%

**Related notes:**
- [[Apply changes on luz_kubernetes]]
- [[Discussion GCP Release Process with Google Cloud Build]]
- [[Recipe Deploy with Terraform]]
- [[Validate your k8s yaml]]
- [[Kubernetes knowledge]]

%% ai-graph-end %%