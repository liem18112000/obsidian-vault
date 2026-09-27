---
ai_hash: ac0b01983e0267da
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: session 2026-09-07
status: seedling
tags:
- atlassian
- mcp
- jira
- issue-links
- story-points
- gotcha
title: 'Atlassian MCP createIssueLink Blocks: inwardIssue is the blocker'
type: lesson
---

# Atlassian MCP createIssueLink Blocks: inwardIssue is the blocker

For the Atlassian MCP `createIssueLink` tool with a directional type like **Blocks**, pass **`inwardIssue` = the blocker** and **`outwardIssue` = the blocked** issue. This is the opposite of the raw Jira REST convention (where the outwardIssue carries the "blocks" verb), so trust the tool doc, not REST habit.

## Verified empirically
Called `createIssueLink(type=Blocks, inwardIssue=SCRUM-96, outwardIssue=SCRUM-97)` intending "96 blocks 97". Fetching SCRUM-97 afterward showed the link with `inwardIssue: SCRUM-96` under a type whose inward description is "is blocked by" — i.e. Jira renders "SCRUM-97 is blocked by SCRUM-96" and "SCRUM-96 blocks SCRUM-97". Correct.

## Also
- Discover the exact link-type name first with `getIssueLinkTypes` (default set: Blocks, Cloners, Duplicate, Relates).
- To set **Story Points** on a team-managed (simplified) project, the field is the custom field **`customfield_10016`** ("Story point estimate", `com.pyxis.greenhopper.jira:jsw-story-points`) — settable even on Subtasks via `editJiraIssue({fields:{customfield_10016: N}})`. Find it per-project with `getJiraIssueTypeMetaWithFields(requiredFieldsOnly=false)`.

Related: [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]], [[Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected]].

## Related

- [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]]
- [[Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected]]

%% ai-graph-start %%

**Related notes:**
- [[Jira MCP create a subtask with issueTypeName Subtask + parent, labelspriority via additional_fields]]
- [[Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected]]
- [[Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns]]

%% ai-graph-end %%