---
title: "Database scaling case study"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47183790501/Database+scaling+case+study
space: "FUT"
topic: infra
relevance: 0.703
depth: 2.25
updated: 2022-09-16
attachments: 0
tags:
  - confluence
  - infra
  - space/fut
---

# Database scaling case study

> [!info] Imported from Confluence
> Space **FUT** · updated 2022-09-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47183790501/Database+scaling+case+study)
> Relevance 0.703 · topic `infra`

This is a cool article that explains the real cases: <a href="https://www.freecodecamp.org/news/understanding-database-scaling-patterns/" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.freecodecamp.org/news/understanding-database-scaling-patterns/</a>

## Summary

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p>Level</p></th>
<th><p>Name</p></th>
<th><p>What</p></th>
</tr>
&#10;<tr>
<td><p>Pattern 1</p></td>
<td><p>Query Optimization &amp; Connection Pool implementation</p></td>
<td><p>Apply cache</p>
<p>Optimize queries</p>
<p>Optimize database connection pool</p></td>
</tr>
<tr>
<td><p>Pattern 2</p></td>
<td><p>Vertical Scaling or Scale Up</p></td>
<td><p>Upgrading hardware</p></td>
</tr>
<tr>
<td><p>Pattern 3</p></td>
<td><p>Command Query Responsibility Segregation (CQRS)</p></td>
<td><p>Apply Database Replication</p></td>
</tr>
<tr>
<td><p>Pattern 4</p></td>
<td><p>Multi Primary Replication</p></td>
<td><p>It’s like Database Replication but write request is distributed to replica.</p></td>
</tr>
<tr>
<td><p>Pattern 5</p></td>
<td><p>Partitioning</p></td>
<td><p>Put specific tables in separate database schema</p>
<p>Or put specific databases in separate machine</p></td>
</tr>
<tr>
<td><p>Pattern 6</p></td>
<td><p>Horizontal Scaling (Database sharding)</p></td>
<td><p>All machines have the same set of database schemas, tables but just hold a part of data.</p></td>
</tr>
<tr>
<td><p>Pattern 7</p></td>
<td><p>Data Centre Wise Partition</p></td>
<td><p>Set up data centers to distribute traffic across them</p></td>
</tr>
</tbody>
</table>

</div>

## Conclusions

Go on with Database Replication
