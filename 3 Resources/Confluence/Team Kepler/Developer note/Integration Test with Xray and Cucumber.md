---
title: "Integration Test with Xray and Cucumber"
created: 2026-03-05
updated: 2026-03-23
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49204035591/Integration+Test+with+Xray+and+Cucumber
confluence_id: "49204035591"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, xray]
---

# Integration Test with Xray and Cucumber

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49204035591/Integration+Test+with+Xray+and+Cucumber) · updated 2026-03-23*

## Overview

These document will addresses there point:

1.  The Behavior-Driven Testing Methodology

2.  The Implementation Standard and Test Project structure

3.  The Definition and Usages of Automation Test

4.

Who will need to read which section

- As you are a Business-Oriented person (Business Analysist, Product Owner or Executive Level), you would need to read (1) and (3)

- As you are an Engineer or Developer, you would need to focus on (2) and (3).

- As you are an Integrators for setting up Xray

## Behavior-Driven Testing (BDT) Methodology

### What is it?

Behavior-Driven Testing focuses on **what the system should do** from user's perspective, written in plain English that everyone understand.

**Example**

```
Feature: User Login

  Scenario: Successful login
    Given a registered user with email "john@example.com"
    When the user logs in with correct credentials
    Then the user should see the dashboard
```

### The Core Idea

Tests are written as **scenarios** using three keywords:

|           |                        |
|-----------|------------------------|
| Keyword   | Meaning                |
| **Given** | The starting condition |
| **When**  | The action performed   |
| **Then**  | The expected result    |
| **And**   | The additional result  |

### Key Benefits

- **Readable** -- Anyone can read and write scenarios (no coding needed)

- **Living documentation** -- Feature files always reflect current behavior

- **Traceability** -- Each scenario maps to a test in Xray/Jira

- **Early feedback** -- Catch misunderstandings before code is written

### How It Works

![[image-20260305-074417.png]]

### Who Does What?

![[image-20260305-074523.png]]

### The Lifecycle

![[image-20260305-074558.png]]

## Implementation Standard and Test Project structure

### Test Project Structure

This project uses [Behave](https://behave.readthedocs.io/), a Behavior Driven Development (BDD) framework for Python.

**Feature Files**

- **(**`features/*.feature`**)**: Written in Gherkin syntax, these files describe the expected behavior of the system in plain English.

**Step Definitions**

- **(**`features/steps/*.py`**)**: Python functions that map the Gherkin steps to actual test logic using the `requests` library.

**Common and Libraries**

- **(**`api/**/*.py`**):** Python client for REST API

- **(**`core/**/*.py`**):** Common Python methods that are utilized for many implementation

- **(**`features/steps/common_steps.py`**):** Python common steps

**Environment Setup**

- **(**`features/environment.py`**)**: Contains hooks for global setup and teardown. It specifically handles loading environment variables and configuration before tests run.

**3rd Party Helpers**

- **(**`helplers/*.py`**)**: Contain the method for integrating with 3rd party tool such as Xray

**CI-related Setup**

- **(**`cloudbuild.yml`**)** or **(**`cloudbuild-*.yml`**):** YAML configuration file for the Cloud Build

- **(**`gke/k8s.yaml`**)** or **(**`gke/k8s-*.yaml`**):** YAML configuration file for Google Kube Engineer manifest

**Test Resource**

- **(**`resouces/test-data/**`**):**the testing resources such as JSON schemas, images or compressed files

### Implementation Standard

As the tests are implemented in Python, We follow the [PEP8 standard](https://peps.python.org/pep-0008/).

Write code as **small, predictable functions** that take input and return output

*Pure Functions -- Same input, same output*

```
# Pure -- easy to test
def build_document_payload(title, folder_id):
    return {"title": title, "folderId": folder_id}

# Impure -- depends on external state, hard to test
def build_document_payload():
    return {"title": global_title, "folderId": get_current_folder()}
```

*No Side Effects -- Don't change things outside the function*

```
# Bad -- mutates the shared list
def add_tag(document, tag):
    document["tags"].append(tag)  # modifies original

# Good -- returns a new copy
def add_tag(document, tag):
    return {**document, "tags": [*document["tags"], tag]}
```

*Compose Small Functions -- Build complex behavior from simple pieces*

```
# Small, single-purpose functions
def create_document(api, payload):
    return api.post("/documents", json=payload)

def verify_status(response, expected=200):
    assert response.status_code == expected

def extract_id(response):
    return response.json()["id"]

# Compose them in a test step
def step_create_and_get_id(context):
    payload = build_document_payload("Test Doc", context.folder_id)
    response = create_document(context.api, payload)
    verify_status(response)
    context.doc_id = extract_id(response)

```

|  |  |
|----|----|
| Principle | What to do |
| **Pure helpers** | Put reusable logic in `common_steps` as pure functions |
| **Keep steps thin** | Step definitions should compose helpers, not contain logic |
| **Avoid global state** | Use `context` to pass data between steps, don't use module-level variables |
| **One job per function** | `create_document()` creates, `verify_document()` verifies -- never both |
| **Return, don't mutate** | Build new payloads instead of modifying existing dicts |

## Definition and Usages of Automation Test

### Automation Test

Automated tests are implemented as code, either compiled or not. Usually, they are executed during the Continuous Integration process, triggered by code changes or on a scheduled basis.

As we work with Xray, Xray provides two [different types of tests](https://docs.getxray.app/space/XRAYCLOUD/44565156) that may be used to represent automated tests:

- **Cucumber**: a test specified in natural language (i.e., in [Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin)) =\> **We will focus on this test type**

- **Generic**: all other automated tests (or any automated test, in general)

### Way of work

![[image-20260306-022951.png]]

1.  The PO

    - Create the test ticket:

      - Create Cucumber tests inside Xray (BDD format)

        Refer to how to write test scenarios: [Gherkin - Xray Cloud Documentation - XRAY view](https://docs.getxray.app/space/XRAYCLOUD/44565320/Gherkin)

        ![[image-20260304-045252.png]]

      - Define Cucumber Test Precondition **(Optional)**

      - Set the feature name (\[feature-name\].feature) into the label **(Required)**

      - ![[image-20260323-061834.png]]

        Link test ticket with Story ticket (or Task ticket)

        ![[image-20260323-062043.png]]

2.  The Engineer

    - Get feature from test ticket in Test Detail to implement the step in Python.

    - When merged into “master“ branch, the automation process will be triggered.

    - Provide the “\_TEST_SET_KEY“ in `cloudbuild-*.yml` file as the id of test ticket.

3.  The Service Account will automate

    - Checkout the code from test repository (`luz-docs-integration-test`).

    - Gather the annotated (with test tickets) feature files.

    - Execute the test feature by feature (by feature names on label).

    - Import the test results (respectively after each feature) to Xray.

    - Clean up all downloaded and generated files.
