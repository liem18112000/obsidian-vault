---
title: "customer360 AI campaign lifecycle: agent plans, api persists draft, email_engine renders at send"
created: 2026-09-21
type: model
status: seedling
source: "session 2026-09-21"
tags: [customer360, campaign, architecture, email, crm_campaign, lifecycle]
---

# customer360 AI campaign lifecycle: agent plans, api persists draft, email_engine renders at send

End-to-end path of an AI-generated marketing campaign in customer360, across three services:

1. **customer360-agent** (`/plan/email`) — the AI only PLANS. Given a brief + a CLOSED candidate content list, it returns a `GeneratedCampaignPlan` (name, objective, strategy_summary, action_plan, start/end dates, and selected `content_item_ids`). It does NOT author email subject/body.

2. **customer360-api** (`core/repositories/campaign_draft_repository.py::create_draft`) — persists the plan as a `crm_campaign` row: `status="Draft"`, `approval_status="InReview"`, `strategy_summary`, the full plan in the `ai_plan` JSONB column, `start/end_date`, a link to an existing **Approved** `crm_message_templates` row (`template_id` — this is the actual email copy, not AI-written), and one `crm_campaign_content_items` row per selected item (ordered by `position`). Off-list content ids are discarded defense-in-depth. Nothing sends while `InReview` — a human approves/rejects.

3. **backend-system/email_engine** (`send.py` + `rendering.py`) — at send time, per recipient: build context `{first_name,last_name,name,email,unsubscribe_url}`, substitute `{{ }}` placeholders in the template's subject/html_body/text_body (plain regex, not Jinja — see [[email_engine renders with plain regex substitution, not Jinja, so AI-authored templates cant execute code]]), HTML-escape the HTML body, rewrite links for click-tracking, inject the open pixel, then dispatch via the configured adapter (SMTP/mock).

Key mental model: the agent chooses WHAT (content + schedule); the Approved template holds the COPY; email_engine produces the final per-recipient message. The three are separate services/DBs — don't couple an agent-side test to the api/email_engine DB layers.

## Related

- [[email-channel-is-outbound-template]]
