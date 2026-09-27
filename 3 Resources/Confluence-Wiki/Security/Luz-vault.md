---
ai_hash: 9c07912869531a64
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.5
entities: []
relevance: 0.701
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49065361421/Luz-vault
space: TK
status: reference
tags:
- confluence
- security
- space/tk
title: Luz-vault
topic: security
type: source
updated: 2026-01-21
---

# Luz-vault

> [!info] Imported from Confluence
> Space **TK** · updated 2026-01-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49065361421/Luz-vault)
> Relevance 0.701 · topic `security`

# Luz-vault

## 1. Definition & Purpose

**Domain:** SECURITY / DOCUMENT MANAGEMENT INFRASTRUCTURE

**luz-vault** is the central HashiCorp Vault cluster used by ePost/Klara services to:

- Securely store **secrets and credentials** (e.g. tenant DB passwords).

- Manage **encryption keys** for documents and files:

  - Per‑tenant **KEK** (Key Encryption Key).

  - Per‑document **DEK/eDEK** (Data Encryption Key / encrypted DEK).

It runs on **GCP GKE** (current), uses **PostgreSQL** (`luz-database`) as storage backend, and is automatically unsealed via a dedicated **luz-vault-unseal** Vault instance.

**Key references:**

- HC Vault insights (core architecture & flows):  
  <https://axonivy.atlassian.net/wiki/spaces/TeamInvisible/pages/47645196391/KLARA+HC+Vault+insights>

------------------------------------------------------------------------

## 2. Core Responsibilities

### 2.1 Secrets & Credentials (KV Engine)

- Stores **tenant‑specific credentials**, most importantly:

  - MongoDB passwords for **luz-jsonstore** tenants.

- Accessed via **KV secrets engine** paths like `kv/<tenant_id>`.

### 2.2 Encryption & Key Management (Transit Engine)

- Provides **client‑side encryption** capabilities:

  - Create **KEK** per tenant: `transit/keys/<tenant_id>`

  - Create **DEK/eDEK** per document: `transit/datakey/plaintext/<tenant_id>`

  - Encrypt / decrypt data:

    - `transit/encrypt/<tenant_id>`

    - `transit/decrypt/<tenant_id>`

- Used heavily by:

  - **luz-docs** for document encryption.

  - **luz-audit** for audit payload encryption.

  - **luz-storage** for file content encryption.

### 2.3 Authentication & Tokens

- Uses **JWT auth** (role `klara-tenant-role`):

  - Apps call `/v1/auth/jwt/login` with a tenant access token.

  - Vault validates JWT against **jwt-service**/Keycloak public keys.

  - Returns a **client token** bound to the transit/kv policies.

- All secrets & crypto operations are performed with this **leased token**, which:

  - Has a lease duration (e.g. 10h).

  - Is revocable and renewable.

------------------------------------------------------------------------

## 3. High‑Level Architecture

### 3.1 Components

<div>

|  |  |
|----|----|
| Component | Description |
| **luz-vault** | Main HashiCorp Vault instance. Provides KV & Transit engines and JWT auth. Uses PostgreSQL (`luzvault` DB) for storage. |
| **luz-vault-unseal** | Secondary Vault used **only** to auto‑unseal `luz-vault` via Transit. Unsealed using K8s secret `luz-vault-keyvalue-unseal-secret`. |
| **luz-database** | PostgreSQL 9.5 DB storing Vault data (`vault_kv_store`, `vault_kv_autounseal`, `vault_ha_locks`). |
| **jwt-service** | Provides public keys and issues JWT tokens used for Vault authentication. |
| **GKE (luz-vault StatefulSet)** | Vault runs as StatefulSet for stable network IDs and easier clustering. |

</div>

------------------------------------------------------------------------

## 4. Services Using luz-vault

**Core clients:**

1.  **luz-docs**

    - Uses **Transit engine** to:

      - Create tenant KEK

      - Generate per‑document DEK/eDEK.

      - Encrypt/decrypt data keys for documents stored in GCS.

2.  **luz-jsonstore**

    - Uses **KV engine** to store **MongoDB passwords per tenant** encrypted in Vault.

    - Reads tenant credentials on startup or when needed.

