---
ai_hash: 5be84cddc7f4964a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: session 2026-09-07
status: seedling
tags:
- atlassian
- mcp
- oauth
- jira
- gotcha
title: Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected
type: lesson
---

# Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected

The Atlassian MCP connection grants Jira/Confluence access **per Atlassian site (cloudId)**, not per account. If you target a site the current OAuth grant does not include, tool calls fail with **`Cloud id: <uuid> isn't explicitly granted by the user`** — even though the hostname resolves to a valid cloudId and your account may be a member of that site.

## Fix
Re-run **`/mcp`** → the Atlassian connection → re-authenticate, and on the consent screen **tick the target site** (both site membership *and* an explicit grant checkbox must be satisfied). `getAccessibleAtlassianResources` lists only the sites actually granted, so use it to confirm.

## Observed
Account had `axonivy.atlassian.net` granted but not `leocdp.atlassian.net` (cloudId `7310203d-...`); every create-issue call to leocdp was rejected until the grant was widened. Fallback when a grant can't be obtained: [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]].

## Related

- [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]]

%% ai-graph-start %%

**Related notes:**
- [[Atlassian MCP connector binds to one cloud site, which can differ from your REST token's site]]
- [[Jira issue HTML export view bypasses missing MCP grant]]
- [[claude.ai Atlassian MCP has Jira scopes only — Confluence returns 403 app-not-installed]]
- [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]]
- [[Read a private Confluence page via REST API with ATLASSIAN API token]]

%% ai-graph-end %%