---
ai_hash: 6f267e4feef33f01
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 2.4
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47586476158/luz-storage+-+DEPRECATED+-+Encryption+Decryption
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: '[luz-storage] - DEPRECATED - Encryption & Decryption'
topic: security
type: source
updated: 2024-09-16
---

# [luz-storage] - DEPRECATED - Encryption & Decryption

> [!info] Imported from Confluence
> Space **LUZ** · updated 2024-09-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47586476158/luz-storage+-+DEPRECATED+-+Encryption+Decryption)
> Relevance 0.731 · topic `security`

"Luz-storage" provides impressive storage speed and security for long-term file storage. It **encrypts** user files and stores them on Google Cloud Storage.

## I. Encryption and Decryption Algorithm.

### a. Files

<div>

|  |  |
|----|----|
| **Name** | `AES/CBC/PKCS5Padding` |
| **Algorithm** | `AES` |
| **Mode** | `CBC` |
| **Reference** | <a href="https://www.baeldung.com/java-aes-encryption-decryption#2-file" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.baeldung.com/java-aes-encryption-decryption#2-file</a> |

</div>

### b. Generate Secret Key

<div>

|  |  |
|----|----|
| **Name** | `PBKDF2WithHmacSHA256` |
| **Algorithm** | `PBKDF2` |
| **Mode** | `HmacSHA256` |
| **Salt** | luz-storage |
| **Key length** | 256 |
| **Iteration** | 65536 |
| **Reference** | <a href="https://www.baeldung.com/java-aes-encryption-decryption#2-secret-key" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.baeldung.com/java-aes-encryption-decryption#2-secret-key</a> |

</div>

## II. Encryption key.

<div hasbody="true" macro-id="fb80a214-4e08-4e25-a770-a3edb2815875" macro-name="tip">

<span class="aui-icon aui-icon-small aui-iconfont-approve confluence-information-macro-icon"> </span>

<div>

Key is generated **randomly**!

The key will be encrypted and stored in luz-keyvaluestore.

<a href="https://www.baeldung.com/java-aes-encryption-decryption#2-secret-key" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.baeldung.com/java-aes-encryption-decryption#2-secret-key</a>

</div>

</div>

## III. Diagram.

### a. Uploading the file.


![[47586476158-File Encryption Sequence Diagram.png]]



<div>

|  |  |  |
|----|----|----|
|  | **Step** | **Note** |
| 1 | Upload a file | User send the request to upload a file |
| 2 | Get the encrypted key | luz-storage retrieves the encrypted key in luz-keyvaluestore |
| 3 | Return result | luz-keyvaluestore response. |
| 4 | Generate a key and encrypt it | In case of the key is not existing on luz-keyvalustore then luz-storage will generate a new key and encrypt it. |
| 5 | Store the encrypted key | luz-storage store the encrypted key to luz-keyvaluestore. |
| 6 | Return result | luz-keyvaluestore response. |
| 7 | Decrypt the key. | In case of the key is existing on luz-keyvaluestore then luz-storage will decrypt it. |
| 8 | Encrypt the bytes of file | Luz-storage read certain bytes from the files and encrypt them. |
| 9 | Upload the encrypted bytes | luz-storage uploads the encrypted bytes to GCS |
| 10 | Return the file-path | After finish streaming upload the file to GCS, luz-storage return the file-path of the file to END-USER |

</div>

### b. Getting the file.


![[47586476158-Decryption File Sequence Diagram.png]]



<div>

|  |  |  |
|----|----|----|
|  | **Step** | **Note** |
| 1 | Get a file | User send the request to get a file |
| 2 | Get the encrypted key | luz-storage calls to luz-keyvaluestore to get the encypted key. |
| 3 | Return result | luz-keyvaluestore response. |
| 4 | Return status code 500 | In case of the encrypted key is not yet stored in luz-keyvaluestore then luz-storage will return status code 500 to END-USER |
| 5 | Decrypt the key | luz-storage decrypts the key from luz-keyvaluestore. |
| 6 | Download the bytes of file | luz-storage streamly downloads a certain bytes of file from GCS. |
| 7 | Return the bytes | GCS returns the encrypted bytes |
| 8 | Decrypt the bytes | luz-storage decrypts these bytes from GCS |
| 9 | Return the decrypted bytes | luz-storage supports streaming downloading from GCS to END-USER. The bytes will sequencely send to END-USER. |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Encryption and decryption flows with Vault]]
- [[luz-storage - Design Security (Encryption & Decryption) with Vault]]
- [[Introduction of Hashicorp Vault]]
- [[Luz-vault]]
- [[MessageV2 Field Encryption Approach]]

%% ai-graph-end %%