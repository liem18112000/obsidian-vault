---
title: "[luz-docs] - MongoDB aggregate slow query analyze"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48197271878/luz-docs+-+MongoDB+aggregate+slow+query+analyze
space: "LUZ"
topic: infra
relevance: 0.714
depth: 2.45
updated: 2025-07-11
attachments: 14
tags:
  - confluence
  - infra
  - space/luz
---

# [luz-docs] - MongoDB aggregate slow query analyze

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-07-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48197271878/luz-docs+-+MongoDB+aggregate+slow+query+analyze)
> Relevance 0.714 · topic `infra`

Preferences page:

- [Investigate performance for Large Volume Tenants (e.g., over 20,000 documents) in ePost Digital Letterbox](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48163652839/Investigate+performance+for+Large+Volume+Tenants+e.g.+over+20+000+documents+in+ePost+Digital+Letterbox)

- [\[luz-docs\] - Optimizing MongoDB Collections for Enhanced Performance](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/48207069373/luz-docs+-+Optimizing+MongoDB+Collections+for+Enhanced+Performance)

------------------------------------------------------------------------

# Preparation Steps

1.  Set the DEV-VN context, use the following command

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1387f9a5-3701-4a1b-86c7-c7bc61ee6801" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    kubectl config use-context gke_klara-nonprod_asia-southeast1-a_klara-dev-vn
    ```

    </div>

    </div>

2.  Port-forward MongoDB using this command

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4e561cd3-c723-48e5-af90-a4668febb955" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    kubectl port-forward svc/luz-mongodb01-cluster-mongos --address 0.0.0.0 27017:27017 -n dev-vn-mongodb01
    ```

    </div>

    </div>

3.  <a href="https://downloads.mongodb.com/compass/mongodb-compass-1.45.0-win32-x64.exe" class="external-link" rel="nofollow">Download and install MongoDB Compass (GUI)</a>

