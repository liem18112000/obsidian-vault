---
title: "Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns"
created: 2026-09-07
type: howto
status: seedling
source: "session 2026-09-07"
tags: [jira, csv-import, subtasks, gotcha]
---

# Jira CSV import builds Story to Sub-task hierarchy via Issue Id and Parent Id columns

Jira's CSV importer can create a parent Story and its Sub-tasks in **one pass** by adding two temporary columns: **`Issue Id`** (a unique key per row) and **`Parent Id`** (each sub-task row sets this to the parent's `Issue Id`). During the wizard's field-mapping step, map both columns to their same-named Jira fields; the importer then nests the subtasks under the parent. No pre-existing parent key is needed — the linkage is resolved within the single import.

## Why / when it applies
This is the only clean way to bulk-load a Story→Sub-task tree from a spreadsheet without the Jira REST API (useful when the Atlassian MCP/OAuth grant lacks the target site — see [[Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected]]).

## Gotchas
- **Multi-value fields (e.g. Labels):** provide N repeated columns all headed `Labels`, one value each — then map every one of them to Labels.
- **Encoding:** write the CSV as **UTF-8 without BOM** and pick UTF-8 in the wizard, or em-dash/emoji become mojibake.
- **Subtask type name:** may be `Sub-task` or `Subtask` depending on the site — confirm in the value-mapping step.
- **Components:** must exist or you tick "create if missing".
- Descriptions import as plain text (markdown is not auto-converted to ADF).

Path: **⚙ → System → External System Import → CSV**.

## Related

- [[Atlassian MCP grants Jira access per-site; ungranted cloudId is rejected]]
