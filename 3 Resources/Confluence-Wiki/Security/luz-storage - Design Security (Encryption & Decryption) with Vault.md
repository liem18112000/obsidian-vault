---
ai_hash: 78453c81ba758431
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 3
entities: []
relevance: 0.931
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48245801024/luz-storage+-+Design+Security+Encryption+Decryption+with+Vault
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: '[luz-storage] - Design: Security (Encryption & Decryption) with Vault'
topic: security
type: source
updated: 2025-05-06
---

# [luz-storage] - Design: Security (Encryption & Decryption) with Vault

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-05-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48245801024/luz-storage+-+Design+Security+Encryption+Decryption+with+Vault)
> Relevance 0.931 · topic `security`

<div>

|  |  |  |
|----|----|----|
| **Upload file** | **Download file** | **Delete file** |
| 

![[48245801024-image-20250110-112450.png]]

 | 

![[48245801024-image-20250110-112515.png]]

 | *luz-storage send a DELETE request to GCS, Vault doesn't involve in this operation* |

</div>

%% ai-graph-start %%

**Related notes:**
- [[luz-storage - DEPRECATED - Encryption & Decryption]]
- [[Encryption and decryption flows with Vault]]
- [[Introduction of Hashicorp Vault]]
- [[Security]]
- [[Envelope encryption with Vault transit keeps Vault off the data path]]

%% ai-graph-end %%