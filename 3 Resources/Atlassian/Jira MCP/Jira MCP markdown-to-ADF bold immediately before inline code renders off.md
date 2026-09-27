---
ai_hash: 59c49e0ad87dbec9
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
- markdown
- adf
- gotcha
title: 'Jira MCP markdown-to-ADF: bold immediately before inline code renders off'
type: gotcha
---

# Jira MCP markdown-to-ADF: bold immediately before inline code renders off

When the Jira MCP converts a Markdown `description`/comment to ADF, **bold text placed immediately before an inline-code span** can render slightly off. A pattern like `**Database schema reality (**` + `` `file.sql` `` produced a stray/misplaced bold marker in the stored ADF.

Fix: keep the bold run and the inline code separated by a normal space or restructure so the bold segment ends cleanly (e.g. put the label and the code on their own, `**Label:** ` then `` `code` ``). Nested bullet lists and normal bold otherwise convert fine.

Minor cosmetic-only issue — content is intact, just the emphasis boundary. Worth knowing when authoring long Markdown descriptions through the MCP.

Related: [[Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue]]

## Related

- [[Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue]]

%% ai-graph-start %%

**Related notes:**
- [[Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue]]

%% ai-graph-end %%