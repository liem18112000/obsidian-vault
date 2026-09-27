---
title: "Benchmark of luz-database (performance env)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47429484783/Benchmark+of+luz-database+performance+env
space: "FUT"
topic: infra
relevance: 0.777
depth: 2.61
updated: 2023-07-12
attachments: 2
tags:
  - confluence
  - infra
  - space/fut
---

# Benchmark of luz-database (performance env)

> [!info] Imported from Confluence
> Space **FUT** · updated 2023-07-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47429484783/Benchmark+of+luz-database+performance+env)
> Relevance 0.777 · topic `infra`

<div class="toc-macro client-side-toc-macro non-printable conf-macro output-block" hasbody="false" headerelements="H1,H2" macro-id="0b8a7dce-3210-4ee2-a60a-e8bcd878618f" macro-name="toc" numberedoutline="false" structure="list">

</div>

# Prepare database

- Database name: benchmarkdb

- Number of tuples: 5.000.000RENT_TIMESTAMP);

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cd473977-e1a1-44bd-afac-be150c24ff78" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
postgres@luz-database-5c98d5cfc4-r8mg7:/$ pgbench -i -s 50 benchmarkdb
creating tables...
5000000 of 5000000 tuples (100%) done (elapsed 5.97 s, remaining 0.00 s)
vacuum...
set primary keys...
done.
postgres@luz-database-5c98d5cfc4-r8mg7:/$ psql -d benchmarkdb;
psql (9.5.23)
Type "help" for help.

benchmarkdb=# \dt;
              List of relations
 Schema |       Name       | Type  |  Owner
--------+------------------+-------+----------
 public | pgbench_accounts | table | postgres
 public | pgbench_branches | table | postgres
 public | pgbench_history  | table | postgres
 public | pgbench_tellers  | table | postgres
(4 rows)
```

</div>

</div>

- Every transaction includes:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bbfe9d33-1443-4fee-a33d-eae88fccc4ce" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
UPDATE pgbench_accounts SET abalance = abalance + :delta WHERE aid = :aid;
SELECT abalance FROM pgbench_accounts WHERE aid = :aid;
UPDATE pgbench_tellers SET tbalance = tbalance + :delta WHERE tid = :tid;
UPDATE pgbench_branches SET bbalance = bbalance + :delta WHERE bid = :bid;
INSERT INTO pgbench_history (tid, bid, aid, delta, mtime) VALUES (:tid, :bid, :aid, :delta, CUR
```

</div>

</div>

# Scenario 1

Number of clients to connect: 96  
Number of transactions per client: 1000  
Number of transactions actually processed: 96000

## Scenario 1.1

Number of worker processes for `pgbench`: 96

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="31a95842-41b2-4df5-9372-63fad91d6419" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 07:59:43 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 96
number of threads: 96
number of transactions per client: 1000
number of transactions actually processed: 96000/96000
latency average: 96.435 ms
tps = 995.492225 (including connections establishing)
tps = 996.522146 (excluding connections establishing)
```

</div>

</div>

## Scenario 1.2

Number of worker processes for `pgbench`: 48

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6eb6f772-7a1b-419e-90b0-b8819e550b49" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 07:55:52 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 96
number of threads: 48
number of transactions per client: 1000
number of transactions actually processed: 96000/96000
latency average: 64.557 ms
tps = 1487.055530 (including connections establishing)
tps = 1488.026071 (excluding connections establishing)
```

</div>

</div>

## Scenario 1.3

Number of worker processes for `pgbench`: 32

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5b8b0096-1982-4449-acee-038853fc35f1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 07:57:50 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 96
number of threads: 32
number of transactions per client: 1000
number of transactions actually processed: 96000/96000
latency average: 63.575 ms
tps = 1510.032681 (including connections establishing)
tps = 1510.762362 (excluding connections establishing)

