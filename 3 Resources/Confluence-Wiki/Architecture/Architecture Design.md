---
ai_hash: 93efe7afa2f7eea6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 53
depth: 2.74
entities: []
relevance: 0.828
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48917053458/Architecture+Design
space: FUT
status: reference
tags:
- confluence
- architecture
- space/fut
title: Architecture Design
topic: architecture
type: source
updated: 2026-07-29
---

# Architecture Design

> [!info] Imported from Confluence
> Space **FUT** · updated 2026-07-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48917053458/Architecture+Design)
> Relevance 0.828 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="61fbc8c7-a533-4dfa-9b6b-cc5453b92f94" macro-name="toc">

</div>

# <span class="inline-comment-marker" ref="cb9f815c-0d37-4584-b7ee-f4af04c51d30">Overview</span>

# C4 overview

### System Context Diagram


![[48917053458-Serverless Workflow high level architecture.png]]



### Dynamic Diagram (collaboration style)


![[48917053458-Serverless Workflow Design Free-Form Diagram.png]]



### Component level


![[48917053458-component-level.png]]



#### Engine component


![[48917053458-WorkflowEngine - High level Design.png]]



#### Diagrams for Task

Task Finite State Machine (FSM)


![[48917053458-Task State Transition.png]]



Context, error handling, event


![[48917053458-Context, error handling, event.png]]



#### Diagram for Workflow

Finite State Machine


![[48917053458-workflow-instance-FSM.png]]



### Flow to dispatch a task instance and receive its result


![[48917053458-dispatch-task-and-receive-result.png]]



# Components

**luz-workflow-manager**: **Manage workflow definition and workflow lifecycle, decide when the workflow start or resume**

- manage workflow definition (json/yaml)

- manage workflow instance (tasks, input, output, states,…)

- start workflow (collect data and send event to run workflow instance)

- resume workflow (consume event callback and send event to resume workflow instance)

**luz-workflow-engine: Execute the workflow tasks and manage task data**

- execute workflow (workflow definition parser, condition/variable conversion, execute tasks, task orchestration, data mapping, ..)

- update workflow task data (input, output, status,..)

- notify the workflow manager when workflow’s tasks complete or pending

**Notes**

- luz-workflow-engine component can scale from 0

- luz-workflow-manager component runs always

- prefer using pubsub push to pull mode as the communication between components (use API endpoint for communicating)

- Each task mostly is short running task (call and wait)

- Data stored in tenant schema

# Interfaces

<div id="expander-1611448814" class="expand-container conf-macro output-block" hasbody="true" macro-id="d53033f1-6e53-4b8b-851a-348ea1270312" macro-name="expand">

<div id="expander-control-1611448814" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-1611448814" class="expand-content expand-hidden">

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
<th><p><strong>Component</strong></p></th>
<th><p><strong>API</strong></p></th>
<th><p><strong>Desciption</strong></p></th>
<th><p><strong>Input</strong></p></th>
</tr>
&#10;<tr>
<td rowspan="5"><p>luz-workflow manager</p></td>
<td><p>POST /workflow-definition</p></td>
<td><p>add workflow definition</p>
<ul>
<li><p>add new workflow definition</p></li>
<li><p>add new version of workflow definition</p></li>
</ul></td>
<td><div id="expander-1782020457" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="450ac554-6d9d-46e2-9bb5-574525780a0b" data-macro-name="expand">
<div id="expander-control-1782020457" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">e.g.</span>
</div>
<div id="expander-content-1782020457" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a69dff03-5575-4319-ad91-0549dbde3825" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;workflowDefinitionId&quot;: null, //OR &quot;existing id&quot;
  &quot;workflowName&quot;: &quot;smartsend&quot;,
  &quot;definitionType&quot;: &quot;yaml&quot;,
  &quot;workflowDefinition&quot;: {
          document:
          dsl: &#39;1.0.0&#39;
          namespace: default
          name: call-http
          version: &#39;1.0.0&#39;
        do:
        - getPet:
            call: http
            with:
              method: get
              endpoint: https://petstore.swagger.io/v2/pet/{petId}&quot;
      }
  }</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p>GET /workflow-definition/{id}</p></td>
