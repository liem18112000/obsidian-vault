---
title: "Jira MCP: create a subtask with issueTypeName Subtask + parent, labels/priority via additional_fields"
created: 2026-09-08
type: howto
status: seedling
source: "session 2026-09-08"
tags: [atlassian, jira, mcp, subtask, howto]
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
