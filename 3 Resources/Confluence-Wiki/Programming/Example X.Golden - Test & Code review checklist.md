---
title: "Example: X.Golden - Test & Code review checklist"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/34388581217/Example+X.Golden+-+Test+Code+review+checklist
space: "X4"
topic: programming
relevance: 0.731
depth: 2.81
updated: 2019-01-10
attachments: 0
tags:
  - confluence
  - programming
  - space/x4
---

# Example: X.Golden - Test & Code review checklist

> [!info] Imported from Confluence
> Space **X4** · updated 2019-01-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/34388581217/Example+X.Golden+-+Test+Code+review+checklist)
> Relevance 0.731 · topic `programming`

Describe a steps to complete your test & code review

## Step-by-step guide

1.  ### Code review guidelines:

    - Your code is run-able

    - **Pass code review valid rules - Link: **

      - **<a href="https://axonivy.atlassian.net/wiki/pages/createpage.action?spaceKey=X4&amp;title=X.Golden%20-%20Code%20review%20rules&amp;linkCreation=true&amp;fromPageId=34388581217" class="createlink">Code review rules</a>[ ](https://axonivy.atlassian.net/wiki/spaces/X4/pages/34388581211/Example+X.Golden+-+Code+review+rules)**

      - ****[Convention & Agreement](https://axonivy.atlassian.net/wiki/spaces/X4/pages/34388581213/Example+X.Golden+-+Agreement+list)****

    - **Flyway migration**:

      - All changes script are created & committed to the correct folder of gline_package in SVN.

      - **Schema & DB name** shouldn't be defined inside your sql script.

      - All sql scripts are updated by flyway (**don't run script manually**)

        - VN server

        - Swiss server: test & demo database.

      - All changes have to be updated in schema_version table correctly.

    - All test case was created / updated

    - Technical documentation is updated

2.  ### Test guidelines

    - Implement code is deployed on development server.

    - <span style="letter-spacing: 0.0px;">Cover all the requirement in </span>X.Golden - GENERAL Requirements<span style="letter-spacing: 0.0px;"> page.</span>

    - All acceptance criteria items work on development server (following the created checklist)

    - Cover all the points in description & comment of user story if any.

    - No error logs in the log file

    - Cover side effect cases

    - Create a bug task if any
