---
title: "MongoDB Query Performance Testing with and without Indexes"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48582066261/MongoDB+Query+Performance+Testing+with+and+without+Indexes
space: "LUZ"
topic: infra
relevance: 0.706
depth: 2.38
updated: 2025-07-18
attachments: 10
tags:
  - confluence
  - infra
  - space/luz
---

# MongoDB Query Performance Testing with and without Indexes

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-07-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48582066261/MongoDB+Query+Performance+Testing+with+and+without+Indexes)
> Relevance 0.706 · topic `infra`

### Purpose

This document presents a performance comparison between MongoDB queries executed **with indexes** and **without indexes** on fields frequently used for sorting:

- `_createdDate`

- `_updatedDate`

- `documentReferenceDate`

- `documentTitle`

The goal is to demonstrate that **applying appropriate indexes on sorting fields** significantly improves query performance and prevents resource-related issues such as exceeding memory limits.

------------------------------------------------------------------------

### ⚙️ Test Setup

- **Database Tool:** MongoDB Compass

- **Test Collection Size:** ~433.000 documents *- TenantID:* 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a

- **Environment:** Dev-vn

- **Query Operation:** Aggregation with `$sort` on the mentioned fields

- **Indexing Approach:**

  - **Test A (With Index):** Single-field indexes created on the sorting fields

  - **Test B (Without Index):** No index applied on sorting fields

------------------------------------------------------------------------

### 📌 Indexes Used in Test A

<div>

|                         |              |
|-------------------------|--------------|
| Field                   | Index Type   |
| `_createdDate`          | Single-field |
| `_updatedDate`          | Single-field |
| `documentReferenceDate` | Single-field |
| `documentTitle`         | Single-field |

</div>

*Indexes were created via MongoDB Compass \> Indexes tab.*


![[48582066261-image-20250716-102831.png]]



  
Query Examples

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="93eec5e5-59a3-48e8-a37f-2053e705b2f3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
  {
    "$sort": {
      "_createdDate": -1
    }
  },
  {
    "$match": {
      "$and": [
        {
          "$or": [
            {
              "securityClassCodes": {
                "$exists": false
              }
            },
            {
              "securityClassCodes": {
                "$size": 0
              }
            },
            {
              "securityClassCodes": {
                "$exists": true,
                "$eq": null
              }
            },
            {
              "securityClassCodes": {
                "$exists": true,
                "$in": []
              }
            }
          ]
        },
        {
          "_isBeingCreated": {
            "$ne": true
          }
        },
        {
          "$or": [
            {
              "_deletionStatus": {
                "$exists": false
              }
            },
            {
              "_deletionStatus": "false"
            }
          ]
        }
      ]
    }
  },
  {
    "$lookup": {
      "from": "folders",
      "let": {
        "folderIds": "$folderIds"
      },
      "pipeline": [
        {
          "$match": {
            "$expr": {
              "$in": [
                {
                  "$toString": "$_id"
                },
                "$$folderIds"
              ]
            }
          }
        },
        {
          "$project": {
            "_id": {
              "$toString": "$_id"
            },
            "name": "$name",
            "securityClassCodes": {
              "$concatArrays": [
                {
                  "$ifNull": [
                    "$securityClassCodes",
                    []
                  ]
                },
                {
                  "$ifNull": [
                    "$inheritedSecurityClassCodes",
                    []
                  ]
                }
              ]
            }
          }
        }
      ],
      "as": "_folders"
    }
  },
  {
    "$addFields": {
      "_folders": "$_folders"
    }
  },
  {
    "$match": {
      "$or": [
        {
          "_folders.securityClassCodes": {
            "$in": []
          }
        },
        {
          "_folders.securityClassCodes": []
        },
        {
          "_folders.securityClassCodes": {
            "$exists": false
          }
        }
      ]
    }
  },
  {
    "$addFields": {
      "_folders": {
        "$filter": {
          "input": "$_folders",
          "cond": {
            "$or": [
              {
                "$gt": [
                  {
                    "$size": {
                      "$setIntersection": [
                        [],
                        {
                          "$ifNull": [
                            "$$this.securityClassCodes",
                            []
                          ]
                        }
                      ]
                    }
                  },
                  0
                ]
              },
              {
                "$eq": [
                  {
                    "$size": {
                      "$ifNull": [
                        "$$this.securityClassCodes",
                        []
                      ]
                    }
                  },
                  0
                ]
              }
            ]
          }
        }
      }
    }
  },
  {
    "$skip": 0
  },
  {
    "$limit": 50
  }
]
```

</div>

</div>

**Result:**


![[48582066261-image-20250718-032551.png]]

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[48582066261-query with index createdDate.wmv|query with index createdDate.wmv]]</span>

- Execution Time: *\< 50ms*

- Query Plan: **IXSCAN** (Index Scan)

- Resource Usage: Efficient, no memory limit error

- Notes: The query utilized the index as expected.

### ❌ Test B – Without Index

Using the same query  
Result:

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[48582066261-query with not index createdDate field.wmv|query with not index createdDate field.wmv]]</span>

- Execution Time: *Can not executed*

- Query Plan: **COLLSCAN** (Collection Scan)

- Resource Usage: Sort exceeded memory limit of 104857600 bytes…

- Notes: The lack of index on `_createdDate` led to inefficient in-memory sort.

### 📊 Comparison Summary

<div>

|              |                |                     |
|--------------|----------------|---------------------|
| **Aspect**   | **With Index** | **Without Index**   |
| Query Speed  | Fast (\< 50ms) | Slow                |
| Query Plan   | IXSCAN         | COLLSCAN            |
| Memory Usage | Optimized      | High, may crash     |
| Scalability  | High           | Poor for large data |

</div>

### Conclusion

Creating indexes on fields used in sorting (e.g., `_createdDate`, `documentTitle`) is critical for ensuring MongoDB query performance and system stability, especially in large datasets.  
In our tests, adding indexes resulted in:

- **Significant reduction in query execution time**

- **Efficient resource usage**

- **Elimination of memory-related query failures**
