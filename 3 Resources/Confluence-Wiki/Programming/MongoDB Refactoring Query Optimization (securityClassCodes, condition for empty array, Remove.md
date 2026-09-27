---
title: "[MongoDB Refactoring] Query Optimization (securityClassCodes, condition for empty array, Remove useless stage)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48671129835/MongoDB+Refactoring+Query+Optimization+securityClassCodes+condition+for+empty+array+Remove+useless+stage
space: "TK"
topic: programming
relevance: 0.818
depth: 3
updated: 2025-12-09
attachments: 12
tags:
  - confluence
  - programming
  - space/tk
---

# [MongoDB Refactoring] Query Optimization (securityClassCodes, condition for empty array, Remove useless stage)

> [!info] Imported from Confluence
> Space **TK** · updated 2025-12-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48671129835/MongoDB+Refactoring+Query+Optimization+securityClassCodes+condition+for+empty+array+Remove+useless+stage)
> Relevance 0.818 · topic `programming`

## Test Case 1: Search document with query and without security class

**Description**: Search document by using query with tenant have no security class in token

**Author**: Tuan Anh

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
<th><p><strong>Step</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Excepted Behavior</strong></p></th>
<th><p><strong>Actual Result</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Get call jwt-service token</p></td>
<td><p>Receive token without security class</p>
<p>{<br />
….<br />
"exp": 1757952770,<br />
"iat": 1757909570,<br />
"security_classes": []<br />
}</p></td>
<td>

![[48671129835-image-20250915-041359.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Call API search luz-docs with upon token with query search</p>

![[48671129835-image-20250911-023230.png]]

</td>
<td><p>Query send to luz-docs without supplying empty arrays to operators like $in, $setIntersection, etc.</p></td>
<td><p>Query send to luz-jsonstore</p>

![[48671129835-image-20250918-032645.png]]


<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="0c593eff-ff2c-4c89-b16d-d1b080eb1bba" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;$and&quot;: [
        {
            &quot;$and&quot;: [
                {
                    &quot;$or&quot;: [
                        {
                            &quot;folderIds.0&quot;: {
                                &quot;$exists&quot;: true
                            }
                        },
                        {
                            &quot;isStored&quot;: true
                        }
                    ]
                }
            ]
        },
        {
            &quot;$or&quot;: [
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$size&quot;: 0
                    }
                },
                {
                    &quot;securityClassCodes&quot;: null
                }
            ]
        },
        {
            &quot;_isBeingCreated&quot;: {
                &quot;$ne&quot;: true
            }
        },
        {
            &quot;$or&quot;: [
                {
                    &quot;personal&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;personal&quot;: {
                        &quot;$ne&quot;: true
                    }
                }
            ]
        },
        {
            &quot;$or&quot;: [
                {
                    &quot;_deletionStatus&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;_deletionStatus&quot;: &quot;false&quot;
                }
            ]
        }
    ]
}</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

## Test Case 2: Search document with facets and without security class

**Description**: Search document by using facets with tenant have no security class in token

**Author**: Tuan Anh

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
<th><p><strong>Step</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Excepted Behavior</strong></p></th>
<th><p><strong>Actual Result</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Get call jwt-service token</p></td>
<td><p>Receive token without security class</p>
<p>{<br />
….<br />
"exp": 1757952770,<br />
"iat": 1757909570,<br />
"security_classes": []<br />
}</p></td>
<td>

