---
title: "Performance Wildfly vs Quarkus"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20535351017/Performance+Wildfly+vs+Quarkus
space: "LUZ"
topic: programming
relevance: 0.775
depth: 2.6
updated: 2021-08-11
attachments: 4
tags:
  - confluence
  - programming
  - space/luz
---

# Performance Wildfly vs Quarkus

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-08-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20535351017/Performance+Wildfly+vs+Quarkus)
> Relevance 0.775 · topic `programming`

**Resources**: 3G - 2CPU, -Xmx2048m

## **-n10 -c10, file1mb5.pdf**

<div>

<table style="width: 100.0%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Time</th>
<th>Wildfly</th>
<th>Quarkus</th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Concurrency Level: 10<br />
Time taken for tests: 10.687 seconds<br />
Complete requests: 10<br />
Failed requests: 0<br />
Total transferred: 1867750 bytes<br />
Total body sent: 15400640<br />
HTML transferred: 1866110 bytes<br />
Requests per second: 0.94 [#/sec] (mean)<br />
Time per request: 10687.205 [ms] (mean)<br />
Time per request: 1068.720 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 170.67 [Kbytes/sec] received<br />
1407.26 kb/s sent<br />
1577.93 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 1 0.4 0 2<br />
Processing: 3403 6512 1247.6 7091 7285<br />
Waiting: 2433 6321 1482.8 7006 7204<br />
Total: 3405 6512 1247.2 7091 7286<br />
ERROR: The median and mean for the initial connection time are more than twice the standard<br />
deviation apart. These results are NOT reliable.</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 7091<br />
66% 7168<br />
75% 7268<br />
80% 7284<br />
90% 7286<br />
95% 7286<br />
98% 7286<br />
99% 7286<br />
100% 7286 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>7208, 7262, 7187, 7016, 7066, 6892, 6684, 6558, 5173, 2572</p></td>
<td><p>Concurrency Level: 10<br />
Time taken for tests: 10.950 seconds<br />
Complete requests: 10<br />
Failed requests: 0<br />
Total transferred: 1868480 bytes<br />
Total body sent: 15757376<br />
HTML transferred: 1867120 bytes<br />
Requests per second: 0.91 [#/sec] (mean)<br />
Time per request: 10950.084 [ms] (mean)<br />
Time per request: 1095.008 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 166.64 [Kbytes/sec] received<br />
1405.29 kb/s sent<br />
1571.93 kb/s total<br />
<br />
Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 1 0.4 0 2<br />
Processing: 2636 7627 1761.0 8230 8382<br />
Waiting: 2211 7463 1852.0 8011 8311<br />
Total: 2638 7627 1760.6 8231 8383<br />
ERROR: The median and mean for the initial connection time are more than twice the standard<br />
deviation apart. These results are NOT reliable.<br />
<br />
Percentage of the requests served within a certain time (ms)<br />
50% 8231<br />
66% 8252<br />
75% 8331<br />
80% 8366<br />
90% 8383<br />
95% 8383<br />
98% 8383<br />
99% 8383<br />
100% 8383 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>8331, 8329, 8204, 8114, 8026, 8097, 7994, 7802, 7815, 2371</p></td>
</tr>
<tr>
<td>2</td>
<td><p>Concurrency Level: 10<br />
Time taken for tests: 5.633 seconds<br />
Complete requests: 10<br />
Failed requests: 0<br />
Total transferred: 1867750 bytes<br />
Total body sent: 16177600<br />
HTML transferred: 1866110 bytes<br />
Requests per second: 1.78 [#/sec] (mean)<br />
Time per request: 5633.333 [ms] (mean)<br />
Time per request: 563.333 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 323.78 [Kbytes/sec] received<br />
2804.46 kb/s sent<br />
3128.24 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 1 0.5 1 2<br />
Processing: 702 4299 1280.9 4774 4932<br />
Waiting: 682 4207 1260.8 4705 4860<br />
Total: 704 4300 1280.6 4775 4933</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 4775<br />
66% 4784<br />
75% 4852<br />
80% 4899<br />
90% 4933<br />
95% 4933<br />
98% 4933<br />
99% 4933<br />
100% 4933 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>4868, 4877, 4724, 4725, 4758, 4693, 4468, 4300, 4220, 686</p></td>
<td><p>Concurrency Level: 10<br />
Time taken for tests: 7.589 seconds<br />
Complete requests: 10<br />
Failed requests: 0<br />
Total transferred: 1868480 bytes<br />
Total body sent: 16508480<br />
HTML transferred: 1867120 bytes<br />
Requests per second: 1.32 [#/sec] (mean)<br />
Time per request: 7589.230 [ms] (mean)<br />
Time per request: 758.923 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 240.43 [Kbytes/sec] received<br />
2124.27 kb/s sent<br />
2364.70 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 1 0.3 0 1<br />
Processing: 1063 5686 1656.2 6294 6527<br />
Waiting: 1042 5570 1619.8 6150 6401<br />
Total: 1064 5686 1655.9 6294 6528<br />
ERROR: The median and mean for the initial connection time are more than twice the standard<br />
deviation apart. These results are NOT reliable.</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 6294<br />
66% 6406<br />
75% 6478<br />
80% 6506<br />
90% 6528<br />
95% 6528<br />
98% 6528<br />
99% 6528<br />
100% 6528 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>6499, 6476, 6386, 6380, 6281, 6086, 5983, 5877, 5469, 1052</p></td>
</tr>
<tr>
<td>3</td>
<td><p>Concurrency Level: 10<br />
Time taken for tests: 5.328 seconds<br />
Complete requests: 10<br />
Failed requests: 0<br />
Total transferred: 1867750 bytes<br />
Total body sent: 15594688<br />
HTML transferred: 1866110 bytes<br />
Requests per second: 1.88 [#/sec] (mean)<br />
Time per request: 5328.009 [ms] (mean)<br />
Time per request: 532.801 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 342.34 [Kbytes/sec] received<br />
2858.33 kb/s sent<br />
3200.66 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 1 0.4 0 2<br />
Processing: 723 4023 1179.9 4499 4605<br />
Waiting: 701 3938 1153.5 4397 4495<br />
Total: 724 4023 1179.5 4499 4606<br />
ERROR: The median and mean for the initial connection time are more than twice the standard<br />
deviation apart. These results are NOT reliable.</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 4499<br />
66% 4507<br />
75% 4527<br />
80% 4532<br />
90% 4606<br />
95% 4606<br />
98% 4606<br />
99% 4606<br />
100% 4606 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>4501, 4417, 4499, 4470, 4412, 4324, 4210, 4088, 3900, 707</p></td>
<td><p>Concurrency Level: 10<br />
Time taken for tests: 7.460 seconds<br />
Complete requests: 10<br />
Failed requests: 0<br />
Total transferred: 1868480 bytes<br />
Total body sent: 16315584<br />
HTML transferred: 1867120 bytes<br />
Requests per second: 1.34 [#/sec] (mean)<br />
Time per request: 7459.871 [ms] (mean)<br />
Time per request: 745.987 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 244.60 [Kbytes/sec] received<br />
2135.85 kb/s sent<br />
2380.45 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 1 0.4 1 2<br />
Processing: 1058 5658 1626.6 6218 6405<br />
Waiting: 1037 5540 1593.6 6054 6291<br />
Total: 1060 5659 1626.3 6218 6405</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 6218<br />
66% 6318<br />
75% 6324<br />
80% 6340<br />
90% 6405<br />
95% 6405<br />
98% 6405<br />
99% 6405<br />
100% 6405 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>6375, 6322, 6304, 6287, 6199, 6009, 5903, 5901, 5808, 1012</p></td>
</tr>
</tbody>
</table>

</div>

<div>

<table style="width: 100.0%;">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><div class="content-wrapper">

![[20535351017-image2021-8-11_14-24-35.png]]


</div></th>
<th><div class="content-wrapper">

![[20535351017-image2021-8-11_14-25-31.png]]


</div></th>
</tr>
&#10;</tbody>
</table>

</div>

## **-n30 -c30, file1mb5.pdf**

<div>

<table style="width: 100.0%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Time</th>
<th>Wildfly</th>
<th>Quarkus</th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Concurrency Level: 30<br />
Time taken for tests: 24.944 seconds<br />
Complete requests: 30<br />
Failed requests: 0<br />
Total transferred: 5603250 bytes<br />
Total body sent: 46775232<br />
HTML transferred: 5598330 bytes<br />
Requests per second: 1.20 [#/sec] (mean)<br />
Time per request: 24943.545 [ms] (mean)<br />
Time per request: 831.452 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 219.37 [Kbytes/sec] received<br />
1831.29 kb/s sent<br />
2050.67 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 4 1.2 4 5<br />
Processing: 4228 19202 3154.6 20415 20727<br />
Waiting: 3022 18613 3203.1 19497 20670<br />
Total: 4228 19206 3155.4 20419 20731</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 20419<br />
66% 20438<br />
75% 20540<br />
80% 20629<br />
90% 20724<br />
95% 20731<br />
98% 20731<br />
99% 20731<br />
100% 20731 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>20504, 20676, 20495, 19902, 20514, 19907, 19984, 19788, 19779, 19882, 19799, 20197, 19787, 19682, 20191, 18911, 19492, 19396, 19898, 18817, 19093, 19405, 19409, 19070, 18513, 16588, 16393, 16179, 16118, 3174</p></td>
<td><p>Concurrency Level: 30<br />
Time taken for tests: 27.365 seconds<br />
Complete requests: 30<br />
Failed requests: 0<br />
Total transferred: 5605440 bytes<br />
Total body sent: 46524992<br />
HTML transferred: 5601360 bytes<br />
Requests per second: 1.10 [#/sec] (mean)<br />
Time per request: 27364.950 [ms] (mean)<br />
Time per request: 912.165 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 200.04 [Kbytes/sec] received<br />
1660.32 kb/s sent<br />
1860.36 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 1 2 0.4 2 3<br />
Processing: 2863 22598 3925.3 23510 24619<br />
Waiting: 2309 22117 3940.0 23110 24379<br />
Total: 2865 22600 3925.3 23512 24620</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 23512<br />
66% 24106<br />
75% 24388<br />
80% 24489<br />
90% 24609<br />
95% 24611<br />
98% 24620<br />
99% 24620<br />
100% 24620 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>1110, 24526, 24592, 24487, 24484, 24210, 24388, 24013, 24186, 24078, 23990, 24004, 23758, 23494, 23294, 23182, 23306, 23294, 22975, 22993, 22693, 22489, 22314, 22286, 21980, 21525, 21275, 20666, 20577, 20063, 2528</p></td>
</tr>
<tr>
<td><br />
</td>
<td><p>Concurrency Level: 30<br />
Time taken for tests: 17.078 seconds<br />
Complete requests: 30<br />
Failed requests: 0<br />
Total transferred: 5603250 bytes<br />
Total body sent: 45444672<br />
HTML transferred: 5598330 bytes<br />
Requests per second: 1.76 [#/sec] (mean)<br />
Time per request: 17077.868 [ms] (mean)<br />
Time per request: 569.262 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 320.41 [Kbytes/sec] received<br />
2598.66 kb/s sent<br />
2919.07 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 5 1.6 5 7<br />
Processing: 891 14115 2856.9 15072 16189<br />
Waiting: 859 13852 2858.3 14720 16074<br />
Total: 892 14119 2857.8 15075 16195</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 15075<br />
66% 15478<br />
75% 15775<br />
80% 15967<br />
90% 15977<br />
95% 16069<br />
98% 16195<br />
99% 16195<br />
100% 16195 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>16127, 16049, 15957, 15771, 15592, 15708, 15689, 15755, 15474, 15544, 15275, 15027, 15103, 14899, 14781, 14486, 14311, 14378, 13893, 13606, 13901, 13197, 12899, 12901, 12618, 12417, 11899, 11686, 10802, 864</p></td>
<td><p>Concurrency Level: 30<br />
Time taken for tests: 19.082 seconds<br />
Complete requests: 30<br />
Failed requests: 0<br />
Total transferred: 5605440 bytes<br />
Total body sent: 46636224<br />
HTML transferred: 5601360 bytes<br />
Requests per second: 1.57 [#/sec] (mean)<br />
Time per request: 19081.619 [ms] (mean)<br />
Time per request: 636.054 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 286.88 [Kbytes/sec] received<br />
2386.76 kb/s sent<br />
2673.63 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 2 0.7 2 3<br />
Processing: 1134 16290 3072.7 17243 17959<br />
Waiting: 1098 16033 3032.3 16993 17695<br />
Total: 1136 16293 3072.9 17245 17961</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 17245<br />
66% 17649<br />
75% 17756<br />
80% 17846<br />
90% 17878<br />
95% 17885<br />
98% 17961<br />
99% 17961<br />
100% 17961 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>17888, 17833, 17802, 17789, 17836, 17691, 17683, 17700, 17507, 17599, 17592, 17491, 17489, 17213, 17102, 17089, 16907, 16799, 16718, 16226, 15968, 15801, 15638, 15311, 15207, 15180, 14957, 14807, 14049</p></td>
</tr>
<tr>
<td><br />
</td>
<td><p>Concurrency Level: 30<br />
Time taken for tests: 16.970 seconds<br />
Complete requests: 30<br />
Failed requests: 0<br />
Total transferred: 5603250 bytes<br />
Total body sent: 46136384<br />
HTML transferred: 5598330 bytes<br />
Requests per second: 1.77 [#/sec] (mean)<br />
Time per request: 16970.047 [ms] (mean)<br />
Time per request: 565.668 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 322.45 [Kbytes/sec] received<br />
2654.98 kb/s sent<br />
2977.42 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 15 11.9 16 29<br />
Processing: 803 14465 2840.4 15569 16202<br />
Waiting: 750 14214 2811.0 15276 15904<br />
Total: 803 14480 2841.5 15596 16204</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 15596<br />
66% 15797<br />
75% 15904<br />
80% 16003<br />
90% 16102<br />
95% 16107<br />
98% 16204<br />
99% 16204<br />
100% 16204 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>15899, 16009, 16087, 15990, 15893, 15897, 15966, 15878, 15873, 15702, 15684, 15670, 15578, 15488, 15497, 15292, 15220, 14989, 14667, 14086, 13886, 13816, 13786, 13403, 13457, 13306, 12472, 12596, 12359, 754</p></td>
<td><p>Concurrency Level: 30<br />
Time taken for tests: 17.419 seconds<br />
Complete requests: 30<br />
Failed requests: 0<br />
Total transferred: 5605440 bytes<br />
Total body sent: 46700992<br />
HTML transferred: 5601360 bytes<br />
Requests per second: 1.72 [#/sec] (mean)<br />
Time per request: 17418.977 [ms] (mean)<br />
Time per request: 580.633 [ms] (mean, across all concurrent requests)<br />
Transfer rate: 314.26 [Kbytes/sec] received<br />
2618.20 kb/s sent<br />
2932.46 kb/s total</p>
<p>Connection Times (ms)<br />
min mean[+/-sd] median max<br />
Connect: 0 2 0.8 2 4<br />
Processing: 904 14873 2838.0 15504 16517<br />
Waiting: 878 14562 2793.7 15157 16234<br />
Total: 906 14875 2838.1 15507 16520</p>
<p>Percentage of the requests served within a certain time (ms)<br />
50% 15507<br />
66% 16216<br />
75% 16326<br />
80% 16331<br />
90% 16426<br />
95% 16441<br />
98% 16520<br />
99% 16520<br />
100% 16520 (longest request)</p>
<p><strong>Thumbnail time generation</strong></p>
<p>16470, 16426, 16324, 16384, 16226, 16308, 16295, 16211, 16206, 16103, 16196, 15957, 15895, 15784, 15319, 15276, 15217, 14913, 14979, 14972, 14589, 14420, 14250, 13844, 13883, 13965, 13649, 13067, 13113, 890</p></td>
</tr>
</tbody>
</table>

</div>

<div>

<table style="width: 99.9305%;">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><div class="content-wrapper">

![[20535351017-image2021-8-11_15-22-37.png]]


</div></th>
<th><div class="content-wrapper">

![[20535351017-image2021-8-11_15-21-16.png]]


</div></th>
</tr>
&#10;</tbody>
</table>

</div>
