---
ai_hash: d8590443c1a52fe0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18 leo-customer360 deployments/server
status: seedling
tags:
- vngcloud
- vserver
- cloud-init
- user-data
- gotcha
title: VNG vServer user_data is mutually exclusive with username/password/ssh_key
type: lesson
---

# VNG vServer user_data is mutually exclusive with username/password/ssh_key

On GreenNode/VNG Cloud vServer, the `vngcloud_vserver_server` `user_data` (cloud-init) field is **mutually exclusive** with `user_name` + `user_password` + `ssh_key`. Supplying user_data alongside any of them fails at apply: `Status Code: 400, {"message":"User data dont allow input username, password and ssh key."}`. Choose ONE path: (a) SSH-key / password login via user_name/user_password/ssh_key and NO user_data, or (b) a cloud-init user_data script that sets up its own users/keys and leave user_name/user_password/ssh_key unset. For a jump host, path (a) is simpler — install extra packages (e.g. postgresql-client) manually after first SSH. The provider/plan does not catch this; it only surfaces at apply.

## Related
[[VNG Cloud vServer SSH keys must be RSA, not ed25519 (Invalid public key at apply)]]

## Related

- [[VNG Cloud vServer SSH keys must be RSA, not ed25519 (Invalid public key at apply)]]

%% ai-graph-start %%

**Related notes:**
- [[VNG Cloud vServer SSH keys must be RSA, not ed25519 (Invalid public key at apply)]]
- [[VNG vServer apply-time gotchas password policy ( @ !) and AZ-restricted volume types (1C needs NVME)]]
- [[GreenNode VNG Ubuntu 24.04 image SSH is broken out-of-the-box; the cloud-init recipe to fix it]]
- [[GreenNode vDB public_access is non-functional here; reach a private DB only via an in-VPC bastion (native login, not user_data)]]
- [[VNG Default secgroup opens nothing inbound; SSH times out until you add a tcp22 secgrouprule]]

%% ai-graph-end %%