![[48671129835-image-20250915-041359.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Call API search luz-docs with upon token with query search</p>

![[48671129835-image-20250911-023517.png]]

</td>
<td><p>Query send to luz-docs without supplying empty arrays to operators like $in, $setIntersection, etc.</p></td>
<td><p>Query send to luz-jsonstore</p>

![[48671129835-image-20250918-032555.png]]


<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="38d71959-0b6e-4833-ab88-b2110b829518" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
        &quot;$match&quot;: {
            &quot;_isBeingCreated&quot;: {
                &quot;$ne&quot;: true
            }
        }
    },
    {
        &quot;$match&quot;: {
            &quot;$or&quot;: [
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$size&quot;: 0
                    }
                },
                {
                    &quot;securityClassCodes&quot;: null
                }
            ]
        }
    },
    {
        &quot;$lookup&quot;: {
            &quot;from&quot;: &quot;folders&quot;,
            &quot;let&quot;: {
                &quot;folderIds&quot;: &quot;$folderIds&quot;
            },
            &quot;pipeline&quot;: [
                {
                    &quot;$match&quot;: {
                        &quot;$expr&quot;: {
                            &quot;$in&quot;: [
                                {
                                    &quot;$toString&quot;: &quot;$_id&quot;
                                },
                                &quot;$$folderIds&quot;
                            ]
                        }
                    }
                },
                {
                    &quot;$project&quot;: {
                        &quot;_id&quot;: {
                            &quot;$toString&quot;: &quot;$_id&quot;
                        },
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
            ],
            &quot;as&quot;: &quot;_folders&quot;
        }
    },
    {
        &quot;$match&quot;: {
            &quot;$or&quot;: [
                {
                    &quot;_folders.securityClassCodes&quot;: []
                },
                {
                    &quot;_folders.securityClassCodes&quot;: {
                        &quot;$exists&quot;: false
                    }
                }
            ]
        }
    },
    {
        &quot;$addFields&quot;: {
            &quot;_folders&quot;: {
                &quot;$filter&quot;: {
                    &quot;input&quot;: &quot;$_folders&quot;,
                    &quot;cond&quot;: {
                        &quot;$eq&quot;: [
                            {
                                &quot;$size&quot;: {
                                    &quot;$ifNull&quot;: [
                                        &quot;$$this.securityClassCodes&quot;,
                                        []
                                    ]
                                }
                            },
                            0
                        ]
                    }
                }
            }
        }
    },
    {
        &quot;$match&quot;: {
            &quot;$and&quot;: [
                {
                    &quot;senderTenantId&quot;: {
                        &quot;$exists&quot;: true
                    }
                },
                {
                    &quot;senderCompanyId&quot;: {
                        &quot;$exists&quot;: true
                    }
                },
                {
                    &quot;origin&quot;: {
                        &quot;$exists&quot;: true
                    }
                },
                {
                    &quot;_deletionStatus&quot;: &quot;false&quot;
                }
            ]
        }
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
            &quot;count&quot;: {
                &quot;$sum&quot;: 1
            }
        }
    },
    {
        &quot;$sort&quot;: {
            &quot;_id.senderTenantId&quot;: 1
        }
    },
    {
        &quot;$skip&quot;: 0
    },
    {
        &quot;$limit&quot;: 2147483647
    }
]</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

## Test Case 3: Search document with query and with security class

**Description**: Search document by using query with tenant have security class in token

**Author**: Tuan Anh

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
<th><p><strong>Step</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Excepted Behavior</strong></p></th>
<th><p><strong>Actual Result</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Get call jwt-service token</p></td>
<td><p>Receive token with security class</p>
<p>{</p>
<p>….<br />
"exp": 1757954352,<br />
"iat": 1757911152,<br />
"security_classes": [<br />
"C",<br />
"B",<br />
"AAA_1",<br />
"A",<br />
"D",<br />
"E"<br />
]<br />
}</p></td>
<td>

![[48671129835-image-20250915-044012.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Call API search luz-docs with upon token with query search</p>

![[48671129835-image-20250911-023230.png]]

</td>
<td><p>Query send to luz-docs with security class information query</p></td>
<td><p>Query send to luz-jsonstore</p>

![[48671129835-image-20250918-032923.png]]


<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="018dafd2-ced6-4615-ba06-34d183bc29da" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;$and&quot;: [
        {
            &quot;$and&quot;: [
                {
                    &quot;$or&quot;: [
                        {
                            &quot;folderIds.0&quot;: {
                                &quot;$exists&quot;: true
                            }
                        },
                        {
                            &quot;isStored&quot;: true
                        }
                    ]
                }
            ]
        },
        {
            &quot;$or&quot;: [
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$size&quot;: 0
                    }
                },
                {
                    &quot;securityClassCodes&quot;: null
                },
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$in&quot;: [
                            &quot;C&quot;,
                            &quot;B&quot;,
                            &quot;AAA_1&quot;,
                            &quot;A&quot;,
                            &quot;D&quot;,
                            &quot;E&quot;
                        ]
                    }
                }
            ]
        },
        {
            &quot;_isBeingCreated&quot;: {
                &quot;$ne&quot;: true
            }
        },
        {
            &quot;$or&quot;: [
                {
                    &quot;personal&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;personal&quot;: {
                        &quot;$ne&quot;: true
                    }
                }
            ]
        },
        {
            &quot;$or&quot;: [
                {
                    &quot;_deletionStatus&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;_deletionStatus&quot;: &quot;false&quot;
                }
            ]
        }
    ]
}</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

## Test Case 4: Search document with facets and with security class

**Description**: Search document by using facets with tenant have security class in token

**Author**: Tuan Anh

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
<th><p><strong>Step</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Excepted Behavior</strong></p></th>
<th><p><strong>Actual Result</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Get call jwt-service token</p></td>
<td><p>Receive token with security class</p>
<p>{</p>
<p>….<br />
"exp": 1757954352,<br />
"iat": 1757911152,<br />
"security_classes": [<br />
"C",<br />
"B",<br />
"AAA_1",<br />
"A",<br />
"D",<br />
"E"<br />
]<br />
}</p></td>
<td>

