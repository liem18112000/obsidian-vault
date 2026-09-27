---
ai_hash: f9c3e32f66ed6e38
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
- subtask
- howto
title: 'Jira MCP: create a subtask with issueTypeName Subtask + parent, labels/priority
  via additional_fields'
type: howto
---

# Jira MCP: create a subtask with issueTypeName Subtask + parent, labels/priority via additional_fields

To create a Jira **subtask** through the claude.ai Atlassian MCP `createJiraIssue` tool: set `issueTypeName: "Subtask"` and `parent: "<STORY-KEY>"` (e.g. `SCRUM-101`). The parent must already exist, so create the Story first, read its `key` from the response, then create the subtasks under it.

- **Labels, priority, components, and any custom field** go through `additional_fields`, NOT top-level params: `{"priority": {"name": "Highest"}, "labels": ["a","b"]}`.
- `description` is a top-level param (string) with `contentFormat: "markdown"` — same as editJiraIssue.
- New issue keys are assigned sequentially by the project (creating 2 stories + 16 subtasks yielded SCRUM-101 … SCRUM-118).
- Team-managed (simplified) projects still use the type name `Subtask` and `Story`.

Related: [[Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue]]

## Related

- [[Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue]]

%% ai-graph-start %%

**Related notes:**
- [[Atlassian MCP has no delete-comment tool; edit via commentId on addCommentToJiraIssue]]
- [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]]
- [[Atlassian MCP createIssueLink Blocks inwardIssue is the blocker]]

%% ai-graph-end %%