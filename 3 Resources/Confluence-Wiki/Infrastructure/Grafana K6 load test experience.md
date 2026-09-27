---
title: "Grafana K6 load test experience"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48883532608/Grafana+K6+load+test+experience
space: "TS"
topic: infra
relevance: 0.716
depth: 2.67
updated: 2025-11-17
attachments: 0
tags:
  - confluence
  - infra
  - space/ts
---

# Grafana K6 load test experience

> [!info] Imported from Confluence
> Space **TS** · updated 2025-11-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48883532608/Grafana+K6+load+test+experience)
> Relevance 0.716 · topic `infra`

1.  **Save memory by using SharedArray when loading common data**

- Instead of

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="51fd9a2a-846a-451b-84c6-75c54eda8662" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import { SharedArray } from 'k6/data';

// By this way, it will happen many times by the number of VUs, which could cause memory leak issue.
const f = JSON.parse(open('./somefile.json'));

export default function () {
  const element = data[Math.floor(Math.random() * data.length)];
  // do something with element
}
```

</div>

</div>

- Should be

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3d50155a-16a4-4631-b328-9505d1d0975b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import { SharedArray } from 'k6/data';

const data = new SharedArray('some name', function () {
  // All heavy work (opening and processing big files for example) should be done inside here.
  // This way it will happen only once and the result will be shared between all VUs, saving time and memory.
  const f = JSON.parse(open('./somefile.json'));
  return f; // f must be an array
});

export default function () {
  const element = data[Math.floor(Math.random() * data.length)];
  // do something with element
}
```

</div>

</div>

2.  **Saving cost by using --local-execute**

- With a large amount of VUs, if your local machine can handle it, you can add flag --local-execute to make your local run the heavy code

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0fdb85a0-b0a4-4a93-adff-8212fb8e4102" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  k6 cloud run --local-execute ./src/test-script.ts
  ```

  </div>

  </div>

3.  **Save CPU consumption when retrieving data by using Record or Map**

- Sometimes you need to extract data from a list. From my experience, it’s better to retrieve data from a Record or Map; it's better to run a loop over the array to find the data.

  - Instead of

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d32cde28-9aaf-471e-bd93-80e786c47319" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
function loadData(): Person[];
const data = loadData();

const personWithId10 = data.filter(p -> p.id === 10)
```

</div>

</div>

Should be

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="272acd26-4e9b-4471-bc84-4a2d3422eb4f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
function loadData(): Record<number, Person>;
const data = loadData();

const personWithId10 = data[10]
```

</div>

</div>
