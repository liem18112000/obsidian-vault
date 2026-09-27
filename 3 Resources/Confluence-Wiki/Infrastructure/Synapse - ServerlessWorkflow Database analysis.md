---
title: "[Synapse - ServerlessWorkflow] Database analysis"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48908501028/Synapse+-+ServerlessWorkflow+Database+analysis
space: "FUT"
topic: infra
relevance: 0.762
depth: 2.6
updated: 2025-11-27
attachments: 0
tags:
  - confluence
  - infra
  - space/fut
---

# [Synapse - ServerlessWorkflow] Database analysis

> [!info] Imported from Confluence
> Space **FUT** · updated 2025-11-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/48908501028/Synapse+-+ServerlessWorkflow+Database+analysis)
> Relevance 0.762 · topic `infra`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="82ead747-bbf8-46b0-b739-cb2f798f19fb" macro-name="toc">

</div>

# Key findings

- uses **Redis** (via Garnet-compatible API) and stores data in a **NoSQL fashion**, and **Document-Oriented Pattern**

- follows a **resource-oriented architecture** similar to Kubernetes

# Example stored data

<div id="expander-280131763" class="expand-container conf-macro output-block" hasbody="true" macro-id="30b923e0-0278-487c-81bd-8c650ab21c4e" macro-name="expand">

<div id="expander-control-280131763" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Workflow definition</span>

</div>

<div id="expander-content-280131763" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b13c3ec8-f2d2-48dc-8fad-df655c7a09f2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "apiVersion": "synapse.io/v1",
  "kind": "Workflow",
  "metadata": {
    "namespace": "default",
    "name": "order-processing",
    "labels": {
      "app": "ecommerce"
    },
    "annotations": {
      "description": "Process customer orders"
    },
    "creationTimestamp": "2025-11-25T10:30:00Z"
  },
  "spec": {
    "versions": [
      {
        "document": {
          "dsl": "1.0.0-alpha5",
          "namespace": "default",
          "name": "order-processing",
          "version": "1.0.0"
        },
        "do": [
          {
            "validateOrder": {
              "call": "http",
              "with": {
                "method": "POST",
                "endpoint": "https://api.example.com/validate"
              }
            }
          },
          {
            "processPayment": {
              "call": "http",
              "with": {
                "method": "POST",
                "endpoint": "https://api.example.com/payment"
              }
            }
          }
        ]
      }
    ]
  },
  "status": {
    "versions": {
      "1.0.0": {
        "totalInstances": 150,
        "lastStartedAt": "2025-11-25T14:30:00Z",
        "lastEndedAt": "2025-11-25T14:32:15Z"
      }
    }
  }
}
```

</div>

</div>

</div>

</div>

<div id="expander-277268095" class="expand-container conf-macro output-block" hasbody="true" macro-id="a9f25283-9a55-4c07-a5d3-b2250660bad5" macro-name="expand">

<div id="expander-control-277268095" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Workflow instance</span>

</div>

<div id="expander-content-277268095" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="45ee2d74-307b-4fc4-a556-f05a4dbdf119" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "apiVersion": "synapse.io/v1",
  "kind": "WorkflowInstance",
  "metadata": {
    "namespace": "default",
    "name": "order-processing-abc123def",
    "labels": {
      "synapse.io/workflow": "default.order-processing",
      "synapse.io/workflow/version": "1.0.0",
      "synapse.io/operator": "default.operator-1",
      "orderId": "ORD-12345"
    },
    "creationTimestamp": "2025-11-25T14:30:00Z"
  },
  "spec": {
    "definition": {
      "namespace": "default",
      "name": "order-processing",
      "version": "1.0.0"
    },
    "input": {
      "orderId": "ORD-12345",
      "customerId": "CUST-789",
      "items": [
        {
          "productId": "PROD-001",
          "quantity": 2,
          "price": 29.99
        }
      ]
    }
  },
  "status": {
    "phase": "completed",
    "processId": "runner-xyz789",
    "startedAt": "2025-11-25T14:30:01Z",
    "endedAt": "2025-11-25T14:32:15Z",
    "contextReference": "ctx_abc123",
    "outputReference": "out_def456",
    "runs": [
      {
        "startedAt": "2025-11-25T14:30:01Z",
        "endedAt": "2025-11-25T14:32:15Z"
      }
    ],
    "tasks": [
      {
        "id": "task_001",
        "name": "validateOrder",
        "reference": "/do/0/validateOrder",
        "isExtension": false,
        "createdAt": "2025-11-25T14:30:02Z",
        "startedAt": "2025-11-25T14:30:02Z",
        "endedAt": "2025-11-25T14:30:05Z",
        "status": "completed",
        "inputReference": "doc_input_001",
        "contextReference": "doc_ctx_001",
        "outputReference": "doc_out_001",
        "next": "processPayment",
        "runs": [
          {
            "startedAt": "2025-11-25T14:30:02Z",
            "endedAt": "2025-11-25T14:30:05Z",
            "outcome": "completed"
          }
        ]
      },
      {
        "id": "task_002",
        "name": "processPayment",
        "reference": "/do/1/processPayment",
        "isExtension": false,
        "parentId": null,
        "createdAt": "2025-11-25T14:30:06Z",
        "startedAt": "2025-11-25T14:30:06Z",
        "endedAt": "2025-11-25T14:32:10Z",
        "status": "completed",
        "inputReference": "doc_input_002",
        "contextReference": "doc_ctx_002",
        "outputReference": "doc_out_002",
        "next": "end",
        "runs": [
          {
            "startedAt": "2025-11-25T14:30:06Z",
            "endedAt": "2025-11-25T14:32:10Z",
            "outcome": "completed"
          }
        ]
      }
    ]
  }
}
```

