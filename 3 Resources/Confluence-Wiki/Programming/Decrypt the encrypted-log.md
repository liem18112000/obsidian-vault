---
title: "Decrypt the encrypted-log"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/47123367788/Decrypt+the+encrypted-log
space: "NEXT"
topic: programming
relevance: 0.724
depth: 2.81
updated: 2023-04-12
attachments: 6
tags:
  - confluence
  - programming
  - space/next
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