real    1m3.806s
user    0m7.032s
sys     0m10.788s
```

</div>

</div>

## Scenario 1.4

Number of worker processes for `pgbench`: 24

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b6b4f42e-85af-4c6f-8894-9baf57ed5a76" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 08:10:45 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 96
number of threads: 24
number of transactions per client: 1000
number of transactions actually processed: 96000/96000
latency average: 146.905 ms
tps = 653.482885 (including connections establishing)
tps = 653.585417 (excluding connections establishing)

real    2m27.238s
user    0m6.799s
sys     0m10.847s
```

</div>

</div>

## Scenario 1.5

Number of worker processes for `pgbench`: 16

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="16fe0e66-6c05-4096-8a04-72b933cd50fb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 08:02:09 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 96
number of threads: 16
number of transactions per client: 1000
number of transactions actually processed: 96000/96000
latency average: 145.081 ms
tps = 661.699618 (including connections establishing)
tps = 661.770480 (excluding connections establishing)
```

</div>

</div>

<div>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Clients** | **Threads** | **TPS** | **Average Latency (ms)** | **Number of transactions** | **CPUs** |
| 96 | 16 | 661 | 145 | 96000 | ~ 1 - 2 |
| 96 | 24 | 653 | 146 | 96000 | ~ 1 - 2 |
| 96 | 32 | 1510 | 63 | 96000 | ~ 1 - 2 |
| 96 | 48 | 1487 | 64 | 96000 | ~ 1 - 2 |
| 96 | 96 | 995 | 96 | 96000 | ~ 1 - 2 |

</div>

# Scenario 2

Number of clients to connect: 192  
Number of transactions per client: 1000  
Number of transactions actually processed: 192000

## Scenario 2.1

Number of worker processes for `pgbench`: 192

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7071630d-fd24-4900-a4d8-33396b5ea287" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 08:16:15 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 192
number of threads: 192
number of transactions per client: 1000
number of transactions actually processed: 192000/192000
latency average: 61.652 ms
tps = 3114.260414 (including connections establishing)
tps = 3123.114860 (excluding connections establishing)
```

</div>

</div>

## Scenario 2.2

Number of worker processes for `pgbench`: 96

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1a18113a-1bfc-4cb3-b982-5ae84c86093c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 08:14:41 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 192
number of threads: 96
number of transactions per client: 1000
number of transactions actually processed: 192000/192000
latency average: 46.726 ms
tps = 4109.040846 (including connections establishing)
tps = 4119.438693 (excluding connections establishing)

real    0m46.836s
user    0m13.881s
sys     0m20.565s
```

</div>

</div>

## Scenario 2.3

Number of worker processes for `pgbench`: 64

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5563b8c6-44f4-4632-8457-fb0cf45ff7ac" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 08:55:47 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 192
number of threads: 64
number of transactions per client: 1000
number of transactions actually processed: 192000/192000
latency average: 55.626 ms
tps = 3451.595047 (including connections establishing)
tps = 3454.682843 (excluding connections establishing)

real    0m56.052s
user    0m13.663s
sys     0m21.325s
```

</div>

</div>

<div>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Clients** | **Threads** | **TPS** | **Average Latency (ms)** | **Number of transactions** | **CPUs** |
| 192 | 64 | 3451 | 55 | 256000 | ~2 - 3 |
| 192 | 96 | 4109 | 46 | 256000 | ~2 - 3 |
| 192 | 192 | 3114 | 61 | 256000 | ~2 - 3 |

</div>

# Scenario 3

Number of clients to connect: 256  
Number of transactions per client: 1000  
Number of transactions actually processed: 256000

## Scenario 3.1

Number of worker processes for `pgbench`: 256

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f2ad318e-803c-41d9-8311-23fd042c3c4d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 08:23:19 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 256
number of threads: 256
number of transactions per client: 1000
number of transactions actually processed: 256000/256000
latency average: 74.596 ms
tps = 3431.816962 (including connections establishing)
tps = 3442.199398 (excluding connections establishing)

