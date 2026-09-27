---
ai_hash: 5fd306071b50f113
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- greennode
- vngcloud
- iam
- service-account
- credentials
title: Get GreenNode API credentials by creating an IAM Service Account (Client ID
  + Secret Key)
type: howto
---

# Get GreenNode API credentials by creating an IAM Service Account (Client ID + Secret Key)

The `client_id`/`client_secret` used to authenticate to GreenNode/VNG Cloud APIs (Terraform provider, OAuth2 `client_credentials`) is an **IAM Service Account** credential, NOT a human login.

**Create it:**
1. IAM console -> https://iam.console.greennode.ai/service-accounts (sign in as Root user or a user with IAM access).
2. Left menu **Service account** -> **Create service account**.
3. Name + optional description.
4. **Attach policies** — give it the permissions the automation needs (e.g. vDB + vServer/network read for subnet lookups; or an admin policy).
5. **Create service account** -> the **Client ID** and **Secret Key (Client Secret)** are shown.

**Gotcha:** the Secret Key is displayed **only once** at creation — copy it immediately. If lost, regenerate from the account's **Security credentials** tab (this invalidates the old secret). Manage attached policies later via the account's **Permission** tab -> **Attach Policies**.

Maps to `TF_VAR_client_id`/`TF_VAR_client_secret` (or the provider's native `CLIENT_ID`/`CLIENT_SECRET`). See [[Provision GreenNode/VNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]] and [[GreenNode Cloud is VNG Cloud rebranded (same IAM, gateway, Terraform provider)]].

## Related

- [[Provision GreenNode/VNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]
- [[GreenNode Cloud is VNG Cloud rebranded (same IAM]]
- [[gateway]]
- [[Terraform provider)]]

%% ai-graph-start %%

**Related notes:**
- [[GreenNode Cloud is VNG Cloud rebranded (same IAM, gateway, Terraform provider)]]
- [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]
- [[vStorage S3 keys differ from vIAM client credentials used by vDB]]
- [[VNG Cloud Terraform provider maps managed Postgres and Redis to vdb resources]]
- [[VNG Cloud list-projects endpoint is GET vserver-gatewayv1projects (not accounts-api)]]

%% ai-graph-end %%