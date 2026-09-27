---
title: "email_engine renders with plain regex substitution, not Jinja, so AI-authored templates can't execute code"
created: 2026-09-21
type: argument
status: seedling
source: "session 2026-09-21 (send-time render)"
tags: [email, templating, security, jinja, ssti, customer360, rendering]
---

# email_engine renders with plain regex substitution, not Jinja, so AI-authored templates can't execute code

The send-time email renderer (`backend-system/email_engine/email_engine/rendering.py`) does per-recipient personalization with a **fixed regex substitution** over a known context dict — `{{ key }}` (spaces optional) via `_PLACEHOLDER_PATTERN.sub`, NOT Jinja or any template engine.

**Why it matters (security design decision):** email/message templates in this system can be authored by the AI agent or by marketers. A full template engine would let an authored template execute arbitrary expressions/code. Plain substitution makes that impossible by construction. Two more safety properties fall out: unknown placeholders render to `""` (never leak raw `{{ x }}` markup to a recipient), and HTML-body merge values are `html.escape`d (a profile field containing markup can't inject into the email HTML — subject and text_body stay unescaped plain text).

Context keys available to a template: `first_name, last_name, name, email, unsubscribe_url` (see `send.py::_render_for_recipient`). After substitution: links are rewritten through the click-tracking redirect (each destination HMAC-signed with `EMAIL_TRACKING_SECRET` so the endpoint rejects a swapped URL — prevents an open redirect), and a 1×1 open-tracking pixel is injected before `</body>`.

General principle: when templates are authored by an LLM or by untrusted-ish users, prefer whitelist substitution over a Turing-complete template engine.

## Related

- [[email-channel-is-outbound-template]]
