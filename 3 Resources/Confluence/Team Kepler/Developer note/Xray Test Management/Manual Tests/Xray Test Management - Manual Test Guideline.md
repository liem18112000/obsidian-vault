---
ai_hash: df28297f2ef0bcdc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49284284418'
confluence_path: Team Kepler > Developer note > Xray Test Management > Manual Tests
created: 2026-03-30
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- xray
title: Xray Test Management - Manual Test Guideline
type: source
updated: 2026-04-01
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49284284418/Xray+Test+Management+-+Manual+Test+Guideline
---

# Xray Test Management - Manual Test Guideline

*Confluence source · Team Kepler › Developer note › Xray Test Management › Manual Tests · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49284284418/Xray+Test+Management+-+Manual+Test+Guideline) · updated 2026-04-01*

## Key Concepts

![[image-20260330-094457.png]]

### Xray Issue Types

Xray extends Jira with four custom issue types:

|  |  |  |  |
|----|----|----|----|
| Issue Type | Jira Key Prefix | Purpose | Example |
| **Test** | `LUZ-XXXX` (type: Test) | A reusable test case with steps and expected results | "Verify document upload with valid PDF" |
| **Test Set** | `LUZ-XXXX` (type: Test Set) | A logical group of tests (by feature, module, etc.) | "Document Upload Tests" |
| **Test Plan** | `LUZ-XXXX` (type: Test Plan) | A collection of test executions for a release/sprint | "Sprint 150 Test Plan" |
| **Test Execution** | `LUZ-XXXX` (type: Test Execution) | A single run of one or more tests with results | "Sprint 150 - Smoke Test Run" |

### Test Types in Xray

|  |  |  |
|----|----|----|
| Type | Description | When to Use |
| **Manual** | Step-by-step tests executed by a human | UI testing, exploratory, acceptance testing |
| **Cucumber** | BDD-style Given/When/Then scenarios | Behavior-driven development workflows |
| **Generic** | Unstructured test — free-text definition | Automated tests, scripts, or ad-hoc checks |

### Test Lifecycle

![[image-20260330-094144.png]]

![[image-20260330-094104.png]]![[image-20260330-071930.png]]

### Test Run Statuses

|               |        |                                         |
|---------------|--------|-----------------------------------------|
| Status        | Icon   | Meaning                                 |
| **TODO**      | Grey   | Test has not been executed yet          |
| **EXECUTING** | Blue   | Test is currently in progress           |
| **PASS**      | Green  | All steps passed as expected            |
| **FAIL**      | Red    | One or more steps failed                |
| **ABORTED**   | Orange | Execution was stopped before completion |

### Step-Level Statuses

Each step within a manual test can have its own status:

|             |                                              |
|-------------|----------------------------------------------|
| Status      | Meaning                                      |
| **PASS**    | Step result matches expected outcome         |
| **FAIL**    | Step result does NOT match expected outcome  |
| **TODO**    | Step has not been executed                   |
| **BLOCKED** | Step is being blocked by some previous steps |

------------------------------------------------------------------------

## Naming Conventions & Standards

### Test Naming

Format: `Verify <action> <condition/scenario>`

|              |                                                         |
|--------------|---------------------------------------------------------|
| Pattern      | Example                                                 |
| Happy path   | `Verify PDF upload to root folder`                      |
| Boundary     | `Verify upload rejects file exceeding 5MB limit`        |
| Negative     | `Verify upload fails with expired auth token`           |
| Permission   | `Verify upload denied without LUZ_DOCS permission`      |
| Cross-tenant | `Verify user cannot access documents of another tenant` |

### Labels

Use consistent labels across all test issues:

|  |  |
|----|----|
| Label Category | Examples |
| **Module** | `document-management`, `folder-management`, `search`, `enrichment`, `security` |
| **Test Type** | `smoke`, `regression`, `e2e`, `boundary`, `negative` |
| **Priority** | `p1-critical`, `p2-high`, `p3-medium` |
| **Feature** | `upload`, `download`, `delete`, `trash`, `archive`, `restore` |
| AI Related | `ai-autmation-test`, `ai-driven-test` |

