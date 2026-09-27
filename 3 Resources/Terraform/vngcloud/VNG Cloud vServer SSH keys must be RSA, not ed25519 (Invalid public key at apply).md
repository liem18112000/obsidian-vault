---
ai_hash: 9378d70064509ef6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18 leo-customer360 deployments/server
status: seedling
tags:
- vngcloud
- vserver
- ssh
- rsa
- gotcha
title: VNG Cloud vServer SSH keys must be RSA, not ed25519 (Invalid public key at
  apply)
type: lesson
---

# VNG Cloud vServer SSH keys must be RSA, not ed25519 (Invalid public key at apply)

GreenNode/VNG Cloud vServer rejects **ed25519** SSH public keys — creating `vngcloud_vserver_sshkey` with an `ssh-ed25519 ...` key fails at apply with `Status Code: 400, {"message":"Invalid public key"}`. Fix: use an **RSA** key (`ssh-keygen -t rsa -b 4096`), i.e. an `ssh-rsa AAAA...` public key. The provider/plan does not catch this — it only surfaces at apply when the API validates the key. (Same likely applies to other key types like ecdsa; RSA is the safe choice on VNG.)

## Related
[[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

## Related

- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vServer user_data is mutually exclusive with usernamepasswordssh_key]]
- [[GreenNode VNG Ubuntu 24.04 image SSH is broken out-of-the-box; the cloud-init recipe to fix it]]
- [[VNG Default secgroup opens nothing inbound; SSH times out until you add a tcp22 secgrouprule]]
- [[VNG vServer apply-time gotchas password policy ( @ !) and AZ-restricted volume types (1C needs NVME)]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

%% ai-graph-end %%