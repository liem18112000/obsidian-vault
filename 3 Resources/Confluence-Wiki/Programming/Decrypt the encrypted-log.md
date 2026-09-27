---
ai_hash: 4fd6d72ceb2f18b5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/47123367788/Decrypt+the+encrypted-log
space: NEXT
status: reference
tags:
- confluence
- programming
- space/next
title: Decrypt the encrypted-log
topic: programming
type: source
updated: 2023-04-12
---

# Decrypt the encrypted-log

> [!info] Imported from Confluence
> Space **NEXT** · updated 2023-04-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/47123367788/Decrypt+the+encrypted-log)
> Relevance 0.724 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="28713bbb-9f6b-4845-b7f5-24dbe42a2db3" macro-name="toc">

</div>

**This page is to introduce how to decrypt the generated log with key** 

##### Step 1: Create java compile class file

***javac luz_finnova\src\main\java\com\axonivy\finnova\function\DecryptFunctionCaller.java***

result:


![[47123367788-image-20220608-095945.png]]



##### Step 2: Call class with input parameter

***java luz_finnova\src\main\java\com\axonivy\finnova\function\DecryptFunctionCaller.java {{encrypted_log}} {{key_to_decrypt}}***

**Result :**


![[47123367788-image-20220608-100620.png]]

%% ai-graph-start %%

**Related notes:**
- [[luz-storage - DEPRECATED - Encryption & Decryption]]
- [[Encryption and decryption flows with Vault]]
- [[How to unseal Vault Unseal]]

%% ai-graph-end %%