<td><p>get workflow definition</p></td>
<td></td>
</tr>
<tr>
<td><p>POST /workflow-instance</p></td>
<td><p>start or resume workflow instance</p>
<ul>
<li><p>receive callback request</p></li>
<li><p>handle workflow timeout</p></li>
<li><p>retry workflow instance</p></li>
</ul></td>
<td><div id="expander-139214100" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="3e21a722-4609-4488-85a2-21e7c0cfc2bf" data-macro-name="expand">
<div id="expander-control-139214100" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">e.g. (cloud event)</span>
</div>
<div id="expander-content-139214100" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ca17c10e-cf63-415c-8f84-ca74e2a14ff6" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;businessData&quot;:{
    &quot;contentType&quot;: &quot;application/pdf&quot;,
    &quot;messageId&quot;: &quot;id&quot;
  },
  &quot;workflowData&quot;: {
    &quot;workflowInstanceId&quot;: &quot;id&quot;,
    &quot;taskName&quot;: &quot;validatePdf&quot;
  }
}</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p>PUT/PATCH /workflow-instance/{id}</p></td>
<td><p>update workflow instance data</p></td>
<td><div id="expander-80979913" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="a2d0d0be-04cd-45d0-b836-024b7045062d" data-macro-name="expand">
<div id="expander-control-80979913" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">e.g.</span>
</div>
<div id="expander-content-80979913" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f2b342e9-601b-4795-b15f-23c5f134a654" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;patchData&quot;:{
    &quot;status&quot;: &quot;ERROR&quot;,
    &quot;output&quot;:{
      &quot;message&quot;: &quot;INVALID_DATA&quot;
    }
  }
}</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p>GET /workflow-instance/{id}</p></td>
<td><p>get workflow instance</p></td>
<td></td>
</tr>
<tr>
<td rowspan="3"><p>luz-workflow-engine</p></td>
<td><p>POST /workflow-task</p></td>
<td><p>add workflow task data</p></td>
<td><div id="expander-1025922776" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="9898a209-0bc8-4dcd-8ba3-99540acb67ef" data-macro-name="expand">
<div id="expander-control-1025922776" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">e.g.</span>
</div>
<div id="expander-content-1025922776" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e8254b10-b65b-4985-8178-420d2d7e51b1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;workflow&quot;:{
    &quot;workflowInstanceId&quot;: &quot;id&quot;
  },
  &quot;taskData&quot;:{
    &quot;task&quot;: &quot;validatePdf&quot;
  }
}</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p>GET /workflow-task/{id}</p></td>
<td><p>get workflow task data</p></td>
<td></td>
</tr>
<tr>
<td><p>PUT/PATCH /workflow-task</p></td>
<td><p>update workflow task data</p></td>
<td><div id="expander-1902788547" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="b40e7974-d509-41cc-bd8c-9607a7a4d382" data-macro-name="expand">
<div id="expander-control-1902788547" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">e.g.</span>
</div>
<div id="expander-content-1902788547" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="24dac9f9-918b-4a32-8ff0-28ecd868445e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;patchData&quot;:{
    &quot;status&quot;: &quot;SUCCESS&quot;,
    &quot;output&quot;:{
      &quot;isValidPdf&quot;: &quot;true&quot;
    }
  }
}</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td></td>
<td><p>POST /run</p></td>
<td><p>start execute workflow tasks</p></td>
<td><div id="expander-1637789185" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="87564cd4-469c-4f87-970e-99199b7aed29" data-macro-name="expand">
<div id="expander-control-1637789185" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">e.g. (cloud event)</span>
</div>
<div id="expander-content-1637789185" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="14a9befb-b916-4013-a464-8504414c5fa7" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;businessData&quot;:{
    &quot;contentType&quot;: &quot;application/pdf&quot;,
    &quot;messageId&quot;: &quot;id&quot;
  },
  &quot;workflowData&quot;: { //resume
    &quot;workflowInstanceId&quot;: &quot;id&quot;,
    &quot;taskName&quot;: &quot;validatePdf&quot;
  }
}</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

# Flow diagrams

## Add workflow definition

Workflow definitions use YAML or JSON to specify workflows and tasks, detailing each task's actions and sequence.

Define this before starting a new workflow instance in the dashboard or via the exposed API. The workflow definition is stored in the database and can be referenced by multiple workflow instances during execution.

The workflow manager stores and updates (as new versions) workflow definitions.


![[48917053458-add-workflow-definition.png]]



## Start workflow instance

After defining the workflow, the user can start a new instance via the dashboard or API.

The workflow manager creates a new record for the instance data and triggers an event to run the workflow.

The workflow engine receives the event and manages execution, including parsing, data binding, task execution (e.g., calling services via HTTP), verifying results, and performing task transitions. During this process, the workflow engine updates the data of task to the database.

After executing all tasks, it triggers the “Finish workflow instance” flow. Otherwise, the workflow instance suspends and stops the process.


![[48917053458-start-workflow.png]]



## Finish workflow instance

The workflow engine finished the task execution in the workflow instance, start to finish the workflow by sending an event to Google pubsub.

The workflow manager receives the event and start to update the final result to corresponding workflow instance to the database.


![[48917053458-complete-workflow.png]]



## Resume workflow instance

During task execution, the workflow instance can suspend for reasons such as waiting for services to return results (e.g., calling async APIs). In this state, the workflow instance is SUSPENDED and awaits a resume event.

When services return results to the waiting task, the workflow manager receives the event and resumes the workflow instance.

The workflow manager sends an event to start the suspended workflow instance. The workflow engine receives this event and runs the instance.

Here, the workflow engine continues at the waiting task and starts the workflow as in the “Start workflow instance” flow.


![[48917053458-resume-workflow.png]]

%% ai-graph-start %%

**Related notes:**
- [[Synapse - ServerlessWorkflow Database analysis]]
- [[OneAPI Architecture overview]]
- [[Batching Design]]
- [[Performance pain points]]
- [[Luz Batch TypeScript - Sequence Diagram]]

%% ai-graph-end %%