real    1m14.729s
user    0m18.461s
sys     0m30.728s
```

</div>

</div>

## Scenario 3.2

Number of worker processes for `pgbench`: 128

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="986b3fa5-14ee-4ba4-b07a-203439befea2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 08:21:10 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 256
number of threads: 128
number of transactions per client: 1000
number of transactions actually processed: 256000/256000
latency average: 61.198 ms
tps = 4183.140232 (including connections establishing)
tps = 4193.263096 (excluding connections establishing)

real    1m1.320s
user    0m18.515s
sys     0m28.167s
```

</div>

</div>

## Scenario 3.3

Number of worker processes for `pgbench`: 64

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="953c9d4b-a8d1-4157-b076-ffc72e1d8feb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 08:26:43 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 256
number of threads: 64
number of transactions per client: 1000
number of transactions actually processed: 256000/256000
latency average: 115.871 ms
tps = 2209.353715 (including connections establishing)
tps = 2210.623272 (excluding connections establishing)

real    1m56.061s
user    0m18.366s
sys     0m30.613s
```

</div>

</div>

<div>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Clients** | **Threads** | **TPS** | **Average Latency (ms)** | **Number of transactions** | **CPUs** |
| 256 | 64 | 2209 | 115 | 256000 | ~3 - 4 |
| 256 | 128 | 4183 | 61 | 256000 | ~3 - 5 |
| 256 | 256 | 3431 | 74 | 256000 | ~3 - 5 |

</div>

# Scenario 4

Every transaction has only 1 SELECT statement.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c6d2c61e-7430-469f-8116-0320c259d024" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SELECT abalance FROM pgbench_accounts WHERE aid = :aid;
```

</div>

</div>

## Scenario 4.1

Number of clients to connect: 256

Number of worker processes for `pgbench`: 128  
Number of transactions per client: 1000  
Number of transactions actually processed: 256000

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e1c3990d-acdf-4cb3-a6d8-de288db24caa" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
postgres@luz-database-7857df8c76-9qp59:~$ echo "Start benchmark...";date;time pgbench -c 256 -j 128 -t 1000 -l -S benchmarkdb;
Start benchmark...
Wed Jul 12 11:27:35 CEST 2023
starting vacuum...end.
transaction type: SELECT only
scaling factor: 50
query mode: simple
number of clients: 256
number of threads: 128
number of transactions per client: 1000
number of transactions actually processed: 256000/256000
latency average: 3.358 ms
tps = 76231.473295 (including connections establishing)
tps = 84459.004046 (excluding connections establishing)

real    0m4.125s
user    0m4.169s
sys     0m3.415s
postgres@luz-database-7857df8c76-9qp59:~$ echo "Start benchmark...";date;time pgbench -c 256 -j 128 -t 1000 -l -S benchmarkdb;
Start benchmark...
Wed Jul 12 11:27:42 CEST 2023
starting vacuum...end.
transaction type: SELECT only
scaling factor: 50
query mode: simple
number of clients: 256
number of threads: 128
number of transactions per client: 1000
number of transactions actually processed: 256000/256000
latency average: 3.301 ms
tps = 77550.353960 (including connections establishing)
tps = 85906.545992 (excluding connections establishing)

real    0m3.975s
user    0m4.103s
sys     0m3.339s
```

</div>

</div>

## Scenario 4.2

Number of clients to connect: 256

Number of worker processes for `pgbench`: 128  
Number of transactions per client: 1000  
Number of transactions actually processed: 2560000

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dbf40214-e210-47c5-a75e-2206e4396dd4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
postgres@luz-database-7857df8c76-9qp59:~$ echo "Start benchmark...";date;time pgbench -c 256 -j 128 -t 10000 -l -S benchmarkdb;
Start benchmark...
Wed Jul 12 11:25:23 CEST 2023
starting vacuum...end.
transaction type: SELECT only
scaling factor: 50
query mode: simple
number of clients: 256
number of threads: 128
number of transactions per client: 10000
number of transactions actually processed: 2560000/2560000
latency average: 2.886 ms
tps = 88715.041292 (including connections establishing)
tps = 89256.437390 (excluding connections establishing)

real    0m29.651s
user    0m38.851s
sys     0m33.412s
postgres@luz-database-7857df8c76-9qp59:~$ echo "Start benchmark...";date;time pgbench -c 256 -j 128 -t 10000 -l -S benchmarkdb;
Start benchmark...
Wed Jul 12 11:26:50 CEST 2023
starting vacuum...end.
transaction type: SELECT only
scaling factor: 50
query mode: simple
number of clients: 256
number of threads: 128
number of transactions per client: 10000
number of transactions actually processed: 2560000/2560000
latency average: 2.906 ms
tps = 88093.017417 (including connections establishing)
tps = 88806.817251 (excluding connections establishing)

real    0m30.312s
user    0m39.440s
sys     0m33.272s
```

