---
ai_hash: fcdfb063c2ca6f3a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18 leo-customer360 deployments/server
status: seedling
tags:
- vngcloud
- vserver
- security-group
- ssh
- gotcha
title: VNG Default secgroup opens nothing inbound; SSH times out until you add a tcp/22
  secgrouprule
type: lesson
---

# VNG Default secgroup opens nothing inbound; SSH times out until you add a tcp/22 secgrouprule

On GreenNode/VNG Cloud, the project **"Default" security group opens NOTHING inbound** — a fresh vServer attached only to it is unreachable: SSH to its floating IP **times out** (filtered, not "refused"). The provider does NOT auto-open SSH. Fix in Terraform with `vngcloud_vserver_secgrouprule` (direction="ingress", ethertype="IPv4", protocol="TCP", port_range_min/max=22, remote_ip_prefix=<cidr>, security_group_id=<the attached secgroup>). Note the arg is `security_group_id` (rule) vs `security_group` (list on the server resource). Also: the floating (public) IP is exported on the server under `internal_interfaces[].floating_ip` (e.g. 49.213.x), NOT `external_interfaces` (which came back empty) — a script auto-discovering the public IP must read internal_interfaces[].floating_ip.

## Related
[[VNG vServer user_data is mutually exclusive with username/password/ssh_key]]

%% ai-graph-start %%

**Related notes:**
- [[GreenNode vDB public_access is non-functional here; reach a private DB only via an in-VPC bastion (native login, not user_data)]]
- [[GreenNode VNG Ubuntu 24.04 image SSH is broken out-of-the-box; the cloud-init recipe to fix it]]
- [[VNG Cloud vServer SSH keys must be RSA, not ed25519 (Invalid public key at apply)]]
- [[VNG vServer user_data is mutually exclusive with usernamepasswordssh_key]]
- [[VNG vServer apply-time gotchas password policy ( @ !) and AZ-restricted volume types (1C needs NVME)]]

%% ai-graph-end %%