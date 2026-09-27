---
title: "Investigate applying AI to security code review"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49392681040/Investigate+applying+AI+to+security+code+review
space: "TS"
topic: programming
relevance: 0.81
depth: 2.76
updated: 2026-05-11
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# Investigate applying AI to security code review

> [!info] Imported from Confluence
> Space **TS** · updated 2026-05-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49392681040/Investigate+applying+AI+to+security+code+review)
> Relevance 0.81 · topic `programming`

## AI Security Code Review Agent

### Context

Security vulnerabilities were found in the code. Solutions are needed to prevent these issues from recurring in the future.

By integrating AI applications, we aim to accelerate work and increase efficiency.

The idea is to create an agent that scans project code, identifies security bugs, and fixes them before the code is run in any environment.

### Agent

1.  Create an AI code review agent to detect security vulnerabilities.

2.  This agent is responsible for scanning the project’s code to find potential vulnerabilities based on standards and definitions by OWASP, CWE, etc.

3.  Prioritize severity using CVSS scoring.

4.  The agent will include 2 skills:

    - General security skill — based on standards such as OWASP, CWE, CVSS, etc.

    - Project-specific security skill — built from lessons learned from issues previously encountered in the project.

5.  Each finding or issue must be supported by evidence referencing existing vulnerability codes from CWE, OWASP, etc.

### Skills

General security skill:

- We create this skill based on the playbook of OWASP: <a href="https://github.com/OWASP/secure-agent-playbook/tree/main" class="external-link" data-card-appearance="inline" data-local-id="1d9ac8c7-45a5-4647-9cc0-c5aeff1505a2" rel="nofollow">https://github.com/OWASP/secure-agent-playbook/tree/main</a>

- We provide links to reference pages and require the agent to learn from the rules defined on those pages, then update its skill.

Project-specific security skill:

- We create this skill based on our experience from previous fixes of security issues.

### Open questions

- How to keep the agent/skill up to date?

- If there is a contribution to the agent, how can we evaluate whether the current agent version is better than the previous agent version?

- How to integrate the agent into generating secure code during development?

## Demonstration
