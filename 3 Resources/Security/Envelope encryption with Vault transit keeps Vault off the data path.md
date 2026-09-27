---
title: "Envelope encryption with Vault transit keeps Vault off the data path"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Encryption and decryption flows with Vault (LUZ)"
tags: [security, encryption, vault, envelope-encryption, minio, s3, confluence-distilled]
---

# Envelope encryption with Vault transit keeps Vault off the data path

Encrypting object-storage files with Vault does not mean sending the file to Vault. Vault's **transit** engine hands out a one-use data key; your service encrypts the bytes locally and stores the *encrypted* key next to the ciphertext. This is envelope encryption, and it keeps Vault off the data path entirely.

**The upload flow:**

1. Authenticate to Vault; receive a client token with the attached policy.
2. Ask the transit engine for a data key:
   ```
   transit/datakey/plaintext/<key_ring>
   ```
   It returns **two** representations of the same key — `plaintext` (the DEK) and `ciphertext` (the eDEK, the DEK sealed under the key ring's master key).
3. Encrypt the file locally with the DEK → eData (AES).
4. Store eData in MinIO/S3 under a **random UUID filename**, with the eDEK in object metadata:
   ```
   x-amz-meta-edex:     <eDEK string>
   x-amz-meta-filepath: bucketName/realFileName
   ```
5. Discard the plaintext DEK.

Download reverses it: read the eDEK from metadata, ask Vault to decrypt it back to the DEK, decrypt the bytes locally.

**Why this shape rather than "let Vault encrypt it":**

- **Vault never sees the file.** Only a 32-byte key crosses the wire, so throughput is bounded by your service, not by Vault — large files do not become a Vault scaling problem.
- **The master key never leaves Vault.** Compromising the object store yields ciphertext plus a *sealed* key; without Vault the eDEK is inert.
- **Key rotation is cheap.** Rotating the key ring re-wraps eDEKs; it does not require re-encrypting terabytes of objects.
- **Per-object keys limit blast radius.** One leaked DEK exposes one object.

> [!warning] Two things to verify before committing to this
> **Does your object store actually preserve custom metadata** through every path you use — multipart upload, server-side copy, lifecycle transitions? The design note flags "Checking if Metadata Support is given from MinIO" as an open question, and it is the right question: if metadata is dropped on a copy, the eDEK is gone and the object is permanently unreadable.
> **Where does the filename mapping live?** Naming objects by random UUID is good for confidentiality, but it means the real path exists only in that metadata (`x-amz-meta-filepath`) or in your database. That mapping is now as critical as the key.

> [!note] Deletes skip Vault
> Deleting is a plain `DELETE` to the object store — no key material involved, so Vault is not in that path at all.

Source: [[Encryption and decryption flows with Vault]] and [[luz-storage - Design Security (Encryption & Decryption) with Vault]] (LUZ, Confluence).

## Related

- [[Encryption and decryption flows with Vault]]
