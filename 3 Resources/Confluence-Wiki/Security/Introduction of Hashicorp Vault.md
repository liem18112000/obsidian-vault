---
ai_hash: a0bda490bb0ec71a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 15
depth: 2.95
entities: []
relevance: 0.802
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530431677/Introduction+of+Hashicorp+Vault
space: LUZ
status: reference
tags:
- confluence
- security
- space/luz
title: Introduction of Hashicorp Vault
topic: security
type: source
updated: 2021-05-10
---

# Introduction of Hashicorp Vault

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-05-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530431677/Introduction+of+Hashicorp+Vault)
> Relevance 0.802 · topic `security`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="8181c6ee-6848-4c5b-9681-94486bebaf91" macro-name="toc">

</div>

Secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data using a UI, CLI, or HTTP API.

1.  # Setup Vault

    1.  ## Create Vault Image (<a href="https://bitbucket.org/axonivy-prod/luz_dockerfiles/src/master/luz-vault/" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_dockerfiles/src/master/luz-vault/</a>)

        \- create config file (<a href="https://bitbucket.org/axonivy-prod/luz_dockerfiles/src/master/luz-vault/config.hcl" class="external-link" rel="nofollow">config.hcl</a>)  
        

![[20530431677-image2021-5-10_10-43-14.png]]

  
        - Create policy to define client permission for whole vault app and policy for luz-doc. The file is <a href="https://bitbucket.org/axonivy-prod/luz_dockerfiles/src/master/luz-vault/transit-policy.hcl" class="external-link" rel="nofollow">transit-policy.hcl</a>  
        

![[20530431677-image2021-5-10_11-5-54.png]]

  
        - Add username and password for a user that can get client token in docker-entrypoint.sh file  
        

![[20530431677-image2021-5-10_11-53-35.png]]

  
          

    2.  ## Add this image to GCP for deployment

2.  # Usage APIs

    1.  ## **Get client token.** 

        Base on the username and password already add in the above config. We call call the API to get client token.

        

![[20530431677-image2021-5-10_12-7-48.png]]



    2.  ## **Generate Data Key** 

        Base on the client token, now we will call API to get data key from specific name.

        

![[20530431677-image2021-5-5_14-49-58.png]]



        

![[20530431677-image2021-5-10_12-11-32.png]]

  
          

    3.  ## **Encrypt Data** 

        **

![[20530431677-image2021-5-5_14-53-49.png]]

![[20530431677-image2021-5-10_12-17-16.png]]

  **

    4.  ## **Decrypt Data 

![[20530431677-image2021-5-5_14-54-35.png]]

 

![[20530431677-image2021-5-10_12-19-28.png]]

**

        **  **

3.  Related Document  
    [Encryption and decryption flows with Vault#Flowtoconfigurekeyandpolicy](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20525655052/Encryption+and+decryption+flows+with+Vault#EncryptionanddecryptionflowswithVault-Flowtoconfigurekeyandpolicy)

%% ai-graph-start %%

**Related notes:**
- [[Vault overview]]
- [[Luz-vault]]
- [[luz-vault - How to run Vault Benchmark]]
- [[luz-vault - Recovery key encryption with RSA Public Keys (draft - vault operator]]
- [[Encryption and decryption flows with Vault]]

%% ai-graph-end %%