</div>

</div>

</div>

</div>

<div id="expander-772824459" class="expand-container conf-macro output-block" hasbody="true" macro-id="cf1b23a0-a857-40f4-afe0-f3e7fc569832" macro-name="expand">

<div id="expander-control-772824459" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Document (used for workflow context/input/output data)</span>

</div>

<div id="expander-content-772824459" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="81e5a5de-eefb-4e1b-b117-7f30e8406f5f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "id": "ctx_abc123",
  "name": "order-processing-abc123def-context",
  "content": {
    "workflow": {
      "definition": {
        "namespace": "default",
        "name": "order-processing",
        "version": "1.0.0"
      }
    },
    "variables": {
      "validationResult": true,
      "paymentId": "PAY-98765",
      "paymentStatus": "confirmed"
    }
  }
}
```

</div>

</div>

</div>

</div>

# What looks good

- NoSQL approach because of the **schemaless** nature of a workflow

- And because of NoSQL approach, every document/record contains as much information as possible:

  - Each workflow definition record contains every version of that workflow

  - Each workflow instance record contains every task instance of that workflow instance (meaning the path of the workflow is stored directly with each workflow instance)

# What maybe not good

- Ensuring atomicity during workflow execution. Synapse currently uses optimistic locking.

- Because a workflow can have many tasks/steps, storing the task path within a workflow instance maybe too much

- Redis/Garnet is not good for data aggregation. For example, we have a usecase to aggregate data like: *"How many times was task 'validate-order' executed across all workflow instances in a 1-month window?*“  

<div id="expander-1739522769" class="expand-container conf-macro output-block" hasbody="true" macro-id="2c6c4f14-b560-42f0-a503-f8da2d0b467e" macro-name="expand">

<div id="expander-control-1739522769" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">AI explanation why Redis/Garnet is not good for data aggregation</span>

</div>

<div id="expander-content-1739522769" class="expand-content expand-hidden">

**Your Analytics Use Case: Task Execution Aggregation**

**Question**: *"How many times was task 'validate-order' executed across all workflow instances in a 1-month window?"*

**With Redis (Current Approach)** - **Challenging**

Redis is **NOT designed for this**. You would need to:

1.  **Scan all WorkflowInstance resources** (using `SCAN` or fetching all keys)

2.  **Deserialize each JSON document**

3.  **Filter and count in application code**

This is inefficient because:

- No native query language for nested JSON

- No indexes on embedded arrays

- O(n) scan of all documents

- High memory usage if loading all instances

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="33d4b6b0-4248-4d81-a19a-4c0113771815" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Pseudo-code of what you'd have to do
var count = 0;
await foreach (var instance in resources.GetAllAsync<WorkflowInstance>())
{
    if (instance.Status?.Tasks != null)
    {
        count += instance.Status.Tasks
            .Where(t => t.Name == "validate-order" 
                     && t.StartedAt >= startDate 
                     && t.StartedAt <= endDate)
            .Count();
    }
}
```

</div>

</div>

**With MongoDB** - **Much Better**

MongoDB excels at this with its **aggregation pipeline**:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2a8ce76d-961f-4be1-9040-514dcd6bc027" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
db.workflowInstances.aggregate([
  { $match: { 
      "status.tasks.name": "validate-order",
      "status.tasks.startedAt": { 
        $gte: ISODate("2025-10-27"), 
        $lt: ISODate("2025-11-27") 
      }
  }},
  { $unwind: "$status.tasks" },
  { $match: { 
      "status.tasks.name": "validate-order",
      "status.tasks.startedAt": { 
        $gte: ISODate("2025-10-27"), 
        $lt: ISODate("2025-11-27") 
      }
  }},
  { $count: "totalExecutions" }
])
```

</div>

</div>

With proper indexes:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="15147fd6-653c-4abf-ba09-d1ff217b7aae" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
db.workflowInstances.createIndex({ 
  "status.tasks.name": 1, 
  "status.tasks.startedAt": 1 
})
```

</div>

</div>

------------------------------------------------------------------------

**Comparison Summary**

<div>

|                        |                         |                      |
|------------------------|-------------------------|----------------------|
| Capability             | Redis (Synapse Current) | MongoDB              |
| Simple CRUD            | ✅ Excellent            | ✅ Good              |
| Store JSON documents   | ✅ Yes (as strings)     | ✅ Native BSON       |
| Query nested fields    | ❌ No (app-side)        | ✅ Native            |
| Aggregations           | ❌ No                   | ✅ Powerful pipeline |
| Time-range queries     | ❌ Manual scan          | ✅ Indexed queries   |
| Pub/Sub                | ✅ Native               | ⚠️ Change Streams    |
| Analytics/Reporting    | ❌ Poor                 | ✅ Excellent         |
| Index on nested arrays | ❌ No                   | ✅ Multikey indexes  |

</div>

------------------------------------------------------------------------

</div>

</div>