![[48671129835-image-20250915-044012.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Call API search luz-docs with upon token with query search</p>

![[48671129835-image-20250911-024048.png]]

</td>
<td><p>Query send to luz-docs with security class information query</p></td>
<td><p>Query send to luz-jsonstore</p>

![[48671129835-image-20250918-033012.png]]


<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="27a03b1e-391a-4399-a3c0-840591be9050" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
        &quot;$match&quot;: {
            &quot;_isBeingCreated&quot;: {
                &quot;$ne&quot;: true
            }
        }
    },
    {
        &quot;$match&quot;: {
            &quot;$or&quot;: [
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$size&quot;: 0
                    }
                },
                {
                    &quot;securityClassCodes&quot;: null
                },
                {
                    &quot;securityClassCodes&quot;: {
                        &quot;$in&quot;: [
                            &quot;C&quot;,
                            &quot;B&quot;,
                            &quot;AAA_1&quot;,
                            &quot;A&quot;,
                            &quot;D&quot;,
                            &quot;E&quot;
                        ]
                    }
                }
            ]
        }
    },
    {
        &quot;$lookup&quot;: {
            &quot;from&quot;: &quot;folders&quot;,
            &quot;let&quot;: {
                &quot;folderIds&quot;: &quot;$folderIds&quot;
            },
            &quot;pipeline&quot;: [
                {
                    &quot;$match&quot;: {
                        &quot;$expr&quot;: {
                            &quot;$in&quot;: [
                                {
                                    &quot;$toString&quot;: &quot;$_id&quot;
                                },
                                &quot;$$folderIds&quot;
                            ]
                        }
                    }
                },
                {
                    &quot;$project&quot;: {
                        &quot;_id&quot;: {
                            &quot;$toString&quot;: &quot;$_id&quot;
                        },
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
            ],
            &quot;as&quot;: &quot;_folders&quot;
        }
    },
    {
        &quot;$match&quot;: {
            &quot;$or&quot;: [
                {
                    &quot;_folders.securityClassCodes&quot;: []
                },
                {
                    &quot;_folders.securityClassCodes&quot;: {
                        &quot;$exists&quot;: false
                    }
                },
                {
                    &quot;_folders.securityClassCodes&quot;: {
                        &quot;$in&quot;: [
                            &quot;C&quot;,
                            &quot;B&quot;,
                            &quot;AAA_1&quot;,
                            &quot;A&quot;,
                            &quot;D&quot;,
                            &quot;E&quot;
                        ]
                    }
                }
            ]
        }
    },
    {
        &quot;$addFields&quot;: {
            &quot;_folders&quot;: {
                &quot;$filter&quot;: {
                    &quot;input&quot;: &quot;$_folders&quot;,
                    &quot;cond&quot;: {
                        &quot;$or&quot;: [
                            {
                                &quot;$gt&quot;: [
                                    {
                                        &quot;$size&quot;: {
                                            &quot;$setIntersection&quot;: [
                                                [
                                                    &quot;C&quot;,
                                                    &quot;B&quot;,
                                                    &quot;AAA_1&quot;,
                                                    &quot;A&quot;,
                                                    &quot;D&quot;,
                                                    &quot;E&quot;
                                                ],
                                                {
                                                    &quot;$ifNull&quot;: [
                                                        &quot;$$this.securityClassCodes&quot;,
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
                                &quot;$eq&quot;: [
                                    {
                                        &quot;$size&quot;: {
                                            &quot;$ifNull&quot;: [
                                                &quot;$$this.securityClassCodes&quot;,
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
        &quot;$match&quot;: {
            &quot;$and&quot;: [
                {
                    &quot;senderTenantId&quot;: {
                        &quot;$exists&quot;: true
                    }
                },
                {
                    &quot;senderCompanyId&quot;: {
                        &quot;$exists&quot;: true
                    }
                },
                {
                    &quot;origin&quot;: {
                        &quot;$exists&quot;: true
                    }
                },
                {
                    &quot;_deletionStatus&quot;: &quot;false&quot;
                }
            ]
        }
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
            &quot;count&quot;: {
                &quot;$sum&quot;: 1
            }
        }
    },
    {
        &quot;$sort&quot;: {
            &quot;_id.senderTenantId&quot;: 1
        }
    },
    {
        &quot;$skip&quot;: 0
    },
    {
        &quot;$limit&quot;: 2147483647
    }
]</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>