### Test Plan Naming

Format: `Plan - <Sprint/Release> - <Scope>`

|                                          |
|------------------------------------------|
| Example                                  |
| `Plan - Sprint 150 - Full Regression`    |
| `Plan - Release 0.01.11 - Smoke Tests`   |
| `Plan - Hotfix LUZ-12600 - Verification` |

### Test Execution Naming

Format: `Execution - <Sprint/Release> - <Scope> - <Environment>`

|                                                            |
|------------------------------------------------------------|
| Example                                                    |
| `Execution - Sprint 150 - Smoke - Staging`                 |
| `Execution - Sprint 150 - Regression - UAT`                |
| `Execution - Hotfix LUZ-12600 - Verification - Production` |

### Test Set Naming

Format: `Test Set - <Module/Feature>`

|                                   |
|-----------------------------------|
| Example                           |
| `Test Set - Document Upload`      |
| `Test Set - Folder Management`    |
| `Test Set - Search and Filtering` |

## Best Practices

### Test Case Design

**DO**:

- **One test case = one scenario.** Avoid combining multiple test objectives into a single test.

- **Keep steps atomic.** Each step should perform a single action.

- **Write expected results for every step.** Never leave expected results blank.

- **Use preconditions** to describe the required state before test execution.

- **Include test data** explicitly — do not assume the tester knows the data.

**DON'T:**

- Don't write vague steps like "Check everything works."

- Don't duplicate tests — reuse existing tests via Test Sets.

- Don't hardcode environment-specific values (URLs, credentials) into steps — use preconditions or test data fields.

### Sample: Well-Written vs. Poorly-Written Test

**Poorly Written Test:**

|      |                    |                    |
|------|--------------------|--------------------|
| Step | Action             | Expected Result    |
| 1    | Upload a document  | It works           |
| 2    | Check the document | It should be there |

**Well-Written Test:**

|  |  |
|----|----|
| Field | Value |
| **Summary** | Verify PDF document upload to root folder |
| **Precondition** | User is logged in with `LUZ_DOCS` permission. Tenant `company-a` exists. No documents in root folder. |
| **Priority** | High |
| **Labels** | `document-management`, `upload`, `smoke` |

|  |  |  |  |
|----|----|----|----|
| Step \# | Action | Data | Expected Result |
| 1 | Navigate to the Document Management page for tenant `company-a` | URL: `/{companyTenantId}/documents` | Document list page is displayed. Root folder is empty. |
| 2 | Click the **"Upload"** button | — | File picker dialog opens |
| 3 | Select a PDF file and confirm upload | File: `test-report.pdf` (2.3 MB) | Upload progress bar appears. File is uploaded successfully. Success notification is shown. |
| 4 | Verify the document appears in the document list | — | `test-report.pdf` appears in the list with correct file name, size (2.3 MB), upload date (today), and status `ACTIVE`. |
| 5 | Click on the document name to preview | — | Document preview opens. PDF content is rendered correctly. |

### Organizing Tests

|                   |                      |                               |
|-------------------|----------------------|-------------------------------|
| Grouping Strategy | Use For              | Example Test Set Name         |
| By Feature/Module | Functional coverage  | `Test Set - Document Upload`  |
| By Priority       | Risk-based testing   | `Test Set - P1 Critical Path` |
| By Test Type      | Smoke/Regression/E2E | `Test Set - Smoke Tests`      |
| By Sprint/Release | Sprint scope         | `Test Set - Sprint 150`       |

### Traceability

Always link Tests to their parent requirements (Stories/Tasks):

This enables:

- **Requirement Coverage Reports** — see which stories have tests and which don't.

