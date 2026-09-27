---
title: "MessageV2 Field Encryption Approach"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49671405621/MessageV2+Field+Encryption+Approach
space: "LUZ"
topic: security
relevance: 0.75
depth: 2.6
updated: 2026-08-20
attachments: 0
tags:
  - confluence
  - security
  - space/luz
---

# MessageV2 Field Encryption Approach

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-08-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49671405621/MessageV2+Field+Encryption+Approach)
> Relevance 0.75 · topic `security`

<div hasbody="true" macro-id="78a7e4aa-5f1e-4f7c-b605-82ea0e38f3fb" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**Status:** Implemented in `luz_epc` using Vault Transit datakeys plus local AES-256-GCM with a cached tenant DEK. CSFLE is **NO-GO**. The map entry is `{ path, enabled }` only.

</div>

</div>

**Story:** <a href="https://axonivy.atlassian.net/browse/LUZ-156508" class="external-link" rel="nofollow">LUZ-156508</a> · **Epic:** <a href="https://axonivy.atlassian.net/browse/LUZ-156499" class="external-link" rel="nofollow">LUZ-156499</a>  
**Code:** `luz_epc/api/src/api/field-encryption/`  
**Repo spec:** `luz_epc/docs/specs/LUZ-156508-field-encryption-approach.md`  
**FigJam:** <a href="https://www.figma.com/board/Zis5FIIS3GgkSqe17Nyy82" class="external-link" rel="nofollow">LUZ-156508 Field Encryption Workflows</a>

# 1. Workflow (repo boundary)

Crypto is not applied to raw V1. Repositories call `MessageFieldEncryptionGate`.

## Write: `createMany` → `prepareV2ForPersist`

1.  Convert V1 → MessageV2.

2.  If `path` is empty, copy from `unhashedCredentials.<pathLeaf>` (for example, leaf `socialSecurityNumber`).

3.  Redact that credential key from `original` and legacy snapshots.

4.  Encrypt enabled `path` values; numbers and booleans are coerced to text.

5.  Persist the envelope in `messagesV2`.

## Update: `updateMany` / `bulkWrite` → `prepareV2UpdateForPersist`

Before Mongo `$set`, redact mapped credential keys from snapshots and encrypt mapped plaintext on `path`, coercing numbers as needed. `flattenRecipientForMongoSet` avoids wiping the SSN. Operations fail closed without `sender.tenantId`.

## Fetch / read: `prepareV1ForReturn`

1.  Fetch MessageV2.

2.  Decrypt mapped envelopes, including disabled entries with leftover ciphertext.

3.  Convert V2 → V1 and copy plaintext onto `unhashedCredentials.<pathLeaf>`.

4.  The API returns plaintext; Compass shows the envelope.

# 2. Encryption and decryption

Vault issues a tenant DEK; EPC encrypts and decrypts fields locally with AES-256-GCM.

1.  Ensure `transit/keys/{tenantId}` exists through lazy creation.

2.  Call `POST /v1/transit/datakey/plaintext/{tenantId}` with `bits: 256`, once per tenant until LRU eviction.

3.  Use AES-256-GCM with that DEK.

4.  Store an envelope containing `ciphertext` (`dek:v1:...`) and `wrappedKey` (`vault:v1:...`).

<div>

|  |  |
|----|----|
| Envelope / decrypt path | Behavior |
| `dek:v1:` + `wrappedKey` | Use the cached DEK if it matches; otherwise unwrap through Transit, then decrypt locally with AES. |
| Legacy `vault:v1:` | Decrypt the field ciphertext through Transit. |

</div>

## Vault authentication and cache

**Auth:** JWT-service company-tenant JWT → `POST /v1/auth/jwt/login` using `klara-tenant-role` / `klara-generic-role` → `X-Vault-Token`.

- One session per `sender.tenantId`; maximum `FIELD_ENCRYPTION_CACHE_MAX` entries (default 1000).

- Refresh tokens near lease expiry or on HTTP 403; refresh is not sliding.

- Keep the DEK across token refresh; drop it on LRU eviction.

- Serialize same-tenant concurrent DEK mint and unwrap operations.

