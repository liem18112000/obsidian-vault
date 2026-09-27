---
title: "[luz-storage] - Design: Security (Encryption & Decryption) with Vault"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48245801024/luz-storage+-+Design+Security+Encryption+Decryption+with+Vault
space: "LUZ"
topic: security
relevance: 0.931
depth: 3
updated: 2025-05-06
attachments: 4
tags:
  - confluence
  - security
  - space/luz
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
