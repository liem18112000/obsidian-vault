---
ai_hash: 6e7ed7c0dc72bc3e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49231036420'
confluence_path: Team Kepler > Developer note > Integration Test with Xray and Cucumber
created: 2026-03-13
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- xray
title: 'Use Case: Xray Feature Import - Local Tool'
type: source
updated: 2026-03-13
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49231036420/Use+Case+Xray+Feature+Import+-+Local+Tool
---

# Use Case: Xray Feature Import - Local Tool

*Confluence source · Team Kepler › Developer note › Integration Test with Xray and Cucumber · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49231036420/Use+Case+Xray+Feature+Import+-+Local+Tool) · updated 2026-03-13*

### Purpose

Import local `.feature` files from the `features/` directory into Xray Cloud.

- Each file becomes a Test issue in Jira.

- All imported tests are grouped into a Test Set (found by name or created).

### Step-by-step breakdown

Discover feature files

- Recursively globs all `.feature` files under the given directory.

- Backslashes are normalized to forward slashes for cross-platform consistency.

Find or create Test Set

- Uses Xray's **GraphQL API** (not REST) to search for an existing Test Set by name, or create one if not found.

```
query GetTestSets($jql: String!, $limit: Int!) {
    getTestSets(jql: $jql, limit: $limit) {
        total
        results { issueId jira(fields: ["key", "summary"]) }
    }
}
```

Create (mutation)

```
mutation CreateTestSet($jira: JSON!) {
    createTestSet(jira: $jira) {
        testSet { issueId jira(fields: ["key"]) }
        warnings
    }
}
```

- Creates a Test Set with:

  - `summary`: e.g. "Test Set 13-03-2026" (auto-generated from date if not provided)

  - `project.key`: "LUZ"

  - `customfield_10049`: "Kepler" (team field)

Import each feature file (parallel)

- Each `.feature` file is imported in a thread pool (`max_workers=8`).

- Extract labels from subfolder path

- Maps the directory structure to Jira labels:

|  |  |
|----|----|
| File path | Labels |
| `features/document/create/create_document.feature` | `["document", "create", "create_document.feature"]` |
| `features/tenant/delete/delete_tenant.feature` | `["tenant", "delete", "delete_tenant.feature"]` |

Upload to Xray

```
resp = requests.post(
    f"{XRAY_BASE_URL}/import/feature",
    headers=headers,
    params={"projectKey": project_key},
    files={
        "file": (basename, open(file_path, "rb"), "text/plain"),
        "testInfo": ("testInfo.json", open(tmp_path, "rb"), "application/json"),
    },
)
```

|            |                                           |                   |
|------------|-------------------------------------------|-------------------|
| Part       | Content                                   | Type              |
| `file`     | The `.feature` file                       | `text/plain`      |
| `testInfo` | Metadata JSON (labels, description, team) | `application/jso` |

Add tests to Test Set:

- Collects the internal Xray issue IDs needed for adding to the Test Set.

- After all files are imported, all collected test IDs are added to the Test Set via GraphQL:

```
mutation AddTestsToTestSet($issueId: String!, $testIssueIds: [String]!) {
    addTestsToTestSet(issueId: $issueId, testIssueIds: $testIssueIds) {
        addedTests
        warning
    }
}
```

![[image-20260313-081409.png]]

%% ai-graph-start %%

**Related notes:**
- [[Integration Test with Xray and Cucumber]]
- [[Use Case - AI-driven Testing]]
- [[Xray Test Management - Manual Test Guideline]]
- [[Use Case - Run by Test Set - Complete Process Flow]]
- [[Multi-Agentic Architecture - Apply in AI Driven Testing]]

%% ai-graph-end %%