- **Impact Analysis** — when a story changes, you know which tests to re-run.

### Defect Linking

When a test step fails:

1.  Create a **Bug** issue in Jira directly from the failed test execution.

2.  Xray automatically links the bug to the test run.

3.  When the bug is fixed, re-execute the test in a new Test Execution.

### Evidence and Attachments

- Attach **screenshots** to failed steps as evidence.

- Attach **logs** or **API responses** when testing backend functionality.

- Use the **comment field** on each step to note any observations.

------------------------------------------------------------------------

## Step-by-Step: Organize with Test Repository

### Overall Layout

The **Test Repository** enables you to view two ways of organizing Issues:

- Folder view: hierarchical organization at the project level by allowing you to organize Tests and Preconditions in folders and sub-folders.

- Test Set view: flat-list organization of Tests through the use of Test Sets.

Each project has its own Test Repository.

- A Test or Precondition can only belong to one folder within the Test Repository. 

![[image-20260401-055953.png]]![[image-20260401-060035.png]]

### Folders Section

The Folders section is a master-detail view which contains the folder hierarchy At the root of the Test Repository, you will see the **Test Repository** folder. This folder has the following characteristics:

- It corresponds to the root folder.

- Is read-only; meaning it can't be deleted or renamed.

- Is composed of multiple folders and sub-folders along with Tests, in the context of the current project.

- Contains all non-organized Test Issues (i.e., Tests that are not part of any folder), in the context of the current project.

Within the **Test Repository** root folder:

- Multiple folders can be created, and Tests can be added to them.

- Tests may only be part of one folder.

- Folders can be created at the root of the Test Repository or in any sub-folder within it.

In a given parent folder, folders must not have similar names.

- For this, Xray does a case-sensitive check of the trimmed folder name whenever you create or rename a folder.

- Moreover, folder names must not use the "/" or "\\ characters.

### Adding a Test to a Folder

This action corresponds to moving the Tests from any folder they may already be (including the Test Repository) to the destination folder.

Thus, if you add a Test that is currently in a folder within the Test Repository to some destination folder, then it will be moved from the source folder to the destination folder.

![[image-20260401-061022.png]]

![[image-20260401-061320.png]]![[image-20260401-061430.png]]![[image-20260401-061523.png]]

## Step-by-Step: Setup Test Environment

A Test Environment contains all necessary elements, including the system under test, for testing to be performed on it.

Depending on your context, a Test environment may represent:

- A Testing stage (e.g. "development", "staging", "preproduction", "production").

- A device model or device operating system (e.g. "Android", "iOS").

- An operating system (e.g. "Windows", "macOS", "Linux").

- A browser (e.g. "Edge", "Chrome", "Firefox").

The semantics of what a Test Environment represents depends on your specific context.

![[image-20260401-051753.png]]

### Step 1: Create a new Test Environment

Within a Test Execution, you may specify the **Test Environment(s)** where the Tests will be executed in the respective attribute. A Test Environment is similar to a label, but Xray has special logic to deal with it.

![[image-20260401-052455.png]]

![[image-20260401-053150.png]]

![[image-20260401-053338.png]]

![[image-20260401-053536.png]]

## Step-by-Step: Creating a Manual Test in Xray

### Step 1: Create a New Test Issue

1.  In Jira, click **"Create"** (or press `C`).

2.  Set the following fields:

