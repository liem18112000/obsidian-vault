---
title: "Carve /.well-known out of any catch-all proxy or certificate validation fails"
created: 2026-09-27
type: gotcha
status: seedling
source: "Confluence: SSL renewal issue (LUZ)"
tags: [tls, ssl, nginx, ingress, acme, cert-manager, confluence-distilled]
---

# Carve /.well-known out of any catch-all proxy or certificate validation fails

If your ingress proxies everything for a custom domain to another origin, it proxies the **domain-validation path too** — and certificate issuance or renewal fails for reasons that look random ("some SSL can be validated while some others cannot").

The fix visible in a working ingress: an explicit location that short-circuits the validation path *before* the catch-all proxy.

```yaml
annotations:
  kubernetes.io/ingress.class: webclient-nginx-ingress
  nginx.org/server-snippets: |-
    location / { proxy_pass https://binh-14-08.online-dev.klara.tech/; }
    location ~ /.well-known/pki-validation/2DB4742365CB7337A9F11F10BD603BD1.txt {
      return 200 "A6DF91145B1B048DC93D45B4473D8ABD1BF11420E226821892CD38C0861C3161 comodoca.com";
    }
    if ($host = www.team-hacka.biz) {
      return 301 $scheme://team-hacka.biz$request_uri;
    }
```

**Why it fails without this.** The CA fetches `http://<domain>/.well-known/pki-validation/<token>.txt` and expects your content. With only `location /`, that request is forwarded to the upstream origin, which knows nothing about the token and returns its own 404 or homepage. Validation fails — while a domain whose ingress happens *not* to proxy the root validates fine. Same config file, different outcome, no obvious cause.

**Two more things in that snippet worth noticing:**

- **The `www` → apex redirect is a `301`.** If the CA validates the `www` variant and you permanently redirect it away, validation for that name can fail too. Know which exact names are on the certificate and make sure each is reachable.
- **The token is hardcoded in the ingress.** That works, and it does not renew. Next renewal issues a *new* token and this annotation is stale — which is precisely how "it worked last year" becomes an expired certificate.

> [!tip] The general rule
> **`/.well-known/` is control-plane traffic, not application traffic.** Carve it out ahead of any catch-all proxy, redirect, or auth middleware. The same trap catches ACME `http-01` (`/.well-known/acme-challenge/`), and it is also why an auth gate on `/` breaks certificate renewal — the CA cannot log in.

> [!warning] Prefer automated issuance over hand-placed tokens
> A hardcoded token is a one-shot manual step that expires silently. `cert-manager` with an ACME solver handles the challenge path and the renewal together, so there is no annotation to go stale. If you must do it manually, put a calendar reminder on the certificate's expiry — the config gives you no warning.

Source: [[SSL renewal issue]] (LUZ, Confluence).
