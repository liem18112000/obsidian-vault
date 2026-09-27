---
ai_hash: 6d169df2c3c705c7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: leo-customer360 beta.leocdp.com ops ports, 2026-08
status: seedling
tags:
- hsts
- tls
- browser
- reverse-proxy
- ops
- gotcha
title: HSTS on a parent domain makes all plain-HTTP and self-signed ports on a subdomain
  unreachable in-browser
type: gotcha
---

# HSTS on a parent domain makes all plain-HTTP and self-signed ports on a subdomain unreachable in-browser

If a parent domain (e.g. leocdp.com) is HSTS-preloaded or sends `Strict-Transport-Security: ...; includeSubDomains`, then EVERY subdomain (beta.leocdp.com) is HSTS-enforced in the browser regardless of what that subdomains own server sends. Effects, per-HOST (HSTS ignores port):
- The browser force-upgrades any `http://host:PORT` to `https://host:PORT`. So a plain-HTTP service on a non-443 port (Dagster :3000, Netdata :19999) becomes an https request against a plaintext listener -> TLS handshake fails / "not connected".
- A self-signed / untrusted cert on any port (Portainer :9443) becomes a HARD failure with NO "Proceed anyway" bypass ("you cannot visit ... because the website uses HSTS", ERR_CERT_AUTHORITY_INVALID / net::ERR_SSL...).
The valid-LE-cert :443 front door is unaffected.

**This defeats the usual "just use http:// on the port" advice** — under HSTS the browser upgrades it. Workarounds to reach raw-port ops tools:
1. Use the raw IP instead of the HSTS hostname: `http://<LB-IP>:3000` etc. (IPs cannot be HSTS hosts). Verified reachable.
2. SSH tunnel to localhost: `ssh -L 3000:localhost:3000 -L 9443:localhost:9443 user@box` then `http://localhost:3000` / click-through self-signed on localhost.
3. Proper fix: front every tool through the trusted-TLS reverse proxy (subdomain or path on :443) so nothing rides raw HTTP/self-signed.

Diagnosis tips: `curl -sI https://sub/` shows no `strict-transport-security` yet the browser still enforces it -> suspect parent-domain preload/includeSubDomains, not the subdomains own headers. curl does NOT honor HSTS, so curl to `http://sub:port` will "work" even when Chrome refuses — dont use curl to disprove an HSTS report.

Related: [[Stripping a path prefix at the proxy breaks framework auto-redirects; forward it un-stripped when the app has root_path]]

%% ai-graph-start %%

**Related notes:**
- [[Serve a no-auth UI over TLS+SSO behind Caddy at a subpath (oauth2-proxy proxy-prefix)]]
- [[Portainer CSRF origin-invalid behind a reverse proxy - expose it directly]]
- [[Exposure model for ops dashboards behind an L4 (OIDC-incapable) load balancer]]
- [[Serve a SPA under a sub-path via the app base-path option, not proxy strip]]
- [[Dagster --path-prefix moves the GraphQL endpoint too, breaking clients that hardcode graphql]]

%% ai-graph-end %%