</div>

</div>

<div>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Clients** | **Threads** | **TPS** | **Average Latency (ms)** | **Number of transactions** | **CPUs** |
| 256 | 128 | 77000 | 3.3 | 256000 | ~1 - 2 |
| 256 | 128 | 88000 | 2.9 | 2560000 | ~4 - 5 |

</div>

# Other scenarios

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f1d9bfa1-5f61-4d67-9c70-9f6ad3a20139" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Start benchmark...
Mon Jul 10 09:18:33 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 768
number of threads: 384
number of transactions per client: 1000
number of transactions actually processed: 768000/768000
latency average: 281.457 ms
tps = 2728.661651 (including connections establishing)
tps = 2734.242858 (excluding connections establishing)

real    4m41.667s
user    0m59.703s
sys     1m40.619s
Start benchmark...
Mon Jul 10 09:23:58 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 768
number of threads: 256
number of transactions per client: 1000
number of transactions actually processed: 768000/768000
latency average: 286.133 ms
tps = 2684.068349 (including connections establishing)
tps = 2687.187908 (excluding connections establishing)

real    4m46.569s
user    1m0.021s
sys     1m41.415s

Start benchmark...
Mon Jul 10 08:36:40 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 512
number of threads: 512
number of transactions per client: 1000
number of transactions actually processed: 512000/512000
latency average: 150.273 ms
tps = 3407.136434 (including connections establishing)
tps = 3418.220517 (excluding connections establishing)

real    2m30.695s
user    0m37.970s
sys     0m57.521s

Start benchmark...
Mon Jul 10 08:32:23 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 512
number of threads: 256
number of transactions per client: 1000
number of transactions actually processed: 512000/512000
latency average: 153.678 ms
tps = 3331.645105 (including connections establishing)
tps = 3337.989603 (excluding connections establishing)

real    2m33.965s
user    0m38.285s
sys     1m2.922s

Start benchmark...
Mon Jul 10 08:44:01 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 384
number of threads: 384
number of transactions per client: 1000
number of transactions actually processed: 384000/384000
latency average: 107.557 ms
tps = 3570.212103 (including connections establishing)
tps = 3584.851552 (excluding connections establishing)

real    1m47.872s
user    0m28.208s
sys     0m43.876s

Start benchmark...
Mon Jul 10 08:40:03 CEST 2023
starting vacuum...end.
transaction type: TPC-B (sort of)
scaling factor: 50
query mode: simple
number of clients: 384
number of threads: 192
number of transactions per client: 1000
number of transactions actually processed: 384000/384000
latency average: 107.441 ms
tps = 3574.064045 (including connections establishing)
tps = 3581.343396 (excluding connections establishing)

real    1m47.631s
user    0m28.327s
sys     0m44.707s
```

</div>

</div>

<div>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **Clients** | **Threads** | **TPS** | **Average Latency (ms)** | **Number of transactions** | **CPUs** |
| 384 | 192 | 3574 | 107 | 384000 | ~4 - 5 |
| 384 | 384 | 3570 | 107 | 384000 | ~4 - 5 |
| 512 | 256 | 3331 | 153 | 512000 | ~5 - 7 |
| 512 | 512 | 3407 | 150 | 512000 | ~5 - 7 |
| 768 | 256 | 2684 | 286 | 768000 | ~5 - 8 |
| 768 | 384 | 2728 | 281 | 768000 | ~5 - 8 |

</div>

# Resource usage during test


![[47429484783-image-20230710-070734.png]]
