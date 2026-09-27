---
title: "Authentication & Authorization Architecture"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48615161915/Authentication+Authorization+Architecture
space: "FUT"
topic: security
relevance: 0.886
depth: 3
updated: 2025-08-18
attachments: 7
tags:
  - confluence
  - security
  - space/fut
---

# Authentication & Authorization Architecture

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-08-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48615161915/Authentication+Authorization+Architecture)
> Relevance 0.886 · topic `security`

![[48615161915-Authentication and Authorization.png]]



<div id="expander-437664612" class="expand-container conf-macro output-block" hasbody="true" macro-id="45f36ed7-e64a-45f6-8135-9426afa13be9" macro-name="expand">

<div id="expander-control-437664612" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Internal Authenticate and Authorization</span>

</div>

<div id="expander-content-437664612" class="expand-content expand-hidden">

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Role-Permission</strong></p></th>
<th><p><strong>API authentication</strong></p></th>
<th><p><strong>API authorization</strong></p></th>
<th><p><strong>Use cases</strong></p></th>
</tr>
&#10;<tr>
<td><ul>
<li><p>user role</p>
<ul>
<li><p>user tenant</p></li>
<li><p>admin/cron user</p></li>
</ul></li>
<li><p>permissions</p>
<ul>
<li><p>business-based (eg. FINANCE_CREDIT_CARD_READ_ONLY)</p></li>
<li><p>module-based (eg. LUZ_DOCS_VIEW_CONTROLER)</p></li>
</ul></li>
</ul>

![[48615161915-role-permission.png]]

</td>
<td><p>by default</p>
<p>OR</p>
<p><code>@PermitAll</code></p>
<p>→ on jwt-service</p></td>
<td>

![[48615161915-image-20250815-100906.png]]


<p><code>@AccessibleWithoutTenan</code>→ no authorized</p>
<p><code>@PermissionAllowed</code> → permission-based authorization</p>
<p>RunAsToken → no authorized</p>
<p>→ on luzsec_service or luz-jwt (library)</p></td>
<td>

![[48615161915-image-20250818-041853.png]]

</td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

## What is Role-based access control RBAC?

Role-based access control (RBAC) restricts system access to authorized users based on their roles within an organization or a system.

RBAC is coarse-grained authorization. In RBAC, a user is assigned a role (e.g., `manager`, `employee`, `auditor`). This role is then granted a broad set of permissions (e.g., "all managers can view all employee records"). Every user with that role gets the exact same access.

## What is Attribute-based access control ABAC?

Attribute-Based Access Control (ABAC) is an authorization model that makes access decisions by evaluating a set of attributes related to the user, the resource, the action, and the environment.

- **User Attributes:** Who is the user? (e.g., `role: "manager"`, `department: "finance"`, `clearance_level: "top_secret"`)

- **Resource Attributes:** What is the user trying to access? (e.g., `file_type: "document"`, `data_sensitivity: "confidential"`, `owner: "john.doe"`)

- **Action Attributes:** What is the user trying to do? (e.g., `action: "read"`, `action: "write"`, `action: "delete"`)

- **Environmental Attributes:** What is the context of the access request? (e.g., `time_of_day: "9am-5pm"`, `network_location: "corporate_vpn"`, `device_type: "mobile"`)

A policy in ABAC is a rule that uses Boolean logic to combine these attributes. For example: "A user can **read** a **confidential** document if their **department** is **finance** and the **time of day** is **between 9 AM and 5 PM**."

ABAC is fine-grained authorization.

Authorization architecture

## What is Authorization Architecture?

Authorization architecture is the system and infrastructure that enforces an authorization model. It defines how policies are stored, evaluated, and enforced to determine what an authenticated user or service is allowed to do.

### Key Components

- **Policy Administration Point (PAP)**: This is where policies are defined, stored, and managed. Think of it as a central rulebook. Policies can be written in a human-readable language (like Rego for Open Policy Agent) and stored in a version control system like Git.

- **Policy Decision Point (PDP)**: This is the "brain" of the architecture. The PDP evaluates an access request against the policies from the PAP and provides a decision, such as `permit` or `deny`. A standalone PDP service is often used as a best practice to decouple the decision logic from the application.

- **Policy Enforcement Point (PEP)**: The PEP is the code or service that actually enforces the PDP's decision. It's the point of contact between the user and the resource. For example, a PEP could be a piece of middleware in a web application that blocks a request if the PDP returns `deny`.

- **Policy Information Point (PIP)**: This component provides the PDP with additional information (attributes) needed to make a decision. The PIP can pull data from various sources, such as a user directory, a database, or even the time of day, to enrich the context of an access request.

## What is Open Policy Agent?

Open Policy Agent (OPA), pronounced "oh-pa," is an open-source, general-purpose **policy engine**. Its main purpose is to **decouple policy decisions from your application code**, allowing you to manage and enforce rules consistently across your entire technology stack.

### Key Concepts

- **Policy as Code:** OPA uses a declarative language called **Rego** to define policies. This means you write your rules as code, which can be stored in a Git repository, versioned, and tested just like any other code.

- **Centralized Decision-Making:** OPA acts as a standalone service (the **Policy Decision Point, or PDP**). Your application (the **Policy Enforcement Point, or PEP**) sends an authorization request to OPA, which then evaluates the request against its policies and returns a decision.

- **General-Purpose Engine:** While commonly used for authorization, OPA is not limited to it. Its flexible design allows it to be used for a wide range of policy-related tasks, such as:

  - **Kubernetes Admission Control:** Ensuring new resources meet security standards.

  - **API Authorization:** Deciding if a user can access a specific endpoint.

  - **CI/CD Pipelines:** Gating deployments based on compliance rules.
