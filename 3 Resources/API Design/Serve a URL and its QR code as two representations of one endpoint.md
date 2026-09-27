---
title: "Serve a URL and its QR code as two representations of one endpoint"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: QR code URL implementation flow (LUZ)"
tags: [rest, content-negotiation, http-accept, qr-code, tokens, confluence-distilled]
---

# Serve a URL and its QR code as two representations of one endpoint

A URL and a QR code encoding that URL are **the same resource in two representations**, not two resources. So they belong behind one endpoint, selected by the `Accept` header, rather than at `/link` and `/qr`.

The design:

| Request `Accept:` | Response |
|---|---|
| `text/html` | the URL |
| `image/png` | the QR code image |

Same path, same request body, same generated token — the caller states what form it can consume, and the server renders accordingly.

**Why this beats two endpoints.** Everything except the final encoding is shared: subscription check, request-body validation, token generation, expiry. Two endpoints means duplicating that or extracting it anyway, and it means two things to keep in sync when the token payload changes. It is also what `Accept` is *for* — content negotiation is the part of HTTP most often reimplemented as a URL suffix or a `?format=` parameter.

**A related parameter worth noting:** `encrypt-token` lets the sender encrypt the token embedded in the returned URL or QR code, *for cases where it carries sensitive user credentials* used to match a recipient. Making encryption an **explicit opt-in per request** is a defensible design — but see the warning.

> [!warning] A QR code is a URL anyone can photograph
> The token travels inside an image that gets printed on letters, shown on screens, and forwarded. Treat it as public: short expiry, single use where possible, and minimal claims. If some payloads carry credentials and need `encrypt-token`, ask whether the safe default should be *encrypt always* — an opt-in security control is one someone will forget to set. Same argument as [[Keycloak action tokens bridge an app session into a browser login]].

> [!tip] Document the representations in the spec, not just in prose
> OpenAPI expresses multiple response media types for one operation directly. Putting `text/html` and `image/png` in the spec means generated clients and docs know both exist — otherwise the QR variant is folklore that only the team who built it knows about.

> [!note] Track what is actually implemented
> The source table marks rows **green = implemented, red = not implemented yet** against each accepted request-body shape. Annotating a design table with implementation status is a small thing that stops a spec being read as a description of reality.

Source: [[QR code URL implementation flow]] (LUZ, Confluence).

## Related

- [[Keycloak action tokens bridge an app session into a browser login]]
