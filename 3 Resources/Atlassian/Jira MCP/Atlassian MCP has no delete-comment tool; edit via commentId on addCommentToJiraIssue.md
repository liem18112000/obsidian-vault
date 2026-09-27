---
title: "Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue"
created: 2026-09-08
type: lesson
status: seedling
source: "session 2026-09-08"
tags: [atlassian, jira, mcp, gotcha]
---

# Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue

The claude.ai Atlassian MCP exposes **no delete-comment tool**. Comments can only be added or updated, never removed programmatically — if you post a comment you later regret, the best you can do is repurpose it.

- **Edit an existing comment:** call `addCommentToJiraIssue` and pass the existing `commentId`. Omit `commentId` to add a new comment instead. It accepts `contentFormat: "markdown"`.
- **Edit summary/description:** call `editJiraIssue` with a `fields` object (e.g. `{summary, description}`) and `contentFormat: "markdown"`. Description string can be plain Markdown.
- **Cloud ID:** pass the site hostname (e.g. `leocdp.atlassian.net`) or the UUID from `getAccessibleAtlassianResources`. The hostname works directly.
- Practical consequence: to "remove" a now-redundant comment, overwrite it into a short changelog note via its `commentId`.

Related: [[Jira MCP markdown-to-ADF: bold immediately before inline code renders off]]

## Related

- [[Jira MCP markdown-to-ADF: bold immediately before inline code renders off]]
