---
ai_hash: 4873909a3c152bb9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-22
entities:
- CRLF
- tfvars user_data heredoc
- Terraform
- VNG vServers
- ./deploy-all.sh uat plan
- leo-customer360 server module
- terraform plan
- vServers
- user_data
- deployments/server/overlays/uat.tfvars
- Windows checkout
- Terraform state
- LF
- vngcloud provider
- running containers
- volumes
- floating IPs
- server/deploy.sh uat apply
- deploy-all.sh uat apply
- --skip server
- lifecycle { ignore_changes = [user_data] }
- vngcloud_vserver_server.this
- git .gitattributes
- '*.tfvars text eol=lf'
- Windows editors
- Terraform string attribute
- force-new attribute
- cloud-init
- scripts
- ignore_changes
- vStorage S3 backend creds
- component .env
- deploy scripts
- Dangerous latent drift
- replacement landmine
- FIRST boot
- spurious replacements
source: session 2026-08-22
status: seedling
tags:
- leo-customer360
- terraform
- vngcloud
- crlf
- drift
- gotcha
- user_data
title: CRLF in a tfvars user_data heredoc makes Terraform force-replace VNG vServers
type: lesson
---

# CRLF in a tfvars user_data heredoc makes Terraform force-replace VNG vServers

**Dangerous latent drift** found by `./deploy-all.sh uat plan` on the leo-customer360 `server` module.

## Symptom
`terraform plan` on the server module shows `2 to add, 2 to destroy` — both vServers (`this["api"]`, `this["1x2"]`) **must be replaced** because `~ user_data ... # forces replacement`. But in the diff, **every `-` (old) line equals its `+` (new) line byte-for-byte** — the content is identical.

## Cause
`deployments/server/overlays/uat.tfvars` (which holds the `user_data` cloud-init heredoc) has **CRLF** line terminators on the Windows checkout, while the resource's stored `user_data` in Terraform state has **LF**. Terraform compares the two strings, sees \r\n vs \n on every line -> treats `user_data` as changed. The vngcloud provider marks `user_data` as force-new, so it plans to **destroy + recreate both boxes** — which would wipe every running container (api, redis, keycloak, jaeger, …), volumes, and possibly reassign floating IPs — all for a no-op.

## Do NOT apply
Never run `server/deploy.sh uat apply` (or `deploy-all.sh uat apply` without `--skip server`) while this diff stands.

## Fixes (pick one)
1. **Best / durable:** add `lifecycle { ignore_changes = [user_data] }` to `vngcloud_vserver_server.this` in the server module. user_data only matters at FIRST boot; ignoring later drift stops spurious replacements permanently (survives future CRLF churn).
2. Normalize the overlay to **LF** (git `.gitattributes`: `*.tfvars text eol=lf`, then renormalize) so it matches state — but Windows editors can reintroduce CRLF.
3. Operationally: always `deploy-all.sh uat apply --skip server` (server is rarely re-applied).

General lesson: on Windows, any Terraform string attribute that is **force-new** (user_data, cloud-init, scripts) is a replacement landmine if its source file is CRLF and state is LF. Pin `eol=lf` for such files and/or `ignore_changes`.

## Related
[[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]

## Related

- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]

%% ai-graph-start %%

**Related notes:**
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]
- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]
- [[vngcloud Terraform accepts root_disk_size change but does not resize the boot volume in-place]]
- [[Renaming a Terraform for_eachmap key needs terraform state mv or it destroys+recreates]]

**Relations:**
- CRLF — *present in* — tfvars user_data heredoc
- CRLF — *causes* — Terraform
- Terraform — *to force-replace* — VNG vServers
- Dangerous latent drift — *found by* — ./deploy-all.sh uat plan
- Dangerous latent drift — *affects* — leo-customer360 server module
- terraform plan — *on* — leo-customer360 server module
- terraform plan — *shows* — replacement of vServers
- vServers — *are replaced due to* — user_data
- user_data — *content is* — identical
- deployments/server/overlays/uat.tfvars — *contains* — user_data
- deployments/server/overlays/uat.tfvars — *has* — CRLF
- CRLF — *on* — Windows checkout
- Terraform state — *stores* — user_data
- user_data — *with* — LF
- Terraform — *compares* — CRLF
- Terraform — *compares* — LF
- Terraform — *detects* — user_data
- user_data — *as changed* — true
- vngcloud provider — *marks* — user_data
- user_data — *as* — force-new attribute
- force-new attribute — *leads to* — destroy + recreate
- destroy + recreate — *would wipe* — running containers
- destroy + recreate — *would wipe* — volumes
- destroy + recreate — *would reassign* — floating IPs
- server/deploy.sh uat apply — *should not be run* — true
- deploy-all.sh uat apply — *should be run with* — --skip server
- Fix 1 — *is* — lifecycle { ignore_changes = [user_data] }
- lifecycle { ignore_changes = [user_data] } — *applied to* — vngcloud_vserver_server.this
- user_data — *matters at* — FIRST boot
- ignoring later drift — *prevents* — spurious replacements
- Fix 2 — *is* — Normalize overlay to LF
- Normalize overlay to LF — *using* — git .gitattributes
- git .gitattributes — *with setting* — *.tfvars text eol=lf
- Windows editors — *can reintroduce* — CRLF
- Fix 3 — *is* — always deploy-all.sh uat apply --skip server
- General lesson — *applies to* — Windows
- General lesson — *applies to* — Terraform string attribute
- Terraform string attribute — *marked as* — force-new attribute
- Terraform string attribute — *examples include* — user_data
- Terraform string attribute — *examples include* — cloud-init
- Terraform string attribute — *examples include* — scripts
- Terraform string attribute — *is a* — replacement landmine
- replacement landmine — *if source file is* — CRLF
- replacement landmine — *if state is* — LF
- Pin eol=lf — *for* — such files
- Pin ignore_changes — *for* — such files
- Related — *to* — Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth

%% ai-graph-end %%