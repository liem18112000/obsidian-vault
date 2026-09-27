---
title: "Concept Intermediate data storage"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47518548246/Concept+Intermediate+data+storage
space: "LUZ"
topic: architecture
relevance: 0.9
depth: 3
updated: 2023-10-12
attachments: 0
tags:
  - confluence
  - architecture
  - space/luz
---

# Concept Intermediate data storage

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-10-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47518548246/Concept+Intermediate+data+storage)
> Relevance 0.9 · topic `architecture`

Next steps:

- Architecture design

  - Based on AHV

  - Get data

  - Create json

  - Extension class diagram (for intermediate file)

  - Extension state machine with team Next ([ELM5 - State Diagram](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47409856669/ELM5+-+State+Diagram) )

  - File Handling

1.  Trigger

    1.  When data complete

    2.  When preparing data

        1.  Initial preparing data

        2.  User triggered data preparing

    3.  Periodicity

        1.  Monthly

        2.  Yearly

        3.  Ad-hoc

2.  Datenhaltung

    1.  JSON

        1.  Attributes

            1.  Domain

            2.  Period

            3.  Flag: Original - overriden

            4.  State (from state machine)

                1.  Not every state must be handeled

            5.  Version

3.  State Machine

    1.  Error handling

    2.  Status

        1.  Prepare data

        2.  Data prepared

        3.  Data transmitted

4.  Prepare Data

    1.  Which domains to start

        1.  AHV

            1.  Entry

                1.  Not sealed data, but calculated

            2.  Leave

                1.  Not sealed data, but calculated

            3.  Monthly

                1.  Sealed data

            4.  Yearly

                1.  Sealed data

            5.  Modifications

                1.  Not sealed data, but calculated

            6.  Changes of prepared data

        2.  UVG

        3.  Tax - Salary Certificate (respect changes in versioning and wait for fields)

        4.  TCrb (Tax Crossboarder Commuter)

    2.  Later

        1.  TAtSrc - TAS/QST (changes and errors in current version)

        2.  FAK (will be modified for ELM.5)

        3.  KTG

        4.  UVGZ

        5.  BVG (will be modiied for ELM.5 and changes)

        6.  Statistics

    3.  All employee data changes can be detected in the data of company or employee. It is not required to compare  JSON objects

    4.  Format

        1.  For JSON data will be prepared the way it must be transmitted in the xml, e.g. time in industrial format (8:15 --\> 8.25)

    5.  Attributes JSON

        1.  Required

        2.  Optional

            1.  Try to fillup all fields which are available

            2.  If no value is available --\> NULL

    6.  Business Cases

        1.  EMA

            1.  Entry

                1.  Trigger when enter a new employee

                2.  Create 1 json object per entry

                3.  State = PREPARED

                4.  1 xml will be created with all prepared entry json objects for the period to transmit

            2.  Modifications

            3.  Leave (Austritt)

        2.  Monthly Transmission

 

1.  File Handling

    1.  Versioning

    2.  Json and XML relation

    3.  Archiving
