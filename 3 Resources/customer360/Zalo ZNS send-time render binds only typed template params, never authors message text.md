---
title: "Zalo ZNS send-time render binds only typed template params, never authors message text"
created: 2026-09-21
type: concept
status: seedling
source: "session 2026-09-21 (ZNS render)"
tags: [zalo, zns, rendering, customer360, notification-engine, templating]
---

# Zalo ZNS send-time render binds only typed template params, never authors message text

Zalo ZNS send-time rendering (`backend-system/notification_engine/notification_engine/rendering.py`) is fundamentally different from email rendering, because ZNS content is FIXED by the OA-approved template — you may only fill its typed parameters, never author message text.

`render_params(template_data, context)` walks each param value and, if it is a string, binds `{{merge}}` tokens (`first_name, last_name, name, phone`) from the per-recipient context; non-string values pass through untouched. An unresolved token is left as-is (`m.group(0)`) — NOT blanked (contrast the email renderer, which blanks unknowns). The output is a `template_data` dict sent to the Zalo OA together with the OA `zalo_template_id` (from the crm_message_templates row's `metadata.zalo_template_id`); the OA renders its approved layout from those params.

No HTML escaping, no link rewriting, no open pixel — those are email-only concerns. Eligibility gate at send: skip recipients with no phone or `zalo_opt_in=false` (`send_zalo_campaign`). The dispatch ledger reuses the email columns — `recipient_email` actually holds the PHONE for Zalo.

Contrast with the email path: email authors full subject/html/text and renders via `email_engine.rendering` (see [[email_engine renders with plain regex substitution, not Jinja, so AI-authored templates cant execute code]]); ZNS only fills a fixed template's slots. Both feed off the same crm_campaign draft the agent produced ([[customer360 AI campaign lifecycle: agent plans, api persists draft, email_engine renders at send]]).

## Related

- [[customer360 AI campaign lifecycle: agent plans]]
- [[api persists draft]]
- [[email_engine renders at send]]
