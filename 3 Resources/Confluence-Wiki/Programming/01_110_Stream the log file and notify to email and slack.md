---
title: "01_110_Stream the log file and notify to email and slack."
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134409125/01_110_Stream+the+log+file+and+notify+to+email+and+slack.
space: "GRAVITY"
topic: programming
relevance: 0.827
depth: 3
updated: 2022-06-24
attachments: 4
tags:
  - confluence
  - programming
  - space/gravity
---

# 01_110_Stream the log file and notify to email and slack.

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2022-06-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134409125/01_110_Stream+the+log+file+and+notify+to+email+and+slack.)
> Relevance 0.827 · topic `programming`

## 1. Clone the project at <a href="https://bitbucket.org/axonivy-prod/cob_log_monitor" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/cob_log_monitor</a>

## 2. Install the project

> # from cob_log_monitor
>     cd lookie
>     npm  install

## 3. Update your **preset/configuration.json **file

> {
>         "host": "<HOST_NAME>",
>         "port": "<APPLICATION_PORT>",
>         "userName": "<EMAIL_SENDER>",
>         "password": "<EMAIL_PASSWORD>",
>         "from": "<EMAIL_SENDER>",
>         "to": "<EMAIL_RECEIVER>",
>         "subject": "<EMAIL_SUBJECT>",
>         "content": "<EMAIL_CONTENT>",
>         "blackLists": [ "<EXCEPTED_EXCEPTION>" ],
>         "slackAccessToken": "<SLACK_CHANNEL_ACCESS_TOKEN>",
>         "slackChannel": "<SLACK_CHANNEL>",
>         "teamsWebhook": "<MICROSOFT_TEAMS_WEBHOOK>",
>         "log": [ "<LOG_FILE>" ],
>         "environment": "<ENVIRONMENT_NAME>",
>         "application": "<APPLICATION_NAME>",
>         "rows": 50
>     }

For more details, please refer <a href="https://bitbucket.org/axonivy-prod/cob_log_monitor/src/master/lookie/README.md" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/cob_log_monitor/src/master/lookie/README.md</a>

## 4. Start the application

> node index.js --ui-highlight --mail 1 --slack 1 --teams 1

`--mail`is the parameter use to enable or disable notification via email. Set`0`to disable, set`1`to enable.

`--slack`is the parameter use to enable or disable notification via Slack. Set`0`to disable, set`1`to enable.

`--teams`is the parameter use to enable or disable notification via Microsoft Teams. Set`0`to disable, set`1`to enable.

## 5. Open your browser and access

<span class="legacy-color-text-blue3">\<configurated_hostname\>:\<configurated_port\>/tail</span>

<span class="legacy-color-text-blue3">Example: 192.168.1.10:9002/tail</span>
