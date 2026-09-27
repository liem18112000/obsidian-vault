---
title: "Update ePost certificate - *.epost.ch - in PROD"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/IO/pages/48526819332/Update+ePost+certificate+-+.epost.ch+-+in+PROD
space: "IO"
topic: security
relevance: 0.852
depth: 3
updated: 2026-06-04
attachments: 8
tags:
  - confluence
  - security
  - space/io
---

# Update ePost certificate - *.epost.ch - in PROD

> [!info] Imported from Confluence
> Space **IO** · updated 2026-06-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48526819332/Update+ePost+certificate+-+.epost.ch+-+in+PROD)
> Relevance 0.852 · topic `security`

# PROD

## MFT

<a href="https://mft-admin.epost.ch:8443" class="external-link" rel="nofollow">https://mft-admin.epost.ch:8443</a>

Login credentials → 1password

Use PEM cert + key, that means combine linux_cert+ca\_.epost.ch.txt + tls.key (latter export it from luz_kubernetes, epost tls secret)

1.  Import certificate in vault

2.  Update certificate for the HTTPS “service”


![[48526819332-image-20250604-133238.png]]



1.  Import cert


![[48526819332-image-20250604-133328.png]]



Click Import (not Add Certificate)


![[48526819332-image-20250604-133428.png]]

![[48526819332-image-20260604-005039.png]]



2.  Update certificate

For mft.epost.ch


![[48526819332-image-20250604-133813.png]]



For <a href="http://mft.admin.epost.ch" class="external-link" rel="nofollow">mft.admin.epost.ch</a>


![[48526819332-image-20250606-213104.png]]



for <a href="http://mft-admin.epost.ch" class="external-link" rel="nofollow">mft-admin.epost.ch</a> the server needs to be restarted: [Everything about Smartsend and Hub](https://axonivy.atlassian.net/wiki/spaces/IO/pages/48146743320/Everything+about+Smartsend+and+Hub)

## Invaris PROD


![[48526819332-image-20250604-133929.png]]



## luz_kubernetes

Update ./kubernetes-overlays/env-prod/ingress/luz-ingress-epost-tls.sops.yaml

## prod-secmail / <a href="http://smtpsapi.epost.ch" class="external-link" rel="nofollow">smtpsapi.epost.ch</a>

generate above secret also here ~/luz_kubernetes/kubernetes-overlays/env-prod-secmail/gateway

## Robin double check SSL pinning

openssl x509 -in 2025/linux_cert+ca\_.epost.ch.txt -pubkey -noout  
-----BEGIN PUBLIC KEY-----  
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAv2KKf0ffjZXqCmdWSZlO  
9eCiz/l/oDedInv1Q/qLRl2++nJUNLHnrk5PiUdlLXWO+AE9oH2bG79MOz2gris/  
ivGlMc3n6/p+mR0ppuAG0Sm7PNHW9K5wl2zd0HBtUS8qR+m2h+h5iByMRybpBbwL  
ctseAHUjCiAetqeHbwKFEFYQgcMkbfh/zTLaU9s8RY+ut7cyFpuudtelYkuGTd4V  
XfNSzCiqnJG+9oHHPik49K77tWosbIj+ICCUHihDFuCBB3uW1hlZnxrdeq1YMex6  
SjoBEOGPbdYbvSAj+xVcfkM7HGFB7OibFJyAoM5Nj+5nza94jQHXFpv2qxKFMKP+  
BwIDAQAB  
-----END PUBLIC KEY-----

openssl x509 -in 2026/linux_cert+ca\_.epost.ch.txt -pubkey -noout  
-----BEGIN PUBLIC KEY-----  
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAv2KKf0ffjZXqCmdWSZlO  
9eCiz/l/oDedInv1Q/qLRl2++nJUNLHnrk5PiUdlLXWO+AE9oH2bG79MOz2gris/  
ivGlMc3n6/p+mR0ppuAG0Sm7PNHW9K5wl2zd0HBtUS8qR+m2h+h5iByMRybpBbwL  
ctseAHUjCiAetqeHbwKFEFYQgcMkbfh/zTLaU9s8RY+ut7cyFpuudtelYkuGTd4V  
XfNSzCiqnJG+9oHHPik49K77tWosbIj+ICCUHihDFuCBB3uW1hlZnxrdeq1YMex6  
SjoBEOGPbdYbvSAj+xVcfkM7HGFB7OibFJyAoM5Nj+5nza94jQHXFpv2qxKFMKP+  
BwIDAQAB  
-----END PUBLIC KEY-----