4.  Connect to the KLARA Business AG Tenant MongoDB, use the following connection string

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6f454eb7-48e3-43aa-808b-51c71b4c7879" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    mongodb://00a04daf-f2b3-41d5-8c12-2d1b4c48a36a:00a04daf-f2b3-41d5-8c12-2d1b4c48a36a@localhost:27017/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a?authSource=00a04daf-f2b3-41d5-8c12-2d1b4c48a36a&readPreference=primary&appname=MongoDB%20Compass&ssl=false
    ```

    </div>

    </div>

------------------------------------------------------------------------

# Context:

*<span style="background-color: rgb(253,208,236);">The size of the document collection is approximately 352K entries</span>*


![[48197271878-image-20241206-052842.png]]



*<span style="background-color: rgb(253,208,236);">The </span>*`documents`*<span style="background-color: rgb(253,208,236);"> collection currently has a single index, which is based on the </span>*`_id`*<span style="background-color: rgb(253,208,236);"> field in ascending order</span>*


![[48197271878-image-20241206-065004.png]]



# Examine results

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
<th></th>
<th><p><strong>Query</strong></p></th>
<th><p><strong>Collection</strong></p></th>
<th><p><strong>Result</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="0b8990b5-905a-43b5-85f5-97f5fd0af854" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
   {
      &quot;$match&quot;:{
         &quot;$and&quot;:[
            {
               &quot;$or&quot;:[
                  {
                     &quot;isStored&quot;:true
                  },
                  {
                     &quot;folderIds.0&quot;:{
                        &quot;$exists&quot;:true
                     }
                  },
                  {
                     &quot;$and&quot;:[
                        {
                           &quot;$or&quot;:[
                              {
                                 &quot;folderIds&quot;:{
                                    &quot;$size&quot;:0
                                 }
                              },
                              {
                                 &quot;folderIds&quot;:{
                                    &quot;$exists&quot;:false
                                 }
                              }
                           ]
                        },
                        {
                           &quot;origin&quot;:{
                              &quot;$regex&quot;:&quot;^User uploaded$&quot;,
                              &quot;$options&quot;:&quot;i&quot;
                           }
                        }
                     ]
                  }
               ]
            },
            {
               &quot;_isBeingCreated&quot;:{
                  &quot;$ne&quot;:true
               }
            },
            {
               &quot;$or&quot;:[
                  {
                     &quot;_deletionStatus&quot;:{
                        &quot;$exists&quot;:false
                     }
                  },
                  {
                     &quot;_deletionStatus&quot;:&quot;false&quot;
                  }
               ]
            }
         ]
      }
   },
   {
      &quot;$lookup&quot;:{
         &quot;from&quot;:&quot;folders&quot;,
         &quot;let&quot;:{
            &quot;folderIds&quot;:&quot;$folderIds&quot;
         },
         &quot;pipeline&quot;:[
            {
               &quot;$match&quot;:{
                  &quot;$expr&quot;:{
                     &quot;$in&quot;:[
                        {
                           &quot;$toString&quot;:&quot;$_id&quot;
                        },
                        &quot;$$folderIds&quot;
                     ]
                  }
               }
            },
            {
               &quot;$project&quot;:{
                  &quot;_id&quot;:{
                     &quot;$toString&quot;:&quot;$_id&quot;
                  },
                  &quot;name&quot;:&quot;$name&quot;,
                  &quot;securityClassCodes&quot;:{
                     &quot;$concatArrays&quot;:[
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$securityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        },
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$inheritedSecurityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        }
                     ]
                  }
               }
            }
         ],
         &quot;as&quot;:&quot;_folders&quot;
      }
   },
   {
      &quot;$addFields&quot;:{
         &quot;_folders&quot;:&quot;$_folders&quot;
      }
   },
   {
      &quot;$sort&quot;:{
         &quot;_updatedDate&quot;:-1
      }
   },
   {
      &quot;$skip&quot;:0
   },
   {
      &quot;$limit&quot;:48
   },
   {
      &quot;$project&quot;:{
         &quot;_files.referenceTsq&quot;:0,
         &quot;_files.thumbnail512&quot;:0,
         &quot;_files.thumbnail256&quot;:0,
         &quot;uploadedHistoryEntry&quot;:0,
         &quot;storageHistoryEntries&quot;:0,
         &quot;printHistoryEntries&quot;:0,
         &quot;downloadHistoryEntries&quot;:0,
         &quot;exportHistoryEntries&quot;:0,
         &quot;deletingHistoryEntries&quot;:0,
         &quot;undoDeletingHistoryEntries&quot;:0,
         &quot;tagModificationHistoryEntries&quot;:0,
         &quot;securityClassModificationHistoryEntries&quot;:0,
         &quot;documentTypeModificationHistoryEntries&quot;:0,
         &quot;restoringHistoryEntries&quot;:0
      }
   }
]</code></pre>
</div>
</div></td>
<td><p>documents</p></td>
<td><div id="expander-655336301" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="d57dcd2d-1176-45a8-b626-315855100138" data-macro-name="expand">
<div id="expander-control-655336301" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Examine raw ouput</span>
</div>
<div id="expander-content-655336301" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="02367972-8268-4bef-a105-9be4b7d9556e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;serverInfo&quot;: {
    &quot;host&quot;: &quot;luz-mongodb01-cluster-mongos-2&quot;,
    &quot;port&quot;: 27017,
    &quot;version&quot;: &quot;4.4.6-8&quot;,
    &quot;gitVersion&quot;: &quot;8e04f19f85906daef6ef85d97b345fbcaf83f04b&quot;
  },
  &quot;stages&quot;: [
    {
      &quot;$cursor&quot;: {
        &quot;queryPlanner&quot;: {
          &quot;plannerVersion&quot;: 1,
          &quot;namespace&quot;: &quot;00a04daf-f2b3-41d5-8c12-2d1b4c48a36a.documents&quot;,
          &quot;indexFilterSet&quot;: false,
          &quot;parsedQuery&quot;: {
            &quot;$and&quot;: [
              {
                &quot;$or&quot;: [
                  {
                    &quot;$and&quot;: [
                      {
                        &quot;$or&quot;: [
                          {
                            &quot;folderIds&quot;: {
                              &quot;$size&quot;: 0
                            }
                          },
                          {
                            &quot;folderIds&quot;: {
                              &quot;$not&quot;: {
                                &quot;$exists&quot;: true
                              }
                            }
                          }
                        ]
                      },
                      {
                        &quot;origin&quot;: {
                          &quot;$regex&quot;: &quot;^User uploaded$&quot;,
                          &quot;$options&quot;: &quot;i&quot;
                        }
                      }
                    ]
                  },
                  { &quot;isStored&quot;: { &quot;$eq&quot;: true } },
                  {
                    &quot;folderIds.0&quot;: {
                      &quot;$exists&quot;: true
                    }
                  }
                ]
              },
              {
                &quot;$or&quot;: [
                  {
                    &quot;_deletionStatus&quot;: {
                      &quot;$eq&quot;: &quot;false&quot;
                    }
                  },
                  {
                    &quot;_deletionStatus&quot;: {
                      &quot;$not&quot;: { &quot;$exists&quot;: true }
                    }
                  }
                ]
              },
              {
                &quot;_isBeingCreated&quot;: {
                  &quot;$not&quot;: { &quot;$eq&quot;: true }
                }
              }
            ]
          },
          &quot;queryHash&quot;: &quot;FCB0D80B&quot;,
          &quot;planCacheKey&quot;: &quot;3510C9EF&quot;,
          &quot;winningPlan&quot;: {
            &quot;stage&quot;: &quot;COLLSCAN&quot;,
            &quot;filter&quot;: {
              &quot;$and&quot;: [
                {
                  &quot;$or&quot;: [
                    {
                      &quot;$and&quot;: [
                        {
                          &quot;$or&quot;: [
                            {
                              &quot;folderIds&quot;: {
                                &quot;$size&quot;: 0
                              }
                            },
                            {
                              &quot;folderIds&quot;: {
                                &quot;$not&quot;: {
                                  &quot;$exists&quot;: true
                                }
                              }
                            }
                          ]
                        },
                        {
                          &quot;origin&quot;: {
                            &quot;$regex&quot;: &quot;^User uploaded$&quot;,
                            &quot;$options&quot;: &quot;i&quot;
                          }
                        }
                      ]
                    },
                    {
                      &quot;isStored&quot;: { &quot;$eq&quot;: true }
                    },
                    {
                      &quot;folderIds.0&quot;: {
                        &quot;$exists&quot;: true
                      }
                    }
                  ]
                },
                {
                  &quot;$or&quot;: [
                    {
                      &quot;_deletionStatus&quot;: {
                        &quot;$eq&quot;: &quot;false&quot;
                      }
                    },
                    {
                      &quot;_deletionStatus&quot;: {
                        &quot;$not&quot;: {
                          &quot;$exists&quot;: true
                        }
                      }
                    }
                  ]
                },
                {
                  &quot;_isBeingCreated&quot;: {
                    &quot;$not&quot;: { &quot;$eq&quot;: true }
                  }
                }
              ]
            },
            &quot;direction&quot;: &quot;forward&quot;
          },
          &quot;rejectedPlans&quot;: []
        },
        &quot;executionStats&quot;: {
          &quot;executionSuccess&quot;: true,
          &quot;nReturned&quot;: 111581,
          &quot;executionTimeMillis&quot;: 240744,
          &quot;totalKeysExamined&quot;: 0,
          &quot;totalDocsExamined&quot;: 354897,
          &quot;executionStages&quot;: {
            &quot;stage&quot;: &quot;COLLSCAN&quot;,
            &quot;filter&quot;: {
              &quot;$and&quot;: [
                {
                  &quot;$or&quot;: [
                    {
                      &quot;$and&quot;: [
                        {
                          &quot;$or&quot;: [
                            {
                              &quot;folderIds&quot;: {
                                &quot;$size&quot;: 0
                              }
                            },
                            {
                              &quot;folderIds&quot;: {
                                &quot;$not&quot;: {
                                  &quot;$exists&quot;: true
                                }
                              }
                            }
                          ]
                        },
                        {
                          &quot;origin&quot;: {
                            &quot;$regex&quot;: &quot;^User uploaded$&quot;,
                            &quot;$options&quot;: &quot;i&quot;
                          }
                        }
                      ]
                    },
                    {
                      &quot;isStored&quot;: { &quot;$eq&quot;: true }
                    },
                    {
                      &quot;folderIds.0&quot;: {
                        &quot;$exists&quot;: true
                      }
                    }
                  ]
                },
                {
                  &quot;$or&quot;: [
                    {
                      &quot;_deletionStatus&quot;: {
                        &quot;$eq&quot;: &quot;false&quot;
                      }
                    },
                    {
                      &quot;_deletionStatus&quot;: {
                        &quot;$not&quot;: {
                          &quot;$exists&quot;: true
                        }
                      }
                    }
                  ]
                },
                {
                  &quot;_isBeingCreated&quot;: {
                    &quot;$not&quot;: { &quot;$eq&quot;: true }
                  }
                }
              ]
            },
            &quot;nReturned&quot;: 111581,
            &quot;executionTimeMillisEstimate&quot;: 198223,
            &quot;works&quot;: 354899,
            &quot;advanced&quot;: 111581,
            &quot;needTime&quot;: 243317,
            &quot;needYield&quot;: 0,
            &quot;saveState&quot;: 8176,
            &quot;restoreState&quot;: 8176,
            &quot;isEOF&quot;: 1,
            &quot;direction&quot;: &quot;forward&quot;,
            &quot;docsExamined&quot;: 354897
          }
        }
      },
      &quot;nReturned&quot;: 111581,
      &quot;executionTimeMillisEstimate&quot;: 230553
    },
    {
      &quot;$lookup&quot;: {
        &quot;from&quot;: &quot;folders&quot;,
        &quot;as&quot;: &quot;_folders&quot;,
        &quot;let&quot;: { &quot;folderIds&quot;: &quot;$folderIds&quot; },
        &quot;pipeline&quot;: [
          {
            &quot;$match&quot;: {
              &quot;$expr&quot;: {
                &quot;$in&quot;: [
                  { &quot;$toString&quot;: &quot;$_id&quot; },
                  &quot;$$folderIds&quot;
                ]
              }
            }
          },
          {
            &quot;$project&quot;: {
              &quot;_id&quot;: { &quot;$toString&quot;: &quot;$_id&quot; },
              &quot;name&quot;: &quot;$name&quot;,
              &quot;securityClassCodes&quot;: {
                &quot;$concatArrays&quot;: [
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$securityClassCodes&quot;,
                      []
                    ]
                  },
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$inheritedSecurityClassCodes&quot;,
                      []
                    ]
                  }
                ]
              }
            }
          }
        ]
      },
      &quot;nReturned&quot;: 111581,
      &quot;executionTimeMillisEstimate&quot;: 240697
    },
    {
      &quot;$addFields&quot;: { &quot;_folders&quot;: &quot;$_folders&quot; },
      &quot;nReturned&quot;: 111581,
      &quot;executionTimeMillisEstimate&quot;: 240707
    },
    {
      &quot;$sort&quot;: {
        &quot;sortKey&quot;: { &quot;_updatedDate&quot;: -1 },
        &quot;limit&quot;: 48
      },
      &quot;nReturned&quot;: 48,
      &quot;executionTimeMillisEstimate&quot;: 240744
    },
    {
      &quot;$project&quot;: {
        &quot;deletingHistoryEntries&quot;: false,
        &quot;undoDeletingHistoryEntries&quot;: false,
        &quot;tagModificationHistoryEntries&quot;: false,
        &quot;documentTypeModificationHistoryEntries&quot;: false,
        &quot;storageHistoryEntries&quot;: false,
        &quot;exportHistoryEntries&quot;: false,
        &quot;restoringHistoryEntries&quot;: false,
        &quot;downloadHistoryEntries&quot;: false,
        &quot;uploadedHistoryEntry&quot;: false,
        &quot;securityClassModificationHistoryEntries&quot;: false,
        &quot;printHistoryEntries&quot;: false,
        &quot;_files&quot;: {
          &quot;referenceTsq&quot;: false,
          &quot;thumbnail512&quot;: false,
          &quot;thumbnail256&quot;: false
        }
      },
      &quot;nReturned&quot;: 48,
      &quot;executionTimeMillisEstimate&quot;: 240744
    }
  ],
  &quot;ok&quot;: 1,
  &quot;operationTime&quot;: {
    &quot;$timestamp&quot;: &quot;7447043889023680513&quot;
  },
  &quot;$clusterTime&quot;: {
    &quot;clusterTime&quot;: {
      &quot;$timestamp&quot;: &quot;7447043919088451586&quot;
    },
    &quot;signature&quot;: {
      &quot;hash&quot;: &quot;ajviw3iRLgReA287yWzeJf1NRms=&quot;,
      &quot;keyId&quot;: {
        &quot;low&quot;: 2,
        &quot;high&quot;: 1721435263,
        &quot;unsigned&quot;: false
      }
    }
  }
}</code></pre>
</div>
</div>
</div>
</div>

![[48197271878-image-20241211-065751.png]]



![[48197271878-image-20241211-065806.png]]

</td>
</tr>
<tr>
<td>2</td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="3726bdaf-1c1d-414c-9ec7-b1c9005ad9e1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
   {
      &quot;$match&quot;:{
         &quot;$and&quot;:[
            {
               &quot;$or&quot;:[
                  {
                     &quot;isStored&quot;:true
                  },
                  {
                     &quot;folderIds.0&quot;:{
                        &quot;$exists&quot;:true
                     }
                  },
                  {
                     &quot;$and&quot;:[
                        {
                           &quot;$or&quot;:[
                              {
                                 &quot;folderIds&quot;:{
                                    &quot;$size&quot;:0
                                 }
                              },
                              {
                                 &quot;folderIds&quot;:{
                                    &quot;$exists&quot;:false
                                 }
                              }
                           ]
                        },
                        {
                           &quot;origin&quot;:{
                              &quot;$regex&quot;:&quot;^User uploaded$&quot;,
                              &quot;$options&quot;:&quot;i&quot;
                           }
                        }
                     ]
                  }
               ]
            },
            {
               &quot;_isBeingCreated&quot;:{
                  &quot;$ne&quot;:true
               }
            },
            {
               &quot;$or&quot;:[
                  {
                     &quot;_deletionStatus&quot;:{
                        &quot;$exists&quot;:false
                     }
                  },
                  {
                     &quot;_deletionStatus&quot;:&quot;false&quot;
                  }
               ]
            }
         ]
      }
   },
   {
      &quot;$lookup&quot;:{
         &quot;from&quot;:&quot;folders&quot;,
         &quot;let&quot;:{
            &quot;folderIds&quot;:&quot;$folderIds&quot;
         },
         &quot;pipeline&quot;:[
            {
               &quot;$match&quot;:{
                  &quot;$expr&quot;:{
                     &quot;$in&quot;:[
                        {
                           &quot;$toString&quot;:&quot;$_id&quot;
                        },
                        &quot;$$folderIds&quot;
                     ]
                  }
               }
            },
            {
               &quot;$project&quot;:{
                  &quot;_id&quot;:{
                     &quot;$toString&quot;:&quot;$_id&quot;
                  },
                  &quot;name&quot;:&quot;$name&quot;,
                  &quot;securityClassCodes&quot;:{
                     &quot;$concatArrays&quot;:[
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$securityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        },
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$inheritedSecurityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        }
                     ]
                  }
               }
            }
         ],
         &quot;as&quot;:&quot;_folders&quot;
      }
   },
   {
      &quot;$addFields&quot;:{
         &quot;_folders&quot;:&quot;$_folders&quot;
      }
   },
   {
      &quot;$count&quot;:&quot;totalRecordCount&quot;
   }
]</code></pre>
</div>
</div></td>
<td><p>documents</p></td>
<td><div id="expander-808912933" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="a86926b6-a7b3-4ca4-8084-2c3db4e6d32d" data-macro-name="expand">
<div id="expander-control-808912933" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Examine raw ouput</span>
</div>
<div id="expander-content-808912933" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="0fd1101a-b4a7-434b-a56f-a5ade55e8876" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;serverInfo&quot;: {
    &quot;host&quot;: &quot;luz-mongodb01-cluster-mongos-2&quot;,
    &quot;port&quot;: 27017,
    &quot;version&quot;: &quot;4.4.6-8&quot;,
    &quot;gitVersion&quot;: &quot;8e04f19f85906daef6ef85d97b345fbcaf83f04b&quot;
  },
  &quot;stages&quot;: [
    {
      &quot;$cursor&quot;: {
        &quot;queryPlanner&quot;: {
          &quot;plannerVersion&quot;: 1,
          &quot;namespace&quot;: &quot;00a04daf-f2b3-41d5-8c12-2d1b4c48a36a.documents&quot;,
          &quot;indexFilterSet&quot;: false,
          &quot;parsedQuery&quot;: {
            &quot;$and&quot;: [
              {
                &quot;$or&quot;: [
                  {
                    &quot;$and&quot;: [
                      {
                        &quot;$or&quot;: [
                          {
                            &quot;folderIds&quot;: {
                              &quot;$size&quot;: 0
                            }
                          },
                          {
                            &quot;folderIds&quot;: {
                              &quot;$not&quot;: {
                                &quot;$exists&quot;: true
                              }
                            }
                          }
                        ]
                      },
                      {
                        &quot;origin&quot;: {
                          &quot;$regex&quot;: &quot;^User uploaded$&quot;,
                          &quot;$options&quot;: &quot;i&quot;
                        }
                      }
                    ]
                  },
                  { &quot;isStored&quot;: { &quot;$eq&quot;: true } },
                  {
                    &quot;folderIds.0&quot;: {
                      &quot;$exists&quot;: true
                    }
                  }
                ]
              },
              {
                &quot;$or&quot;: [
                  {
                    &quot;_deletionStatus&quot;: {
                      &quot;$eq&quot;: &quot;false&quot;
                    }
                  },
                  {
                    &quot;_deletionStatus&quot;: {
                      &quot;$not&quot;: { &quot;$exists&quot;: true }
                    }
                  }
                ]
              },
              {
                &quot;_isBeingCreated&quot;: {
                  &quot;$not&quot;: { &quot;$eq&quot;: true }
                }
              }
            ]
          },
          &quot;queryHash&quot;: &quot;216BC599&quot;,
          &quot;planCacheKey&quot;: &quot;6273EA82&quot;,
          &quot;winningPlan&quot;: {
            &quot;stage&quot;: &quot;PROJECTION_SIMPLE&quot;,
            &quot;transformBy&quot;: {
              &quot;_folders&quot;: 1,
              &quot;folderIds&quot;: 1,
              &quot;_id&quot;: 0
            },
            &quot;inputStage&quot;: {
              &quot;stage&quot;: &quot;COLLSCAN&quot;,
              &quot;filter&quot;: {
                &quot;$and&quot;: [
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;$or&quot;: [
                              {
                                &quot;folderIds&quot;: {
                                  &quot;$size&quot;: 0
                                }
                              },
                              {
                                &quot;folderIds&quot;: {
                                  &quot;$not&quot;: {
                                    &quot;$exists&quot;: true
                                  }
                                }
                              }
                            ]
                          },
                          {
                            &quot;origin&quot;: {
                              &quot;$regex&quot;: &quot;^User uploaded$&quot;,
                              &quot;$options&quot;: &quot;i&quot;
                            }
                          }
                        ]
                      },
                      {
                        &quot;isStored&quot;: {
                          &quot;$eq&quot;: true
                        }
                      },
                      {
                        &quot;folderIds.0&quot;: {
                          &quot;$exists&quot;: true
                        }
                      }
                    ]
                  },
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;_deletionStatus&quot;: {
                          &quot;$eq&quot;: &quot;false&quot;
                        }
                      },
                      {
                        &quot;_deletionStatus&quot;: {
                          &quot;$not&quot;: {
                            &quot;$exists&quot;: true
                          }
                        }
                      }
                    ]
                  },
                  {
                    &quot;_isBeingCreated&quot;: {
                      &quot;$not&quot;: { &quot;$eq&quot;: true }
                    }
                  }
                ]
              },
              &quot;direction&quot;: &quot;forward&quot;
            }
          },
          &quot;rejectedPlans&quot;: []
        },
        &quot;executionStats&quot;: {
          &quot;executionSuccess&quot;: true,
          &quot;nReturned&quot;: 111581,
          &quot;executionTimeMillis&quot;: 306323,
          &quot;totalKeysExamined&quot;: 0,
          &quot;totalDocsExamined&quot;: 354897,
          &quot;executionStages&quot;: {
            &quot;stage&quot;: &quot;PROJECTION_SIMPLE&quot;,
            &quot;nReturned&quot;: 111581,
            &quot;executionTimeMillisEstimate&quot;: 215406,
            &quot;works&quot;: 354899,
            &quot;advanced&quot;: 111581,
            &quot;needTime&quot;: 243317,
            &quot;needYield&quot;: 0,
            &quot;saveState&quot;: 9596,
            &quot;restoreState&quot;: 9596,
            &quot;isEOF&quot;: 1,
            &quot;transformBy&quot;: {
              &quot;_folders&quot;: 1,
              &quot;folderIds&quot;: 1,
              &quot;_id&quot;: 0
            },
            &quot;inputStage&quot;: {
              &quot;stage&quot;: &quot;COLLSCAN&quot;,
              &quot;filter&quot;: {
                &quot;$and&quot;: [
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;$or&quot;: [
                              {
                                &quot;folderIds&quot;: {
                                  &quot;$size&quot;: 0
                                }
                              },
                              {
                                &quot;folderIds&quot;: {
                                  &quot;$not&quot;: {
                                    &quot;$exists&quot;: true
                                  }
                                }
                              }
                            ]
                          },
                          {
                            &quot;origin&quot;: {
                              &quot;$regex&quot;: &quot;^User uploaded$&quot;,
                              &quot;$options&quot;: &quot;i&quot;
                            }
                          }
                        ]
                      },
                      {
                        &quot;isStored&quot;: {
                          &quot;$eq&quot;: true
                        }
                      },
                      {
                        &quot;folderIds.0&quot;: {
                          &quot;$exists&quot;: true
                        }
                      }
                    ]
                  },
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;_deletionStatus&quot;: {
                          &quot;$eq&quot;: &quot;false&quot;
                        }
                      },
                      {
                        &quot;_deletionStatus&quot;: {
                          &quot;$not&quot;: {
                            &quot;$exists&quot;: true
                          }
                        }
                      }
                    ]
                  },
                  {
                    &quot;_isBeingCreated&quot;: {
                      &quot;$not&quot;: { &quot;$eq&quot;: true }
                    }
                  }
                ]
              },
              &quot;nReturned&quot;: 111581,
              &quot;executionTimeMillisEstimate&quot;: 215230,
              &quot;works&quot;: 354899,
              &quot;advanced&quot;: 111581,
              &quot;needTime&quot;: 243317,
              &quot;needYield&quot;: 0,
              &quot;saveState&quot;: 9596,
              &quot;restoreState&quot;: 9596,
              &quot;isEOF&quot;: 1,
              &quot;direction&quot;: &quot;forward&quot;,
              &quot;docsExamined&quot;: 354897
            }
          }
        }
      },
      &quot;nReturned&quot;: 111581,
      &quot;executionTimeMillisEstimate&quot;: 285209
    },
    {
      &quot;$lookup&quot;: {
        &quot;from&quot;: &quot;folders&quot;,
        &quot;as&quot;: &quot;_folders&quot;,
        &quot;let&quot;: { &quot;folderIds&quot;: &quot;$folderIds&quot; },
        &quot;pipeline&quot;: [
          {
            &quot;$match&quot;: {
              &quot;$expr&quot;: {
                &quot;$in&quot;: [
                  { &quot;$toString&quot;: &quot;$_id&quot; },
                  &quot;$$folderIds&quot;
                ]
              }
            }
          },
          {
            &quot;$project&quot;: {
              &quot;_id&quot;: { &quot;$toString&quot;: &quot;$_id&quot; },
              &quot;name&quot;: &quot;$name&quot;,
              &quot;securityClassCodes&quot;: {
                &quot;$concatArrays&quot;: [
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$securityClassCodes&quot;,
                      []
                    ]
                  },
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$inheritedSecurityClassCodes&quot;,
                      []
                    ]
                  }
                ]
              }
            }
          }
        ]
      },
      &quot;nReturned&quot;: 111581,
      &quot;executionTimeMillisEstimate&quot;: 306183
    },
    {
      &quot;$addFields&quot;: { &quot;_folders&quot;: &quot;$_folders&quot; },
      &quot;nReturned&quot;: 111581,
      &quot;executionTimeMillisEstimate&quot;: 306219
    },
    {
      &quot;$group&quot;: {
        &quot;_id&quot;: { &quot;$const&quot;: null },
        &quot;totalRecordCount&quot;: {
          &quot;$sum&quot;: { &quot;$const&quot;: 1 }
        }
      },
      &quot;nReturned&quot;: 1,
      &quot;executionTimeMillisEstimate&quot;: 306322
    },
    {
      &quot;$project&quot;: {
        &quot;totalRecordCount&quot;: true,
        &quot;_id&quot;: false
      },
      &quot;nReturned&quot;: 1,
      &quot;executionTimeMillisEstimate&quot;: 306322
    }
  ],
  &quot;ok&quot;: 1,
  &quot;operationTime&quot;: {
    &quot;$timestamp&quot;: &quot;7447046423054385155&quot;
  },
  &quot;$clusterTime&quot;: {
    &quot;clusterTime&quot;: {
      &quot;$timestamp&quot;: &quot;7447046448824188929&quot;
    },
    &quot;signature&quot;: {
      &quot;hash&quot;: &quot;8ohQJFJ8Q4JOo7T42kOYvvxxpXk=&quot;,
      &quot;keyId&quot;: {
        &quot;low&quot;: 2,
        &quot;high&quot;: 1721435263,
        &quot;unsigned&quot;: false
      }
    }
  }
}</code></pre>
</div>
</div>
</div>
</div>

![[48197271878-image-20241211-070851.png]]



![[48197271878-image-20241211-070907.png]]

</td>
</tr>
<tr>
<td>3</td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="094db4f8-286d-423b-9a76-b9bd9e496f46" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
   {
      &quot;$match&quot;:{
         &quot;_isBeingCreated&quot;:{
            &quot;$ne&quot;:true
         }
      }
   },
   {
      &quot;$match&quot;:{
         &quot;$or&quot;:[
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:false
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$size&quot;:0
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:true,
                  &quot;$eq&quot;:null
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:true,
                  &quot;$in&quot;:[
                     &#10;                  ]
               }
            }
         ]
      }
   },
   {
      &quot;$lookup&quot;:{
         &quot;from&quot;:&quot;folders&quot;,
         &quot;let&quot;:{
            &quot;folderIds&quot;:&quot;$folderIds&quot;
         },
         &quot;pipeline&quot;:[
            {
               &quot;$match&quot;:{
                  &quot;$expr&quot;:{
                     &quot;$in&quot;:[
                        {
                           &quot;$toString&quot;:&quot;$_id&quot;
                        },
                        &quot;$$folderIds&quot;
                     ]
                  }
               }
            },
            {
               &quot;$project&quot;:{
                  &quot;_id&quot;:{
                     &quot;$toString&quot;:&quot;$_id&quot;
                  },
                  &quot;name&quot;:&quot;$name&quot;,
                  &quot;securityClassCodes&quot;:{
                     &quot;$concatArrays&quot;:[
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$securityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        },
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$inheritedSecurityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        }
                     ]
                  }
               }
            }
         ],
         &quot;as&quot;:&quot;_folders&quot;
      }
   },
   {
      &quot;$addFields&quot;:{
         &quot;_folders&quot;:&quot;$_folders&quot;
      }
   },
   {
      &quot;$match&quot;:{
         &quot;$or&quot;:[
            {
               &quot;_folders.securityClassCodes&quot;:{
                  &quot;$in&quot;:[
                     &#10;                  ]
               }
            },
            {
               &quot;_folders.securityClassCodes&quot;:[
                  &#10;               ]
            },
            {
               &quot;_folders.securityClassCodes&quot;:{
                  &quot;$exists&quot;:false
               }
            }
         ]
      }
   },
   {
      &quot;$addFields&quot;:{
         &quot;_folders&quot;:{
            &quot;$filter&quot;:{
               &quot;input&quot;:&quot;$_folders&quot;,
               &quot;cond&quot;:{
                  &quot;$or&quot;:[
                     {
                        &quot;$gt&quot;:[
                           {
                              &quot;$size&quot;:{
                                 &quot;$setIntersection&quot;:[
                                    [
                                       &#10;                                    ],
                                    {
                                       &quot;$ifNull&quot;:[
                                          &quot;$$this.securityClassCodes&quot;,
                                          [
                                             &#10;                                          ]
                                       ]
                                    }
                                 ]
                              }
                           },
                           0
                        ]
                     },
                     {
                        &quot;$eq&quot;:[
                           {
                              &quot;$size&quot;:{
                                 &quot;$ifNull&quot;:[
                                    &quot;$$this.securityClassCodes&quot;,
                                    [
                                       &#10;                                    ]
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
      &quot;$match&quot;:{
         &quot;$and&quot;:[
            {
               &quot;senderTenantId&quot;:{
                  &quot;$exists&quot;:true
               }
            },
            {
               &quot;senderCompanyId&quot;:{
                  &quot;$exists&quot;:true
               }
            },
            {
               &quot;origin&quot;:{
                  &quot;$exists&quot;:true
               }
            },
            {
               &quot;_deletionStatus&quot;:&quot;false&quot;
            }
         ]
      }
   },
   {
      &quot;$group&quot;:{
         &quot;_id&quot;:{
            &quot;senderTenantId&quot;:&quot;$senderTenantId&quot;,
            &quot;senderCompanyId&quot;:&quot;$senderCompanyId&quot;,
            &quot;origin&quot;:&quot;$origin&quot;
         },
         &quot;documentReferenceDate&quot;:{
            &quot;$max&quot;:&quot;$documentReferenceDate&quot;
         },
         &quot;count&quot;:{
            &quot;$sum&quot;:1
         }
      }
   },
   {
      &quot;$sort&quot;:{
         &quot;_id.senderTenantId&quot;:1
      }
   },
   {
      &quot;$skip&quot;:0
   },
   {
      &quot;$limit&quot;:2147483647
   }
]</code></pre>
</div>
</div></td>
<td><p>documents</p></td>
<td><div id="expander-1844481114" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="769ce054-b45d-49e6-916d-ffaaeb5b18a6" data-macro-name="expand">
<div id="expander-control-1844481114" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Examine raw ouput</span>
</div>
<div id="expander-content-1844481114" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="23089936-b9c1-440f-bc26-444a92791894" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;serverInfo&quot;: {
    &quot;host&quot;: &quot;luz-mongodb01-cluster-mongos-2&quot;,
    &quot;port&quot;: 27017,
    &quot;version&quot;: &quot;4.4.6-8&quot;,
    &quot;gitVersion&quot;: &quot;8e04f19f85906daef6ef85d97b345fbcaf83f04b&quot;
  },
  &quot;stages&quot;: [
    {
      &quot;$cursor&quot;: {
        &quot;queryPlanner&quot;: {
          &quot;plannerVersion&quot;: 1,
          &quot;namespace&quot;: &quot;00a04daf-f2b3-41d5-8c12-2d1b4c48a36a.documents&quot;,
          &quot;indexFilterSet&quot;: false,
          &quot;parsedQuery&quot;: {
            &quot;$and&quot;: [
              {
                &quot;$or&quot;: [
                  {
                    &quot;$and&quot;: [
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$eq&quot;: null
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$exists&quot;: true
                        }
                      }
                    ]
                  },
                  {
                    &quot;$and&quot;: [
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$exists&quot;: true
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$in&quot;: []
                        }
                      }
                    ]
                  },
                  {
                    &quot;securityClassCodes&quot;: {
                      &quot;$size&quot;: 0
                    }
                  },
                  {
                    &quot;securityClassCodes&quot;: {
                      &quot;$not&quot;: { &quot;$exists&quot;: true }
                    }
                  }
                ]
              },
              {
                &quot;_deletionStatus&quot;: {
                  &quot;$eq&quot;: &quot;false&quot;
                }
              },
              { &quot;origin&quot;: { &quot;$exists&quot;: true } },
              {
                &quot;senderCompanyId&quot;: {
                  &quot;$exists&quot;: true
                }
              },
              {
                &quot;senderTenantId&quot;: {
                  &quot;$exists&quot;: true
                }
              },
              {
                &quot;_isBeingCreated&quot;: {
                  &quot;$not&quot;: { &quot;$eq&quot;: true }
                }
              }
            ]
          },
          &quot;queryHash&quot;: &quot;3EC179C7&quot;,
          &quot;planCacheKey&quot;: &quot;494EEC09&quot;,
          &quot;winningPlan&quot;: {
            &quot;stage&quot;: &quot;PROJECTION_SIMPLE&quot;,
            &quot;transformBy&quot;: {
              &quot;_folders&quot;: 1,
              &quot;documentReferenceDate&quot;: 1,
              &quot;folderIds&quot;: 1,
              &quot;origin&quot;: 1,
              &quot;senderCompanyId&quot;: 1,
              &quot;senderTenantId&quot;: 1,
              &quot;_id&quot;: 0
            },
            &quot;inputStage&quot;: {
              &quot;stage&quot;: &quot;COLLSCAN&quot;,
              &quot;filter&quot;: {
                &quot;$and&quot;: [
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$eq&quot;: null
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          }
                        ]
                      },
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$in&quot;: []
                            }
                          }
                        ]
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$size&quot;: 0
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$not&quot;: {
                            &quot;$exists&quot;: true
                          }
                        }
                      }
                    ]
                  },
                  {
                    &quot;_deletionStatus&quot;: {
                      &quot;$eq&quot;: &quot;false&quot;
                    }
                  },
                  {
                    &quot;origin&quot;: { &quot;$exists&quot;: true }
                  },
                  {
                    &quot;senderCompanyId&quot;: {
                      &quot;$exists&quot;: true
                    }
                  },
                  {
                    &quot;senderTenantId&quot;: {
                      &quot;$exists&quot;: true
                    }
                  },
                  {
                    &quot;_isBeingCreated&quot;: {
                      &quot;$not&quot;: { &quot;$eq&quot;: true }
                    }
                  }
                ]
              },
              &quot;direction&quot;: &quot;forward&quot;
            }
          },
          &quot;rejectedPlans&quot;: []
        },
        &quot;executionStats&quot;: {
          &quot;executionSuccess&quot;: true,
          &quot;nReturned&quot;: 25,
          &quot;executionTimeMillis&quot;: 226922,
          &quot;totalKeysExamined&quot;: 0,
          &quot;totalDocsExamined&quot;: 354897,
          &quot;executionStages&quot;: {
            &quot;stage&quot;: &quot;PROJECTION_SIMPLE&quot;,
            &quot;nReturned&quot;: 25,
            &quot;executionTimeMillisEstimate&quot;: 188903,
            &quot;works&quot;: 354899,
            &quot;advanced&quot;: 25,
            &quot;needTime&quot;: 354873,
            &quot;needYield&quot;: 0,
            &quot;saveState&quot;: 8003,
            &quot;restoreState&quot;: 8003,
            &quot;isEOF&quot;: 1,
            &quot;transformBy&quot;: {
              &quot;_folders&quot;: 1,
              &quot;documentReferenceDate&quot;: 1,
              &quot;folderIds&quot;: 1,
              &quot;origin&quot;: 1,
              &quot;senderCompanyId&quot;: 1,
              &quot;senderTenantId&quot;: 1,
              &quot;_id&quot;: 0
            },
            &quot;inputStage&quot;: {
              &quot;stage&quot;: &quot;COLLSCAN&quot;,
              &quot;filter&quot;: {
                &quot;$and&quot;: [
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$eq&quot;: null
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          }
                        ]
                      },
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$in&quot;: []
                            }
                          }
                        ]
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$size&quot;: 0
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$not&quot;: {
                            &quot;$exists&quot;: true
                          }
                        }
                      }
                    ]
                  },
                  {
                    &quot;_deletionStatus&quot;: {
                      &quot;$eq&quot;: &quot;false&quot;
                    }
                  },
                  {
                    &quot;origin&quot;: { &quot;$exists&quot;: true }
                  },
                  {
                    &quot;senderCompanyId&quot;: {
                      &quot;$exists&quot;: true
                    }
                  },
                  {
                    &quot;senderTenantId&quot;: {
                      &quot;$exists&quot;: true
                    }
                  },
                  {
                    &quot;_isBeingCreated&quot;: {
                      &quot;$not&quot;: { &quot;$eq&quot;: true }
                    }
                  }
                ]
              },
              &quot;nReturned&quot;: 25,
              &quot;executionTimeMillisEstimate&quot;: 188867,
              &quot;works&quot;: 354899,
              &quot;advanced&quot;: 25,
              &quot;needTime&quot;: 354873,
              &quot;needYield&quot;: 0,
              &quot;saveState&quot;: 8003,
              &quot;restoreState&quot;: 8003,
              &quot;isEOF&quot;: 1,
              &quot;direction&quot;: &quot;forward&quot;,
              &quot;docsExamined&quot;: 354897
            }
          }
        }
      },
      &quot;nReturned&quot;: 25,
      &quot;executionTimeMillisEstimate&quot;: 226917
    },
    {
      &quot;$lookup&quot;: {
        &quot;from&quot;: &quot;folders&quot;,
        &quot;as&quot;: &quot;_folders&quot;,
        &quot;let&quot;: { &quot;folderIds&quot;: &quot;$folderIds&quot; },
        &quot;pipeline&quot;: [
          {
            &quot;$match&quot;: {
              &quot;$expr&quot;: {
                &quot;$in&quot;: [
                  { &quot;$toString&quot;: &quot;$_id&quot; },
                  &quot;$$folderIds&quot;
                ]
              }
            }
          },
          {
            &quot;$project&quot;: {
              &quot;_id&quot;: { &quot;$toString&quot;: &quot;$_id&quot; },
              &quot;name&quot;: &quot;$name&quot;,
              &quot;securityClassCodes&quot;: {
                &quot;$concatArrays&quot;: [
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$securityClassCodes&quot;,
                      []
                    ]
                  },
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$inheritedSecurityClassCodes&quot;,
                      []
                    ]
                  }
                ]
              }
            }
          }
        ]
      },
      &quot;nReturned&quot;: 25,
      &quot;executionTimeMillisEstimate&quot;: 226922
    },
    {
      &quot;$match&quot;: {
        &quot;$or&quot;: [
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$in&quot;: []
            }
          },
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$eq&quot;: []
            }
          },
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$not&quot;: { &quot;$exists&quot;: true }
            }
          }
        ]
      },
      &quot;nReturned&quot;: 25,
      &quot;executionTimeMillisEstimate&quot;: 226922
    },
    {
      &quot;$addFields&quot;: { &quot;_folders&quot;: &quot;$_folders&quot; },
      &quot;nReturned&quot;: 25,
      &quot;executionTimeMillisEstimate&quot;: 226922
    },
    {
      &quot;$addFields&quot;: {
        &quot;_folders&quot;: {
          &quot;$filter&quot;: {
            &quot;input&quot;: &quot;$_folders&quot;,
            &quot;as&quot;: &quot;this&quot;,
            &quot;cond&quot;: {
              &quot;$or&quot;: [
                {
                  &quot;$gt&quot;: [
                    {
                      &quot;$size&quot;: [
                        {
                          &quot;$setIntersection&quot;: [
                            {
                              &quot;$ifNull&quot;: [
                                &quot;$$this.securityClassCodes&quot;,
                                { &quot;$const&quot;: [] }
                              ]
                            },
                            { &quot;$const&quot;: [] }
                          ]
                        }
                      ]
                    },
                    { &quot;$const&quot;: 0 }
                  ]
                },
                {
                  &quot;$eq&quot;: [
                    {
                      &quot;$size&quot;: [
                        {
                          &quot;$ifNull&quot;: [
                            &quot;$$this.securityClassCodes&quot;,
                            { &quot;$const&quot;: [] }
                          ]
                        }
                      ]
                    },
                    { &quot;$const&quot;: 0 }
                  ]
                }
              ]
            }
          }
        }
      },
      &quot;nReturned&quot;: 25,
      &quot;executionTimeMillisEstimate&quot;: 226922
    },
    {
      &quot;$group&quot;: {
        &quot;_id&quot;: {
          &quot;senderTenantId&quot;: &quot;$senderTenantId&quot;,
          &quot;senderCompanyId&quot;: &quot;$senderCompanyId&quot;,
          &quot;origin&quot;: &quot;$origin&quot;
        },
        &quot;documentReferenceDate&quot;: {
          &quot;$max&quot;: &quot;$documentReferenceDate&quot;
        },
        &quot;count&quot;: { &quot;$sum&quot;: { &quot;$const&quot;: 1 } }
      },
      &quot;nReturned&quot;: 2,
      &quot;executionTimeMillisEstimate&quot;: 226922
    },
    {
      &quot;$sort&quot;: {
        &quot;sortKey&quot;: { &quot;_id.senderTenantId&quot;: 1 },
        &quot;limit&quot;: 2147483647
      },
      &quot;nReturned&quot;: 2,
      &quot;executionTimeMillisEstimate&quot;: 226922
    }
  ],
  &quot;ok&quot;: 1,
  &quot;operationTime&quot;: {
    &quot;$timestamp&quot;: &quot;7447048957085089793&quot;
  },
  &quot;$clusterTime&quot;: {
    &quot;clusterTime&quot;: {
      &quot;$timestamp&quot;: &quot;7447048995739795457&quot;
    },
    &quot;signature&quot;: {
      &quot;hash&quot;: &quot;dX4V1lMiiwh9M9WWCZaMRfS+Crg=&quot;,
      &quot;keyId&quot;: {
        &quot;low&quot;: 2,
        &quot;high&quot;: 1721435263,
        &quot;unsigned&quot;: false
      }
    }
  }
}</code></pre>
</div>
</div>
</div>
</div>

![[48197271878-image-20241211-071412.png]]



![[48197271878-image-20241211-071435.png]]

</td>
</tr>
<tr>
<td>4</td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="6ea73c23-d726-4dd7-9221-6a120511a32c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
   {
      &quot;$match&quot;:{
         &quot;$and&quot;:[
            {
               &quot;$or&quot;:[
                  {
                     &quot;parentFolderIds&quot;:{
                        &quot;$size&quot;:0
                     }
                  },
                  {
                     &quot;parentFolderIds&quot;:{
                        &quot;$exists&quot;:false
                     }
                  }
               ]
            },
            {
               &quot;$or&quot;:[
                  {
                     &quot;$or&quot;:[
                        {
                           &quot;securityClassCodes&quot;:{
                              &quot;$exists&quot;:true,
                              &quot;$in&quot;:[
                                 &#10;                              ]
                           }
                        },
                        {
                           &quot;inheritedSecurityClassCodes&quot;:{
                              &quot;$exists&quot;:true,
                              &quot;$in&quot;:[
                                 &#10;                              ]
                           }
                        }
                     ]
                  },
                  {
                     &quot;$and&quot;:[
                        {
                           &quot;$or&quot;:[
                              {
                                 &quot;securityClassCodes&quot;:{
                                    &quot;$exists&quot;:false
                                 }
                              },
                              {
                                 &quot;securityClassCodes&quot;:{
                                    &quot;$size&quot;:0
                                 }
                              },
                              {
                                 &quot;securityClassCodes&quot;:{
                                    &quot;$exists&quot;:true,
                                    &quot;$eq&quot;:null
                                 }
                              }
                           ]
                        },
                        {
                           &quot;$or&quot;:[
                              {
                                 &quot;inheritedSecurityClassCodes&quot;:{
                                    &quot;$exists&quot;:false
                                 }
                              },
                              {
                                 &quot;inheritedSecurityClassCodes&quot;:{
                                    &quot;$size&quot;:0
                                 }
                              },
                              {
                                 &quot;inheritedSecurityClassCodes&quot;:{
                                    &quot;$exists&quot;:true,
                                    &quot;$eq&quot;:null
                                 }
                              }
                           ]
                        }
                     ]
                  }
               ]
            },
            {
               &quot;$or&quot;:[
                  {
                     &quot;_deletionStatus&quot;:{
                        &quot;$exists&quot;:false
                     }
                  },
                  {
                     &quot;_deletionStatus&quot;:&quot;false&quot;
                  }
               ]
            }
         ]
      }
   },
   {
      &quot;$count&quot;:&quot;totalRecordCount&quot;
   }
]</code></pre>
</div>
</div></td>
<td><p>folders</p></td>
<td><div id="expander-1144738262" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="d99821aa-7d0f-495c-b2c8-6cd96aec85a0" data-macro-name="expand">
<div id="expander-control-1144738262" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Examine raw ouput</span>
</div>
<div id="expander-content-1144738262" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="false" data-macro-id="457994f2-7ba7-475a-960e-b31a224a5b5f" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code> </code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td>5</td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="393a3d54-0474-4544-af2d-4b8086c5b250" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
   {
      &quot;$match&quot;:{
         &quot;_isBeingCreated&quot;:{
            &quot;$ne&quot;:true
         }
      }
   },
   {
      &quot;$match&quot;:{
         &quot;$or&quot;:[
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:false
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$size&quot;:0
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:true,
                  &quot;$eq&quot;:null
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:true,
                  &quot;$in&quot;:[
                     &#10;                  ]
               }
            }
         ]
      }
   },
   {
      &quot;$lookup&quot;:{
         &quot;from&quot;:&quot;folders&quot;,
         &quot;let&quot;:{
            &quot;folderIds&quot;:&quot;$folderIds&quot;
         },
         &quot;pipeline&quot;:[
            {
               &quot;$match&quot;:{
                  &quot;$expr&quot;:{
                     &quot;$in&quot;:[
                        {
                           &quot;$toString&quot;:&quot;$_id&quot;
                        },
                        &quot;$$folderIds&quot;
                     ]
                  }
               }
            },
            {
               &quot;$project&quot;:{
                  &quot;_id&quot;:{
                     &quot;$toString&quot;:&quot;$_id&quot;
                  },
                  &quot;name&quot;:&quot;$name&quot;,
                  &quot;securityClassCodes&quot;:{
                     &quot;$concatArrays&quot;:[
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$securityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        },
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$inheritedSecurityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        }
                     ]
                  }
               }
            }
         ],
         &quot;as&quot;:&quot;_folders&quot;
      }
   },
   {
      &quot;$addFields&quot;:{
         &quot;_folders&quot;:&quot;$_folders&quot;
      }
   },
   {
      &quot;$match&quot;:{
         &quot;$or&quot;:[
            {
               &quot;_folders.securityClassCodes&quot;:{
                  &quot;$in&quot;:[
                     &#10;                  ]
               }
            },
            {
               &quot;_folders.securityClassCodes&quot;:[
                  &#10;               ]
            },
            {
               &quot;_folders.securityClassCodes&quot;:{
                  &quot;$exists&quot;:false
               }
            }
         ]
      }
   },
   {
      &quot;$addFields&quot;:{
         &quot;_folders&quot;:{
            &quot;$filter&quot;:{
               &quot;input&quot;:&quot;$_folders&quot;,
               &quot;cond&quot;:{
                  &quot;$or&quot;:[
                     {
                        &quot;$gt&quot;:[
                           {
                              &quot;$size&quot;:{
                                 &quot;$setIntersection&quot;:[
                                    [
                                       &#10;                                    ],
                                    {
                                       &quot;$ifNull&quot;:[
                                          &quot;$$this.securityClassCodes&quot;,
                                          [
                                             &#10;                                          ]
                                       ]
                                    }
                                 ]
                              }
                           },
                           0
                        ]
                     },
                     {
                        &quot;$eq&quot;:[
                           {
                              &quot;$size&quot;:{
                                 &quot;$ifNull&quot;:[
                                    &quot;$$this.securityClassCodes&quot;,
                                    [
                                       &#10;                                    ]
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
      &quot;$match&quot;:{
         &quot;$and&quot;:[
            {
               &quot;folderIds&quot;:{
                  &quot;$exists&quot;:true
               }
            },
            {
               &quot;_deletionStatus&quot;:&quot;false&quot;
            }
         ]
      }
   },
   {
      &quot;$group&quot;:{
         &quot;_id&quot;:&quot;$folderIds&quot;,
         &quot;count&quot;:{
            &quot;$sum&quot;:1
         }
      }
   },
   {
      &quot;$sort&quot;:{
         &quot;_id&quot;:1
      }
   },
   {
      &quot;$skip&quot;:0
   },
   {
      &quot;$limit&quot;:2147483647
   }
]</code></pre>
</div>
</div></td>
<td><p>documents</p></td>
<td><div id="expander-1086534779" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="b9fac79d-cb26-4f90-ade0-a76d20d0cf62" data-macro-name="expand">
<div id="expander-control-1086534779" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Examine raw ouput</span>
</div>
<div id="expander-content-1086534779" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ac458dcd-dc10-419f-a904-f081a8007c76" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;serverInfo&quot;: {
    &quot;host&quot;: &quot;luz-mongodb01-cluster-mongos-2&quot;,
    &quot;port&quot;: 27017,
    &quot;version&quot;: &quot;4.4.6-8&quot;,
    &quot;gitVersion&quot;: &quot;8e04f19f85906daef6ef85d97b345fbcaf83f04b&quot;
  },
  &quot;stages&quot;: [
    {
      &quot;$cursor&quot;: {
        &quot;queryPlanner&quot;: {
          &quot;plannerVersion&quot;: 1,
          &quot;namespace&quot;: &quot;00a04daf-f2b3-41d5-8c12-2d1b4c48a36a.documents&quot;,
          &quot;indexFilterSet&quot;: false,
          &quot;parsedQuery&quot;: {
            &quot;$and&quot;: [
              {
                &quot;$or&quot;: [
                  {
                    &quot;$and&quot;: [
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$eq&quot;: null
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$exists&quot;: true
                        }
                      }
                    ]
                  },
                  {
                    &quot;$and&quot;: [
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$exists&quot;: true
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$in&quot;: []
                        }
                      }
                    ]
                  },
                  {
                    &quot;securityClassCodes&quot;: {
                      &quot;$size&quot;: 0
                    }
                  },
                  {
                    &quot;securityClassCodes&quot;: {
                      &quot;$not&quot;: { &quot;$exists&quot;: true }
                    }
                  }
                ]
              },
              {
                &quot;_deletionStatus&quot;: {
                  &quot;$eq&quot;: &quot;false&quot;
                }
              },
              {
                &quot;folderIds&quot;: { &quot;$exists&quot;: true }
              },
              {
                &quot;_isBeingCreated&quot;: {
                  &quot;$not&quot;: { &quot;$eq&quot;: true }
                }
              }
            ]
          },
          &quot;queryHash&quot;: &quot;965F5292&quot;,
          &quot;planCacheKey&quot;: &quot;B81AD236&quot;,
          &quot;winningPlan&quot;: {
            &quot;stage&quot;: &quot;PROJECTION_SIMPLE&quot;,
            &quot;transformBy&quot;: {
              &quot;_folders&quot;: 1,
              &quot;folderIds&quot;: 1,
              &quot;_id&quot;: 0
            },
            &quot;inputStage&quot;: {
              &quot;stage&quot;: &quot;COLLSCAN&quot;,
              &quot;filter&quot;: {
                &quot;$and&quot;: [
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$eq&quot;: null
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          }
                        ]
                      },
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$in&quot;: []
                            }
                          }
                        ]
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$size&quot;: 0
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$not&quot;: {
                            &quot;$exists&quot;: true
                          }
                        }
                      }
                    ]
                  },
                  {
                    &quot;_deletionStatus&quot;: {
                      &quot;$eq&quot;: &quot;false&quot;
                    }
                  },
                  {
                    &quot;folderIds&quot;: {
                      &quot;$exists&quot;: true
                    }
                  },
                  {
                    &quot;_isBeingCreated&quot;: {
                      &quot;$not&quot;: { &quot;$eq&quot;: true }
                    }
                  }
                ]
              },
              &quot;direction&quot;: &quot;forward&quot;
            }
          },
          &quot;rejectedPlans&quot;: []
        },
        &quot;executionStats&quot;: {
          &quot;executionSuccess&quot;: true,
          &quot;nReturned&quot;: 354526,
          &quot;executionTimeMillis&quot;: 244636,
          &quot;totalKeysExamined&quot;: 0,
          &quot;totalDocsExamined&quot;: 354897,
          &quot;executionStages&quot;: {
            &quot;stage&quot;: &quot;PROJECTION_SIMPLE&quot;,
            &quot;nReturned&quot;: 354526,
            &quot;executionTimeMillisEstimate&quot;: 182429,
            &quot;works&quot;: 354899,
            &quot;advanced&quot;: 354526,
            &quot;needTime&quot;: 372,
            &quot;needYield&quot;: 0,
            &quot;saveState&quot;: 7430,
            &quot;restoreState&quot;: 7430,
            &quot;isEOF&quot;: 1,
            &quot;transformBy&quot;: {
              &quot;_folders&quot;: 1,
              &quot;folderIds&quot;: 1,
              &quot;_id&quot;: 0
            },
            &quot;inputStage&quot;: {
              &quot;stage&quot;: &quot;COLLSCAN&quot;,
              &quot;filter&quot;: {
                &quot;$and&quot;: [
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$eq&quot;: null
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          }
                        ]
                      },
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$in&quot;: []
                            }
                          }
                        ]
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$size&quot;: 0
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$not&quot;: {
                            &quot;$exists&quot;: true
                          }
                        }
                      }
                    ]
                  },
                  {
                    &quot;_deletionStatus&quot;: {
                      &quot;$eq&quot;: &quot;false&quot;
                    }
                  },
                  {
                    &quot;folderIds&quot;: {
                      &quot;$exists&quot;: true
                    }
                  },
                  {
                    &quot;_isBeingCreated&quot;: {
                      &quot;$not&quot;: { &quot;$eq&quot;: true }
                    }
                  }
                ]
              },
              &quot;nReturned&quot;: 354526,
              &quot;executionTimeMillisEstimate&quot;: 182050,
              &quot;works&quot;: 354899,
              &quot;advanced&quot;: 354526,
              &quot;needTime&quot;: 372,
              &quot;needYield&quot;: 0,
              &quot;saveState&quot;: 7430,
              &quot;restoreState&quot;: 7430,
              &quot;isEOF&quot;: 1,
              &quot;direction&quot;: &quot;forward&quot;,
              &quot;docsExamined&quot;: 354897
            }
          }
        }
      },
      &quot;nReturned&quot;: 354526,
      &quot;executionTimeMillisEstimate&quot;: 212014
    },
    {
      &quot;$lookup&quot;: {
        &quot;from&quot;: &quot;folders&quot;,
        &quot;as&quot;: &quot;_folders&quot;,
        &quot;let&quot;: { &quot;folderIds&quot;: &quot;$folderIds&quot; },
        &quot;pipeline&quot;: [
          {
            &quot;$match&quot;: {
              &quot;$expr&quot;: {
                &quot;$in&quot;: [
                  { &quot;$toString&quot;: &quot;$_id&quot; },
                  &quot;$$folderIds&quot;
                ]
              }
            }
          },
          {
            &quot;$project&quot;: {
              &quot;_id&quot;: { &quot;$toString&quot;: &quot;$_id&quot; },
              &quot;name&quot;: &quot;$name&quot;,
              &quot;securityClassCodes&quot;: {
                &quot;$concatArrays&quot;: [
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$securityClassCodes&quot;,
                      []
                    ]
                  },
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$inheritedSecurityClassCodes&quot;,
                      []
                    ]
                  }
                ]
              }
            }
          }
        ]
      },
      &quot;nReturned&quot;: 354526,
      &quot;executionTimeMillisEstimate&quot;: 244587
    },
    {
      &quot;$match&quot;: {
        &quot;$or&quot;: [
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$in&quot;: []
            }
          },
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$eq&quot;: []
            }
          },
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$not&quot;: { &quot;$exists&quot;: true }
            }
          }
        ]
      },
      &quot;nReturned&quot;: 354526,
      &quot;executionTimeMillisEstimate&quot;: 244593
    },
    {
      &quot;$addFields&quot;: { &quot;_folders&quot;: &quot;$_folders&quot; },
      &quot;nReturned&quot;: 354526,
      &quot;executionTimeMillisEstimate&quot;: 244596
    },
    {
      &quot;$addFields&quot;: {
        &quot;_folders&quot;: {
          &quot;$filter&quot;: {
            &quot;input&quot;: &quot;$_folders&quot;,
            &quot;as&quot;: &quot;this&quot;,
            &quot;cond&quot;: {
              &quot;$or&quot;: [
                {
                  &quot;$gt&quot;: [
                    {
                      &quot;$size&quot;: [
                        {
                          &quot;$setIntersection&quot;: [
                            {
                              &quot;$ifNull&quot;: [
                                &quot;$$this.securityClassCodes&quot;,
                                { &quot;$const&quot;: [] }
                              ]
                            },
                            { &quot;$const&quot;: [] }
                          ]
                        }
                      ]
                    },
                    { &quot;$const&quot;: 0 }
                  ]
                },
                {
                  &quot;$eq&quot;: [
                    {
                      &quot;$size&quot;: [
                        {
                          &quot;$ifNull&quot;: [
                            &quot;$$this.securityClassCodes&quot;,
                            { &quot;$const&quot;: [] }
                          ]
                        }
                      ]
                    },
                    { &quot;$const&quot;: 0 }
                  ]
                }
              ]
            }
          }
        }
      },
      &quot;nReturned&quot;: 354526,
      &quot;executionTimeMillisEstimate&quot;: 244596
    },
    {
      &quot;$group&quot;: {
        &quot;_id&quot;: &quot;$folderIds&quot;,
        &quot;count&quot;: { &quot;$sum&quot;: { &quot;$const&quot;: 1 } }
      },
      &quot;nReturned&quot;: 1,
      &quot;executionTimeMillisEstimate&quot;: 244602
    },
    {
      &quot;$sort&quot;: {
        &quot;sortKey&quot;: { &quot;_id&quot;: 1 },
        &quot;limit&quot;: 2147483647
      },
      &quot;nReturned&quot;: 1,
      &quot;executionTimeMillisEstimate&quot;: 244602
    }
  ],
  &quot;ok&quot;: 1,
  &quot;operationTime&quot;: {
    &quot;$timestamp&quot;: &quot;7447051877662851073&quot;
  },
  &quot;$clusterTime&quot;: {
    &quot;clusterTime&quot;: {
      &quot;$timestamp&quot;: &quot;7447051881957818370&quot;
    },
    &quot;signature&quot;: {
      &quot;hash&quot;: &quot;YbAt6iAlUR9k5ZYDr6gEVrhi838=&quot;,
      &quot;keyId&quot;: {
        &quot;low&quot;: 2,
        &quot;high&quot;: 1721435263,
        &quot;unsigned&quot;: false
      }
    }
  }
}</code></pre>
</div>
</div>
</div>
</div>

![[48197271878-image-20241211-072936.png]]



![[48197271878-image-20241211-072953.png]]

</td>
</tr>
<tr>
<td>6</td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="4dd6a9db-2995-4ec3-af66-09786500ee1e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
   {
      &quot;$match&quot;:{
         &quot;_isBeingCreated&quot;:{
            &quot;$ne&quot;:true
         }
      }
   },
   {
      &quot;$match&quot;:{
         &quot;$or&quot;:[
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:false
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$size&quot;:0
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:true,
                  &quot;$eq&quot;:null
               }
            },
            {
               &quot;securityClassCodes&quot;:{
                  &quot;$exists&quot;:true,
                  &quot;$in&quot;:[
                     &#10;                  ]
               }
            }
         ]
      }
   },
   {
      &quot;$lookup&quot;:{
         &quot;from&quot;:&quot;folders&quot;,
         &quot;let&quot;:{
            &quot;folderIds&quot;:&quot;$folderIds&quot;
         },
         &quot;pipeline&quot;:[
            {
               &quot;$match&quot;:{
                  &quot;$expr&quot;:{
                     &quot;$in&quot;:[
                        {
                           &quot;$toString&quot;:&quot;$_id&quot;
                        },
                        &quot;$$folderIds&quot;
                     ]
                  }
               }
            },
            {
               &quot;$project&quot;:{
                  &quot;_id&quot;:{
                     &quot;$toString&quot;:&quot;$_id&quot;
                  },
                  &quot;name&quot;:&quot;$name&quot;,
                  &quot;securityClassCodes&quot;:{
                     &quot;$concatArrays&quot;:[
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$securityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        },
                        {
                           &quot;$ifNull&quot;:[
                              &quot;$inheritedSecurityClassCodes&quot;,
                              [
                                 &#10;                              ]
                           ]
                        }
                     ]
                  }
               }
            }
         ],
         &quot;as&quot;:&quot;_folders&quot;
      }
   },
   {
      &quot;$addFields&quot;:{
         &quot;_folders&quot;:&quot;$_folders&quot;
      }
   },
   {
      &quot;$match&quot;:{
         &quot;$or&quot;:[
            {
               &quot;_folders.securityClassCodes&quot;:{
                  &quot;$in&quot;:[
                     &#10;                  ]
               }
            },
            {
               &quot;_folders.securityClassCodes&quot;:[
                  &#10;               ]
            },
            {
               &quot;_folders.securityClassCodes&quot;:{
                  &quot;$exists&quot;:false
               }
            }
         ]
      }
   },
   {
      &quot;$addFields&quot;:{
         &quot;_folders&quot;:{
            &quot;$filter&quot;:{
               &quot;input&quot;:&quot;$_folders&quot;,
               &quot;cond&quot;:{
                  &quot;$or&quot;:[
                     {
                        &quot;$gt&quot;:[
                           {
                              &quot;$size&quot;:{
                                 &quot;$setIntersection&quot;:[
                                    [
                                       &#10;                                    ],
                                    {
                                       &quot;$ifNull&quot;:[
                                          &quot;$$this.securityClassCodes&quot;,
                                          [
                                             &#10;                                          ]
                                       ]
                                    }
                                 ]
                              }
                           },
                           0
                        ]
                     },
                     {
                        &quot;$eq&quot;:[
                           {
                              &quot;$size&quot;:{
                                 &quot;$ifNull&quot;:[
                                    &quot;$$this.securityClassCodes&quot;,
                                    [
                                       &#10;                                    ]
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
      &quot;$match&quot;:{
         &quot;$or&quot;:[
            {
               &quot;$and&quot;:[
                  {
                     &quot;tags&quot;:{
                        &quot;$size&quot;:0
                     }
                  },
                  {
                     &quot;tags&quot;:{
                        &quot;$exists&quot;:true
                     }
                  },
                  {
                     &quot;_deletionStatus&quot;:&quot;false&quot;
                  }
               ]
            },
            {
               &quot;$and&quot;:[
                  {
                     &quot;tags.0&quot;:{
                        &quot;$exists&quot;:true
                     }
                  },
                  {
                     &quot;_deletionStatus&quot;:&quot;false&quot;
                  }
               ]
            }
         ]
      }
   },
   {
      &quot;$unwind&quot;:{
         &quot;path&quot;:&quot;$tags&quot;,
         &quot;preserveNullAndEmptyArrays&quot;:true
      }
   },
   {
      &quot;$group&quot;:{
         &quot;_id&quot;:&quot;$tags&quot;,
         &quot;count&quot;:{
            &quot;$sum&quot;:1
         }
      }
   },
   {
      &quot;$sort&quot;:{
         &quot;_id&quot;:1
      }
   }
]</code></pre>
</div>
</div></td>
<td><p>documents</p></td>
<td><div id="expander-178333902" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="7f8ede5a-c0c9-40bc-b0e4-4f4bedca25e4" data-macro-name="expand">
<div id="expander-control-178333902" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Examine raw ouput</span>
</div>
<div id="expander-content-178333902" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="adb74c29-4014-41cc-8063-3e4644e3e96f" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;serverInfo&quot;: {
    &quot;host&quot;: &quot;luz-mongodb01-cluster-mongos-2&quot;,
    &quot;port&quot;: 27017,
    &quot;version&quot;: &quot;4.4.6-8&quot;,
    &quot;gitVersion&quot;: &quot;8e04f19f85906daef6ef85d97b345fbcaf83f04b&quot;
  },
  &quot;stages&quot;: [
    {
      &quot;$cursor&quot;: {
        &quot;queryPlanner&quot;: {
          &quot;plannerVersion&quot;: 1,
          &quot;namespace&quot;: &quot;00a04daf-f2b3-41d5-8c12-2d1b4c48a36a.documents&quot;,
          &quot;indexFilterSet&quot;: false,
          &quot;parsedQuery&quot;: {
            &quot;$and&quot;: [
              {
                &quot;$or&quot;: [
                  {
                    &quot;$and&quot;: [
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$eq&quot;: null
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$exists&quot;: true
                        }
                      }
                    ]
                  },
                  {
                    &quot;$and&quot;: [
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$exists&quot;: true
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$in&quot;: []
                        }
                      }
                    ]
                  },
                  {
                    &quot;securityClassCodes&quot;: {
                      &quot;$size&quot;: 0
                    }
                  },
                  {
                    &quot;securityClassCodes&quot;: {
                      &quot;$not&quot;: { &quot;$exists&quot;: true }
                    }
                  }
                ]
              },
              {
                &quot;_isBeingCreated&quot;: {
                  &quot;$not&quot;: { &quot;$eq&quot;: true }
                }
              }
            ]
          },
          &quot;queryHash&quot;: &quot;BBFC09B2&quot;,
          &quot;planCacheKey&quot;: &quot;80A031DE&quot;,
          &quot;winningPlan&quot;: {
            &quot;stage&quot;: &quot;PROJECTION_SIMPLE&quot;,
            &quot;transformBy&quot;: {
              &quot;_deletionStatus&quot;: 1,
              &quot;_folders&quot;: 1,
              &quot;folderIds&quot;: 1,
              &quot;tags&quot;: 1,
              &quot;_id&quot;: 0
            },
            &quot;inputStage&quot;: {
              &quot;stage&quot;: &quot;COLLSCAN&quot;,
              &quot;filter&quot;: {
                &quot;$and&quot;: [
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$eq&quot;: null
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          }
                        ]
                      },
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$in&quot;: []
                            }
                          }
                        ]
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$size&quot;: 0
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$not&quot;: {
                            &quot;$exists&quot;: true
                          }
                        }
                      }
                    ]
                  },
                  {
                    &quot;_isBeingCreated&quot;: {
                      &quot;$not&quot;: { &quot;$eq&quot;: true }
                    }
                  }
                ]
              },
              &quot;direction&quot;: &quot;forward&quot;
            }
          },
          &quot;rejectedPlans&quot;: []
        },
        &quot;executionStats&quot;: {
          &quot;executionSuccess&quot;: true,
          &quot;nReturned&quot;: 354527,
          &quot;executionTimeMillis&quot;: 262238,
          &quot;totalKeysExamined&quot;: 0,
          &quot;totalDocsExamined&quot;: 354897,
          &quot;executionStages&quot;: {
            &quot;stage&quot;: &quot;PROJECTION_SIMPLE&quot;,
            &quot;nReturned&quot;: 354527,
            &quot;executionTimeMillisEstimate&quot;: 195441,
            &quot;works&quot;: 354899,
            &quot;advanced&quot;: 354527,
            &quot;needTime&quot;: 371,
            &quot;needYield&quot;: 0,
            &quot;saveState&quot;: 7853,
            &quot;restoreState&quot;: 7853,
            &quot;isEOF&quot;: 1,
            &quot;transformBy&quot;: {
              &quot;_deletionStatus&quot;: 1,
              &quot;_folders&quot;: 1,
              &quot;folderIds&quot;: 1,
              &quot;tags&quot;: 1,
              &quot;_id&quot;: 0
            },
            &quot;inputStage&quot;: {
              &quot;stage&quot;: &quot;COLLSCAN&quot;,
              &quot;filter&quot;: {
                &quot;$and&quot;: [
                  {
                    &quot;$or&quot;: [
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$eq&quot;: null
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          }
                        ]
                      },
                      {
                        &quot;$and&quot;: [
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$exists&quot;: true
                            }
                          },
                          {
                            &quot;securityClassCodes&quot;: {
                              &quot;$in&quot;: []
                            }
                          }
                        ]
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$size&quot;: 0
                        }
                      },
                      {
                        &quot;securityClassCodes&quot;: {
                          &quot;$not&quot;: {
                            &quot;$exists&quot;: true
                          }
                        }
                      }
                    ]
                  },
                  {
                    &quot;_isBeingCreated&quot;: {
                      &quot;$not&quot;: { &quot;$eq&quot;: true }
                    }
                  }
                ]
              },
              &quot;nReturned&quot;: 354527,
              &quot;executionTimeMillisEstimate&quot;: 194850,
              &quot;works&quot;: 354899,
              &quot;advanced&quot;: 354527,
              &quot;needTime&quot;: 371,
              &quot;needYield&quot;: 0,
              &quot;saveState&quot;: 7853,
              &quot;restoreState&quot;: 7853,
              &quot;isEOF&quot;: 1,
              &quot;direction&quot;: &quot;forward&quot;,
              &quot;docsExamined&quot;: 354897
            }
          }
        }
      },
      &quot;nReturned&quot;: 354527,
      &quot;executionTimeMillisEstimate&quot;: 227373
    },
    {
      &quot;$lookup&quot;: {
        &quot;from&quot;: &quot;folders&quot;,
        &quot;as&quot;: &quot;_folders&quot;,
        &quot;let&quot;: { &quot;folderIds&quot;: &quot;$folderIds&quot; },
        &quot;pipeline&quot;: [
          {
            &quot;$match&quot;: {
              &quot;$expr&quot;: {
                &quot;$in&quot;: [
                  { &quot;$toString&quot;: &quot;$_id&quot; },
                  &quot;$$folderIds&quot;
                ]
              }
            }
          },
          {
            &quot;$project&quot;: {
              &quot;_id&quot;: { &quot;$toString&quot;: &quot;$_id&quot; },
              &quot;name&quot;: &quot;$name&quot;,
              &quot;securityClassCodes&quot;: {
                &quot;$concatArrays&quot;: [
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$securityClassCodes&quot;,
                      []
                    ]
                  },
                  {
                    &quot;$ifNull&quot;: [
                      &quot;$inheritedSecurityClassCodes&quot;,
                      []
                    ]
                  }
                ]
              }
            }
          }
        ]
      },
      &quot;nReturned&quot;: 354527,
      &quot;executionTimeMillisEstimate&quot;: 262045
    },
    {
      &quot;$match&quot;: {
        &quot;$or&quot;: [
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$in&quot;: []
            }
          },
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$eq&quot;: []
            }
          },
          {
            &quot;_folders.securityClassCodes&quot;: {
              &quot;$not&quot;: { &quot;$exists&quot;: true }
            }
          }
        ]
      },
      &quot;nReturned&quot;: 354527,
      &quot;executionTimeMillisEstimate&quot;: 262092
    },
    {
      &quot;$addFields&quot;: { &quot;_folders&quot;: &quot;$_folders&quot; },
      &quot;nReturned&quot;: 354527,
      &quot;executionTimeMillisEstimate&quot;: 262114
    },
    {
      &quot;$addFields&quot;: {
        &quot;_folders&quot;: {
          &quot;$filter&quot;: {
            &quot;input&quot;: &quot;$_folders&quot;,
            &quot;as&quot;: &quot;this&quot;,
            &quot;cond&quot;: {
              &quot;$or&quot;: [
                {
                  &quot;$gt&quot;: [
                    {
                      &quot;$size&quot;: [
                        {
                          &quot;$setIntersection&quot;: [
                            {
                              &quot;$ifNull&quot;: [
                                &quot;$$this.securityClassCodes&quot;,
                                { &quot;$const&quot;: [] }
                              ]
                            },
                            { &quot;$const&quot;: [] }
                          ]
                        }
                      ]
                    },
                    { &quot;$const&quot;: 0 }
                  ]
                },
                {
                  &quot;$eq&quot;: [
                    {
                      &quot;$size&quot;: [
                        {
                          &quot;$ifNull&quot;: [
                            &quot;$$this.securityClassCodes&quot;,
                            { &quot;$const&quot;: [] }
                          ]
                        }
                      ]
                    },
                    { &quot;$const&quot;: 0 }
                  ]
                }
              ]
            }
          }
        }
      },
      &quot;nReturned&quot;: 354527,
      &quot;executionTimeMillisEstimate&quot;: 262130
    },
    {
      &quot;$match&quot;: {
        &quot;$or&quot;: [
          {
            &quot;$and&quot;: [
              { &quot;tags&quot;: { &quot;$size&quot;: 0 } },
              { &quot;tags&quot;: { &quot;$exists&quot;: true } },
              {
                &quot;_deletionStatus&quot;: {
                  &quot;$eq&quot;: &quot;false&quot;
                }
              }
            ]
          },
          {
            &quot;$and&quot;: [
              { &quot;tags.0&quot;: { &quot;$exists&quot;: true } },
              {
                &quot;_deletionStatus&quot;: {
                  &quot;$eq&quot;: &quot;false&quot;
                }
              }
            ]
          }
        ]
      },
      &quot;nReturned&quot;: 27,
      &quot;executionTimeMillisEstimate&quot;: 262209
    },
    {
      &quot;$unwind&quot;: {
        &quot;path&quot;: &quot;$tags&quot;,
        &quot;preserveNullAndEmptyArrays&quot;: true
      },
      &quot;nReturned&quot;: 27,
      &quot;executionTimeMillisEstimate&quot;: 262209
    },
    {
      &quot;$group&quot;: {
        &quot;_id&quot;: &quot;$tags&quot;,
        &quot;count&quot;: { &quot;$sum&quot;: { &quot;$const&quot;: 1 } }
      },
      &quot;nReturned&quot;: 1,
      &quot;executionTimeMillisEstimate&quot;: 262209
    },
    {
      &quot;$sort&quot;: { &quot;sortKey&quot;: { &quot;_id&quot;: 1 } },
      &quot;nReturned&quot;: 1,
      &quot;executionTimeMillisEstimate&quot;: 262209
    }
  ],
  &quot;ok&quot;: 1,
  &quot;operationTime&quot;: {
    &quot;$timestamp&quot;: &quot;7447054497592901633&quot;
  },
  &quot;$clusterTime&quot;: {
    &quot;clusterTime&quot;: {
      &quot;$timestamp&quot;: &quot;7447054523362705409&quot;
    },
    &quot;signature&quot;: {
      &quot;hash&quot;: &quot;3n2a3n+2tT/UTgSaN8nSM8QPEXo=&quot;,
      &quot;keyId&quot;: {
        &quot;low&quot;: 2,
        &quot;high&quot;: 1721435263,
        &quot;unsigned&quot;: false
      }
    }
  }
}</code></pre>
</div>
</div>
</div>
</div>

![[48197271878-image-20241211-073544.png]]



![[48197271878-image-20241211-073558.png]]

</td>
</tr>
</tbody>
</table>

</div>

# Problems

- Queries on non-indexed fields (e.g., sort, group, count) require **COLLSCAN** (scanning all entries) instead of the more efficient **IXSCAN** (using an index to quickly find relevant data).

- Some queried fields contain large data (e.g., file content from enrichment), making **COLLSCAN** resource-intensive and slow, consuming excessive RAM and CPU.

1.  **Lack of indexing**: Large collections like documents (with the number of documents potentially reaching hundreds of millions) are currently only using the default index, which is \_id, for queries.

2.  **All data fields are stored in the same collection**: All data fields (including those that are never used for querying) are stored in the same collection as the documents.

3.  **No deletion or migration of old data to coldline storage**: For example, document data from 10 years ago is still stored alongside current data. When querying, the system still scans data from 10 years ago unnecessarily, causing the collection's size to grow indefinitely day by day.

# Related user story:

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48197271878_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-136313" macro-id="04cda0fd-f54e-4c3b-83cd-1c848c1bd658" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-136313" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-136313</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>