- Never log DEK bytes and never encrypt `sender.tenantId`.

# 3. EncryptedFieldsMap

Default field: `recipient.address.socialSecurityNumber`.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d4a1bda9-733a-44f8-914c-6b0a3fbd4c14" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{ "path": "recipient.address.socialSecurityNumber", "enabled": true }
```

</div>

</div>

Credential copy, redaction, and restore use the path leaf: `socialSecurityNumber` → `unhashedCredentials.socialSecurityNumber`. There is no `sourcePath`, `targetPath`, or `plaintextPattern`.

**Override order:** `ENCRYPTED_FIELDS_MAP_FILE` → `ENCRYPTED_FIELDS_MAP` → `encrypted-fields.defaults.ts`.

- **Add:** add `{ path, enabled: true }`; the field is not available for Atlas or text search, and the roll API must be migrated.

- **Remove:** set `enabled: false`, backfill, then delete the entry after all envelopes are gone.

# 4. Deploy (`luz_kubernetes`) — JWKS-style

<div>

|  |  |
|----|----|
| Piece | Value |
| Overlay file | `luz-epc-api/encrypted-fields.map.json` |
| ConfigMap | `luz-epc-api-encrypted-fields-map` |
| Mount | `/config/encrypted-fields.map.json` |
| Env | `ENCRYPTED_FIELDS_MAP_FILE=/config/encrypted-fields.map.json` |
| Overlays | `env-dev`, `env-dev-vn`, `env-dev-staging`, `env-test`, `env-performance`, `env-prod` |

</div>

Also configure `FIELD_ENCRYPTION_ENABLED=true`, `FIELD_ENCRYPTION_PROVIDER=vault`, `LUZ_VAULT_BASE_URL=http://luz-vault:8200`, and the JWT roles. For a map-only change, edit the JSON, deploy the overlay, and roll `luz-epc-api`; no image rebuild is required.

# 5. Configuration

<div>

|  |  |
|----|----|
| Variable | Purpose |
| `FIELD_ENCRYPTION_ENABLED` | Default true. If false while plaintext is present, fail closed. |
| `FIELD_ENCRYPTION_PROVIDER` | `vault` or `local`; production does not silently fall back to local. |
| `LUZ_VAULT_*` / JWT admin | Vault authentication. |
| `ENCRYPTED_FIELDS_MAP_FILE` | JSON path; takes precedence. |
| `ENCRYPTED_FIELDS_MAP` | Inline JSON map. |
| `FIELD_ENCRYPTION_CACHE_MAX` | LRU size; default 1000. |
| `FIELD_ENCRYPTION_LOCAL_SECRET` | Required when production explicitly uses local encryption. |

</div>

# 6. Security

- Fail closed on create and update for mapped fields.

- Require `sender.tenantId`.

- Redact snapshots on create and update / merge-metadata.

- Never log plaintext or DEK bytes.

- Block encrypted paths from Atlas and text search.

- Production without a Vault URL fails startup.

# 7. Decision history

<div>

|                                        |                 |
|----------------------------------------|-----------------|
| Option                                 | Outcome         |
| MongoDB CSFLE                          | **NO-GO**       |
| Vault Transit + tenant DEK + local AES | **Implemented** |
| SSN-only hard-coded crypto             | **Rejected**    |

</div>

# 8. Open items

- Vault SLOs under concurrent datakey, login, and unwrap operations.

- Monitoring V2 may still show envelopes.

- Migration window for plaintext and legacy envelopes.

- Production rollout checklist.

# References

- <a href="https://axonivy.atlassian.net/browse/LUZ-156508" class="external-link" rel="nofollow">LUZ-156508</a>

- <a href="https://axonivy.atlassian.net/browse/LUZ-156499" class="external-link" rel="nofollow">LUZ-156499</a>

- Code: `luz_epc/api/src/api/field-encryption/`

- Spec: `luz_epc/docs/specs/LUZ-156508-field-encryption-approach.md`

- K8s: `luz_kubernetes` + overlay `encrypted-fields.map.json`

- FigJam: <a href="https://www.figma.com/board/Zis5FIIS3GgkSqe17Nyy82" class="external-link" rel="nofollow">workflows</a>