3.  **luz-audit**

    - Uses **Transit engine** to:

      - Encrypt audit log data.

      - Manage KEKs/DEKs for audit payloads.

4.  **luz-storage**

    - Uses **luz-vault** as key manager for **encrypted GCS file storage**.

    - Create tenant KEK

    - Generate per‑document DEK/eDEK.

    - Encrypt/decrypt data keys for documents stored in GCS.

------------------------------------------------------------------------

## 5. Diagram – System Context

<div id="expander-329246203" class="expand-container conf-macro output-block" hasbody="true" macro-id="e35a9f46-a43f-4d6e-8ceb-ad53e1a6680c" macro-name="expand">

<div id="expander-control-329246203" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Mermaid Diagram</span>

</div>

<div id="expander-content-329246203" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9dd2c9ce-b2b9-4b3f-a141-bf725a952612" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
flowchart LR
    subgraph VAULT["Vault Platform"]
        LV["luz-vault<br/>(HashiCorp Vault core)"]
        LVU["luz-vault-unseal<br/>(Auto-unseal Vault)"]
        DB["luz-database<br/>(PostgreSQL: vault_kv_store, vault_kv_autounseal)"]
        JWT["jwt-service / Keycloak<br/>(JWT & public keys)"]
    end

    subgraph SERVICES["LUZ Services using Vault"]
        DOCS["luz-docs<br/>(Document service)"]
        JSON["luz-jsonstore<br/>(Tenant & doc metadata)"]
        AUDIT["luz-audit<br/>(Audit logs)"]
        STORAGE["luz-storage<br/>(Binary file storage)"]
        STAT["luz-docs-statistic<br/>(Document statistics)"]
    end

    %% Vault core relations
    LVU -->|"Transit auto‑unseal<br/>mTLS, autounseal key"| LV
    LV -->|"storage backend"| DB
    LV -->|"JWT auth, token validation"| JWT

    %% Service usages
    DOCS -->|"Transit: KEK/DEK/eDEK<br/>encrypt/decrypt documents"| LV
    JSON -->|"KV: tenant DB passwords"| LV
    AUDIT -->|"Transit: encrypt audit payloads<br/>+ KV passwords"| LV
    STORAGE -->|"Transit: file encryption keys"| LV

    %% Indirect dependency through jsonstore
    STAT -->|"read/write statistics<br/>via luz-jsonstore"| JSON
    JSON -->|"auth via Vault"| LV
```

</div>

</div>

</div>

</div>


![[49065361421-Vault-client.png]]



------------------------------------------------------------------------

## 6. Typical Flows

### 6.1 Authentication (JWT → Client Token)

1.  Client (e.g. `luz-docs`) obtains a **tenant access token** from LUZ JWT/Keycloak.

2.  Client calls Vault:

    - `POST /v1/auth/jwt/login`

    - Payload contains `jwt: <access_token>`, `role: "klara-tenant-role"`.

3.  Vault:

    - Validates JWT signature and issuer using public keys from jwt-service/Keycloak.

    - Checks the configured role mapping.

    - Returns a **client token** with attached `transit-policy`, etc.

4.  Client caches the **Vault client token** and uses it until expiry or revocation.

### 6.2 Key Lifecycle (KEK / DEK / eDEK)

For a new tenant or document:

1.  **Create KEK/eKEK** for tenant (once):  
    `POST /v1/logical/transit/keys/<tenant_id>`

2.  **Create DEK/eDEK** per document:  
    `POST /v1/logical/transit/datakey/plaintext/<tenant_id>`  
    → returns `plaintext` (DEK) + `ciphertext` (eDEK).

3.  Client:

    - Stores **eDEK** alongside document metadata or in storage (e.g. GCS).

    - Uses **DEK** to encrypt the actual data, then discards DEK.

4.  To read later:

    - Client sends **eDEK** to Vault: `POST /v1/logical/transit/decrypt/<tenant_id>`

    - Vault returns the plaintext **DEK**, which client uses to decrypt the document/file.

%% ai-graph-start %%

**Related notes:**
- [[Vault overview]]
- [[Introduction of Hashicorp Vault]]
- [[MessageV2 Field Encryption Approach]]
- [[Luz-audit]]
- [[Security]]

%% ai-graph-end %%