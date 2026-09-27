---
ai_hash: 766c0116505a5cb3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08
status: seedling
tags:
- atlassian
- jira
- mcp
- gotcha
title: Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue
type: lesson
---

# Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue

The claude.ai Atlassian MCP exposes **no delete-comment tool**. Comments can only be added or updated, never removed programmatically — if you post a comment you later regret, the best you can do is repurpose it.

- **Edit an existing comment:** call `addCommentToJiraIssue` and pass the existing `commentId`. Omit `commentId` to add a new comment instead. It accepts `contentFormat: "markdown"`.
- **Edit summary/description:** call `editJiraIssue` with a `fields` object (e.g. `{summary, description}`) and `contentFormat: "markdown"`. Description string can be plain Markdown.
- **Cloud ID:** pass the site hostname (e.g. `leocdp.atlassian.net`) or the UUID from `getAccessibleAtlassianResources`. The hostname works directly.
- Practical consequence: to "remove" a now-redundant comment, overwrite it into a short changelog note via its `commentId`.

Related: [[Jira MCP markdown-to-ADF bold immediately before inline code renders off|Jira MCP markdown-to-ADF: bold immediately before inline code renders off]]

## Related

- [[Jira MCP markdown-to-ADF bold immediately before inline code renders off|Jira MCP markdown-to-ADF: bold immediately before inline code renders off]]

%% ai-graph-start %%

**Related notes:**
- [[Jira MCP markdown-to-ADF bold immediately before inline code renders off]]
- [[Jira MCP create a subtask with issueTypeName Subtask + parent, labelspriority via additional_fields]]
- [[Atlassian MCP connector binds to one cloud site, which can differ from your REST token's site]]
- [[Edit Confluence Cloud via authenticated Playwright browser when the Atlassian MCP app is not installed]]
- [[Vinnstack's Jira client avoids the Atlassian MCP connector]]

%% ai-graph-end %%