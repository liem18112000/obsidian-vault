---
title: "CodeQL py/clear-text-logging-sensitive-data: don't log phone/PII, log a correlation id"
created: 2026-09-19
type: lesson
status: seedling
source: "leo-customer360 PR 77 CI fix 2026-09-19"
tags: [codeql, security, logging, pii, ci, gotcha]
---

# CodeQL py/clear-text-logging-sensitive-data: don't log phone/PII, log a correlation id

CodeQL's `py/clear-text-logging-sensitive-data` query flags any value it considers sensitive (by name/taint — e.g. a variable called `phone`, or a password/token) reaching a logging sink. It fails the CodeQL check on a PR and posts an inline bot comment per hit.

## Fix that actually clears it
Do NOT pass the sensitive value to the logger at all. Slicing/masking (e.g. `phone[-4:]`) often does NOT clear the alert because the taint still flows from the sensitive source. Instead log a **non-PII correlation handle** that lets you trace the record without exposing the person — e.g. an internal UUID, or a signed tracking token that encodes ids but not the raw contact.

Real instance: notification_engine ZNS adapters logged the recipient `phone`; replaced with the `tracking_id` handle -> alerts 44/45 cleared, CodeQL check green.

## Note
The bot's PR review comments auto-mark "outdated" once the flagged lines change; the code-scanning alert closes on the next scan. The green CodeQL PR check is the real gate.

## Related

- [[Never sign a security token with a secret documented as unused]]
