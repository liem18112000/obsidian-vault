---
title: "mTLS (mutual TLS, 2 way TLS)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496026069/mTLS+mutual+TLS+2+way+TLS
space: "LUZ"
topic: security
relevance: 0.716
depth: 2.42
updated: 2019-09-18
attachments: 7
tags:
  - confluence
  - security
  - space/luz
---

# mTLS (mutual TLS, 2 way TLS)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2019-09-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496026069/mTLS+mutual+TLS+2+way+TLS)
> Relevance 0.716 · topic `security`

SSL vs TLS

- both cryptographic protocols, ensure data encryption between comm. communication partners
- SSL : Secure Socket Layer
- TLS : Transport Layer Security
- Difference : SSL 1, 2, 3 ... are old and deprecated, TLS 1.2 is current standard, NOT DOWNWARD compatible !

  

Browser - Server

- Show cert chain in browser (Apple, GLKB)


![[20496026069-image2019-9-17_9-4-14.png]]



  

Server - Server (Java → Truststore is cacerts)


![[20496026069-image2019-9-17_9-15-47.png]]



  


![[20496026069-image2019-9-17_9-5-45.png]]



  

mutual TLS, 2-way TLS


![[20496026069-image2019-9-17_9-8-42.png]]



  

GLKB - mTLS on token service

[Finnova - Oauth 2.0](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20468880369/Finnova+-+Oauth+2.0)

  


![[20496026069-image2019-9-17_9-22-26.png]]



  

Installation

- Tools : keytool, Keystore Explorer

<!-- -->

- Guide : [Hotfix - 0.01.52.01 (11.09.2019)](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496024137/Hotfix+-+0.01.52.01+11.09.2019)

<!-- -->

- Do it on Klara-Staging

  

How to debug

- standalone.conf
- JAVA_OPTS="\$JAVA_OPTS -<a href="http://Djavax.net" class="external-link" rel="nofollow">Djavax.net</a>.debug=all"

  

Important : <a href="https://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html" class="external-link" rel="nofollow">https://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html</a>
