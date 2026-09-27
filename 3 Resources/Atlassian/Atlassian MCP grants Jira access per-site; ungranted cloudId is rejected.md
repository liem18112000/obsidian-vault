---
title: "Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected"
created: 2026-09-07
type: lesson
status: seedling
source: "session 2026-09-07"
tags: [atlassian, mcp, oauth, jira, gotcha]
---

# Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected

The Atlassian MCP connection grants Jira/Confluence access **per Atlassian site (cloudId)**, not per account. If you target a site the current OAuth grant does not include, tool calls fail with **`Cloud id: <uuid> isn't explicitly granted by the user`** — even though the hostname resolves to a valid cloudId and your account may be a member of that site.

## Fix
Re-run **`/mcp`** → the Atlassian connection → re-authenticate, and on the consent screen **tick the target site** (both site membership *and* an explicit grant checkbox must be satisfied). `getAccessibleAtlassianResources` lists only the sites actually granted, so use it to confirm.

## Observed
Account had `axonivy.atlassian.net` granted but not `leocdp.atlassian.net` (cloudId `7310203d-...`); every create-issue call to leocdp was rejected until the grant was widened. Fallback when a grant can't be obtained: [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]].

## Related

- [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]]