|  |  |
|----|----|
| Field | Value |
| **Project** | LUZ (or your project key) |
| **Issue Type** | Test |
| **Summary** | Descriptive test name (see [Naming Conventions](http://localhost:63342/markdownPreview/1712716958/markdown-preview-index-aste9q21l4nmu16rldg5k93k5p.html#7-naming-conventions--standards)) |
| **Priority** | High / Medium / Low |
| **Labels** | Feature labels (e.g., `document-upload`, `smoke`) |

Expected Result Screen

![[image-20260330-080917.png]]

**Screenshot Example:** "Create Issue" dialog with Issue Type set to "Test"

![[image-20260330-075627.png]]![[image-20260330-080712.png]]

![[image-20260330-080023.png]]

### Step 2: Set the Test Type to Manual

After creating the test issue, open it and navigate to the **Xray** section:

1.  In the test issue, find the **"Test Type"** field.

2.  Select **"Manual"** from the dropdown.

> **Screenshot Example:** The Test issue detail view showing the Test Type selector set to "Manual"

![[image-20260330-081253.png]]

### Step 3: Define Preconditions

1.  In the test issue, scroll to the **"Preconditions"** section. A precondition can be **reused across multiple tests**.

2.  Click **"Add Precondition"** or create a new one.

3.  Write the required preconditions.

Expected Result Screen

![[image-20260330-082130.png]]

> **Screenshot Example:** Precondition editor within the Test issue

![[image-20260330-081625.png]]![[image-20260330-082038.png]]

![[image-20260330-081955.png]]

### Step 4: Add Test Steps

1.  In the test issue, navigate to the **"Test Details"** tab.

2.  Click **"Add Step"** for each test step.

3.  Fill in **Action**, **Data**, and **Expected Result** for each step.

Expected Result Screen

![[image-20260330-085044.png]]

> **Screenshot Example:** The Test Details tab showing all 5 steps with Action, Data, and Expected Result columns

![[image-20260330-083406.png]]

![[image-20260330-084736.png]]![[image-20260330-084853.png]]

### Optional Step: Add Test Cases with AI

The process for generating AI Tests involves opening the AI Test Cases

Includes structured [Test Steps](https://docs.getxray.app/space/XRAYCLOUD/44565316) (Action, Data, Expected Result).

1.  On your Jira Cloud instance, enter the Testing Board and choose one of the entry points described in the section above, and click the *Test Cases* button. A modal will open

2.  Here, you can:

    - Select your Test Type (mandatory field).

    - Link [Preconditions](https://docs.getxray.app/space/XRAYCLOUD/44565159) (optional field): link relevant Preconditions; only “summary” and “description” fields will be used as context. 

    - Link Coverable Issues (optional field): add well-defined User Stories, Epics, or Issues that describe the functionality to be tested. When launching the AI Settings feature from a coverable Issue, the *Link Coverable Issues* field will be pre-filled. Only “summary” and “description” fields will be used in context.

    - Insert your Test Requirements (mandatory field): If linking detailed Coverable Issues, the Requirements value can be as simple as "Create sufficient test cases for the linked Issues”.

3.  Once you're finished filling in these fields, click *Generate Test Cases* to proceed. You will see a progress bar, and your Test Cases will be instantaneously generated

4.  You can:

    - Use the respective boxes to select the Test/s you wish to review and edit. When you edit the title and description of selected Tests, the generator will update them in the next step and generate Tests based on your changes.

    - Create subfolders from topics inside the destination folder by selecting the corresponding box.

5.  To proceed with the Test Case generation, click *Review & Edit*. You will see a progress bar, and your Test Cases will be instantaneously listed. Once the Tests are listed, select the Tests you wish to edit/review by checking the corresponding box.

6.  Start by reviewing the *Details*

7.  You will see a progress bar, and your Test Cases will be instantaneously generated, taking you back to the Testing Board

![[image-20260401-063058.png]]![[image-20260401-063405.png]]![[image-20260401-063528.png]]![[image-20260401-063549.png]]

![[image-20260401-063602.png]]![[image-20260401-063901.png]]![[image-20260401-063627.png]]

### Optional Step: Add Test Data with Dataset

To create or edit the default dataset (within a Test):

1.  Click the *Dataset* button. A modal will open.

2.  Click the *Create parameter* button. A modal will open for you to specify parameter attributes

![[image-20260401-012545.png]]

![[image-20260401-012608.png]]

**Define the dataset:**

1.  Specify the parameter's name (mandatory field).

    - Parameter names must start with a letter or underscore and can only contain letters, numbers, a space between words, "\_", "-" and a maximum of 64 characters.

2.  Check the *Combinatorial* checkbox if you are creating a combinatorial parameter.

3.  Choose the parameter type: *Text* or *List*. If the parameter type is a *List*, you can:

    - Create an *Ad hoc list* just for this parameter. You need to specify the values for the list (mandatory field).

    - Use a *Project* predefined list.

4.  Once you're finished, click *Create*. You will be redirected to the dataset modal.

**Expected Results:**

![[image-20260401-064325.png]]

![[image-20260401-012823.png]]

**Combinatorial Parameters**

Combinatorial parameters are special parameters that will be combined with the remaining parameters to generate all possible combinations automatically. This prevents users from typing all the combinations when creating a dataset.

Seeding parameters are those parameters that describe fixed Test cases. The seeding parameters will not be combined with each other; they will only be combined with combinatorial parameters.

**Example**

You can add books to a shopping cart in your online bookstore. The parameters are: *Item*, *Price*, *Rating*, *In Stock*, *Condition*, and *Format*. There are certain books to be tested (three in this case). However, we will test all combinations of these books with the following parameters: *Gift* and *Quantity*. 

In this case, these will be the seeding parameters:

- *Item.*

- *Price.*

- *Rating.*

- *In Stock*,

- *Condition.*

- *Format.*

Since we want to test these items with all the combinations of *Gift* and *Quantity* parameters, we can create these as combinatorial parameters: 

- *Gift\**

- *Quantity\**

**Combinatorial parameters are denoted with an asterisk (\*) suffix. **

![[image-20260401-014406.png]]![[image-20260401-014437.png]]

Adding Combinatorial Parameter Values

1.  Once you have at least one parameter, you can start filling in their values and adding new iterations.

2.  A placeholder is provided within each combinatorial parameter.

3.  To add new values to combinatorial parameters.

    - For **text** parameters, type the value and click the *check* icon.

    - For **list** parameters, select an option and click the *check* icon.

![[image-20260401-013231.png]]

![[image-20260401-013238.png]]

Adding Rows (Filling the Parameter Values)

1.  Once you have non-combinatorial parameters, an empty placeholder row appear so that the parameters can be populated for the default iteration. 

2.  Editing parameter values is as simple as editing their corresponding cells. The values will be kept when the cell loses focus.

3.  You can navigate between cells of the same row and also between rows using the keyboard: TAB (forward), SHIFT+TAB (backward).

4.  To create new rows, you can click the *New* button, or navigate using the keyboard from the last row.

![[image-20260401-013404.png]]

### Optional Step: Using Xray Dataset

Parameterized Tests in Xray are defined just like any other Test with the addition of some parameter names within the specification. 

- Parameters are embedded within the Test specifications using the following notation:** **`${PARAMETER_NAME}`.

![[image-20260401-011749.png]]

When specifying a Test Step, to reference a parameter you have two options:

1.  Start typing `${`. If there is a default dataset defined on the Test, you will see a list of the available parameters. Choose the desired parameter using the cursor keys or mouse. The parameter will be inserted into the text.

2.  Use the toolbar button `${`. After pressing this button, and if there is a default dataset defined on the test, you will see a list of the available parameters. Choose the desired parameter using the cursor keys or mouse. The parameter will be placed at the cursor position.

![[image-20260401-012142.png]]

### Optional Step: Using Modular Tests

Modular Test design is a way of promoting Test case **reusability** and **composition**. To design modular Tests, you can create a **Manual Test** where some of the **Test Step** call or include other **Test Cases**. This prevents Testers from having to write the same steps over and over again for different high-level Tests. Still, they can also be run individually if needed.

**A Call Test** can, in turn, also call other Tests. You can compose a Test scenario with up to five levels of depth.

Limitations:

- Precondition Issues will be ignored when Tests are being called/included in other Tests.

- Modular Test design only works with manual Tests.

- The call context dataset for call Tests does not support multiple iterations.

- There is a depth limit of five call Tests: A → B → C → D → E.

- The total number of call Tests allowed for a given Test Run is 200.

![[image-20260401-015711.png]]

**Creating a Call Test Step**

1.  On your Jira Cloud instance, open a Test Issue with Test Steps.

2.  Click *Add Step* and select *Call Test*. A modal will open.

3.  Use the dropdown to select the Test Issue and then click *Add*.

4.  The Step will be added right away as a new Test Step, appearing in purple.

![[image-20260401-020539.png]]

![[image-20260401-020659.png]]

**Parameterizing Call Tests**

It is possible to specify and override the parameter values for Call Tests. After defining the call Test Step, you can edit the dataset in this context. To define the test data for a Call Test:

1.  Hover the Call Test Step and click the dataset icon.

2.  The dataset modal will appear.

3.  Click *Add parameter* and select *New parameter...*

4.  A new modal will open. Enter a *Name* (mandatory field) and select a *Type*.

5.  Click *Create*.

![[image-20260401-021037.png]]![[image-20260401-021702.png]]

![[image-20260401-021714.png]]![[image-20260401-021725.png]]

### Step 5: Link Test to Requirement (Story)

1.  In the test issue, scroll to the **"Links"** section.

2.  Click **"Link"** and select link type **"tests"** (or "covers").

3.  Search for the related Story/Task (e.g., `LUZ-12345`).

Expected Result:

![[image-20260330-085134.png]]

>

> **Screenshot Example:** The dialog showing a "tests" relationship to the parent Story
>
> ![[image-20260330-085200.png]]

![[image-20260330-085231.png]]

------------------------------------------------------------------------

## Step-by-Step: Executing a Manual Test

### Step 1: Create a Test Execution

1.  Click **"Create"** in Jira.

2.  Set **Issue Type** to **"Test Execution"**.

3.  Fill in the fields:

|                 |                                           |
|-----------------|-------------------------------------------|
| Field           | Value                                     |
| **Summary**     | `Sprint 150 - Document Upload Smoke Test` |
| **Fix Version** | `0.01.11.00`                              |
| **Environment** | `Staging`                                 |
| **Assignee**    | Tester's name                             |

Expected Result:

![[image-20260330-090302.png]]

> **Screenshot Example:** Create Issue dialog with Issue Type "Test Execution"

![[image-20260330-085902.png]]![[image-20260330-085955.png]]

### Optional Step: Creating Sub-Test Executions

A Sub-Test Execution functions similarly to a Test Execution Issue type.

- The key difference is that Sub-Test Execution is a sub-task issue type and can be created within the context of a Requirement.

- Creating a Test Execution as a sub-task of a requirement Issue allows you to track executions on the Agile board.

![[image-20260401-065006.png]]

![[image-20260401-065019.png]]

### Step 2: Execute a Test (Run)

1.  In the Test Execution, click the **play button \[▶\]** next to the test you want to run.

2.  The **Test Run** screen opens with all steps listed.

3.  Define the Test Environment

**Exected Result**

![[image-20260330-090145.png]]

> **Screenshot Example:** The Test Run screen showing step-by-step execution with PASS, FAIL and TODO statuses

![[image-20260330-090948.png]]![[image-20260401-054054.png]]![[image-20260401-053800.png]]

**If you apply Dataset, test run multiple iterations with dataset you provide:**

- The Step parameters will be replaced by the corresponding iteration values.

- The Steps affect iteration status, in turn, affects the overall Test run status

![[image-20260401-014059.png]]

**When run composed of modular Tests**

- Xray will unfold/expand all Steps and replace the parameters with their resolved values.

- On the Step number column, a warning indicates when a Step belongs to another Test Issue.

- The users can click the Test Issue key to navigate to that Test.

![[image-20260401-050129.png]]

### Step 3: Mark Step Results

For each step:

1.  **Execute the action** described in the step.

2.  **Compare** the actual result with the expected result.

3.  Click the **status button** for the step:

    - **PASS** (green) — actual matches expected

    - **FAIL** (red) — actual does NOT match expected

4.  **If FAIL:** Add a comment explaining what went wrong, and attach evidence (screenshot, log).

Note:

- There is one more status “Blue“ mean being blocked

- For example, the step 3 is being block by step 2 failure

![[image-20260330-091022.png]]![[image-20260330-091401.png]]

### Step 4: Handle Failures — Create a Defect

When a step fails:

1.  Click **"Create Defect"** button on the failed step (or the overall test run).

2.  A pre-populated Jira Bug issue is created with:

    - Link back to the Test Execution

    - Test step details

    - Environment information

3.  Fill in additional details (severity, description, reproduction steps).

4.  The defect is automatically linked to the Test Run.

Expected Results

![[image-20260330-092311.png]]![[image-20260330-092506.png]]

> **Screenshot Example:** Defect creation dialog pre-populated from a failed test step

![[image-20260330-092012.png]]

![[image-20260330-092121.png]]

### Step 5: Set the Overall Test Run Status

After all steps are executed:

1.  If **all steps passed** → Overall status is automatically set to **PASS**.

2.  If **any step failed** → Overall status is automatically set to **FAIL**.

3.  You can **override** the overall status manually if needed.

4.  Click **"Save"** or navigate away — results are auto-saved.

If the test is PASSED:

1.  Set the status of the ticket to “Resolved“ (GREEN)

![[image-20260330-095017.png]]

2.  Set the status of Test Execution to “Resolved“ (GREEN)

![[image-20260330-095217.png]]

![[image-20260330-092645.png]]![[image-20260330-092843.png]]

### Step 6: Review Test Execution Results

Go back to the Test Execution issue to see the summary:

1.  Locate the test execution report for viewing the status

2.  View the test coverage at the original story which has been tested

**Expected Results**

![[image-20260330-095511.png]]

> **Screenshot Example:** Test Execution summary showing progress bar and per-test results

![[image-20260330-093230.png]]![[image-20260330-093135.png]]![[image-20260330-095430.png]]

------------------------------------------------------------------------

## Step-by-Step: Versioning Test

### Create new version

1.  On your Jira, go to a Test Issue.

2.  Next to the Issue's version information, click the ellipsis icon (Figure 1 - 1) and select *New version* (Figure 1 - 2). A modal will open (Figure 2).

![[image-20260330-101654.png]]

1.  In this modal you can set the version name and choose a base version to copy the definition from

2.  You can also choose to make the new version the Default version

3.  Once you're finished, click *Create*

![[image-20260330-101855.png]]

### Setting a Default Version

1.  On your Jira Cloud instance, go to a Test Issue.

2.  Next to the Issue's version information, click the ellipsis icon and select *Set default*

![[image-20260330-102250.png]]

### Executing a Test Version

You can create new executions of a specific version from the Test Runs web panel on the Test Issue page. 

1.  1On your Jira, go to a Test Issue and click *Test Runs*.

2.  Click the *Execute in* button and select the *New test execution...* option.

3.  The Create Test Execution modal will open.

4.  Once you're finished, click *Create*

![[image-20260330-102536.png]]

%% ai-graph-start %%

**Related notes:**
- [[Integration Test with Xray and Cucumber]]
- [[Use Case - Xray Feature Import - Local Tool]]
- [[Invoice Run V2 - Retry Uploaded customer document step - Should update correct status after retry]]
- [[Test and code review report template.2]]
- [[CROSS-TEST LUZ-142507 Implement Analyze API Integration (Phase 1) Part 2]]

%% ai-graph-end %%