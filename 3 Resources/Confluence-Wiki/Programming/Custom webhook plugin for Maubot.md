---
ai_hash: 05d586a78b6f32be
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48674504768/Custom+webhook+plugin+for+Maubot
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Custom webhook plugin for Maubot
topic: programming
type: source
updated: 2025-09-19
---

# Custom webhook plugin for Maubot

> [!info] Imported from Confluence
> Space **TS** · updated 2025-09-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48674504768/Custom+webhook+plugin+for+Maubot)
> Relevance 0.738 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="8d1c3453-0bba-41a7-9f7b-02550e40b2ee" macro-name="toc">

</div>

# Motivation

- There is an existing webhook plugin for Maubot (<a href="https://github.com/jkhsjdhjs/maubot-webhook" class="external-link" rel="nofollow">Github repo</a>), but it's not very useful. The plugin simply exposes an endpoint and then sends a message to a room whenever the endpoint is triggered

<div id="expander-240015634" class="expand-container conf-macro output-block" hasbody="true" macro-id="e931483b-b8e0-4736-b5ff-2479b337e567" macro-name="expand">

<div id="expander-control-240015634" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">For more detail</span>

</div>

<div id="expander-content-240015634" class="expand-content expand-hidden">

- The plugin exposes an endpoint at `/_matrix/maubot/<instanceID>/send`

- We can trigger this endpoint to send a message to a room

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dad8f5c4-2fe9-45cd-a872-0debeabc15d5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
$ curl -X POST -H "Content-Type: application/json" -u abc:123 https://your.maubot.instance/_matrix/maubot/plugin/<instance ID>/send -d '
{
    "title": "This is a test message:",
    "list": [
        "Hello",
        "World!"
    ]
}'
```

</div>

</div>

- The users can not configure it by themselves, and the configurations are also limited

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="93aa69f2-3822-475c-a093-eb62c310e02b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
path: /send
method: POST
room: '!AAAAAAAAAAAAAAAAAA:example.com'
message: |
    **{{ json.title }}**
    {% for text in json.list %}
    - {{ text }}
    {% endfor %}
message_format: markdown
message_type: m.text
auth_type: Basic
auth_token: abc:123
force_json: false
ignore_empty_messages: false
```

</div>

</div>

</div>

</div>

- The process to create a plugin in Maubot is straightforward, and the documentation is also well-written.  
  Documentation: <a href="https://docs.mau.fi/maubot/dev/getting-started.html" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.mau.fi/maubot/dev/getting-started.html</a>

- With our own plugin, we can implement whatever functionality we need; we don't have to rely on 3rd-party plugins.

# Purpose

- Create a custom webhook plugin that allows users to register webhook URLs and automatically forwards all messages in a room to the registered webhooks.

- The users can send commands in chat to interact with the plugin bot

- The admin of Maubot can configure the message’s data structure to forward to webhooks, as well as the template of response messages

# Config & Usage

<div id="expander-999562082" class="expand-container conf-macro output-block" hasbody="true" macro-id="e141965b-7e45-4a09-8ee0-8b088e8c109f" macro-name="expand">

<div id="expander-control-999562082" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Basic commands</span>

</div>

<div id="expander-content-999562082" class="expand-content expand-hidden">

- `!webhook register <url>` - Register a new webhook URL for your user in the current room

- `!webhook list` - List all webhooks in the current room (shows ID, URL, and status)

- `!webhook unregister [id|url]` - **Delete** webhook(s) permanently from database

  - `!webhook unregister` - Delete all your webhooks

  - `!webhook unregister 123` - Delete webhook with ID 123

  - `!webhook unregister https://...` - Delete webhook with specific URL

- `!webhook disable [id|url]` - **Disable** webhook(s) temporarily (keeps in database)

  - `!webhook disable` - Disable all your active webhooks

  - `!webhook disable 123` - Disable webhook with ID 123

  - `!webhook disable https://...` - Disable webhook with specific URL

- `!webhook enable [id|url]` - **Enable** previously disabled webhook(s)

  - `!webhook enable` - Enable all your disabled webhooks

  - `!webhook enable 123` - Enable webhook with ID 123

  - `!webhook enable https://...` - Enable webhook with specific URL

- `!webhook create` - Create a new webhook URL endpoint to be used by external services

- `!webhook delete <id>` - Delete a webhook URL endpoint

</div>

</div>

<div id="expander-650129721" class="expand-container conf-macro output-block" hasbody="true" macro-id="7232a2fb-72f2-48c4-8ef9-cd50a27f125d" macro-name="expand">

<div id="expander-control-650129721" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Maubot admin example config</span>

</div>

<div id="expander-content-650129721" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7a5d880a-0b56-48c5-8510-48a8ced2c96d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Webhook Plugin Base Configuration

# Webhook request timeout in seconds
webhook_timeout: 30

# Maximum number of retries for failed webhook requests
max_webhook_retries: 3

# User agent string for webhook requests
webhook_user_agent: Maubot-Webhook-Plugin/1.0

# Message data structure sent to webhooks
# Available variables: event_id, room_id, sender, timestamp, message_type, body, formatted_body, format
message_data_template:
    event_id: '{event_id}'
    room_id: '{room_id}'
    sender: '{sender}'
    timestamp: '{timestamp}'
    message_type: '{message_type}'
    message_content: '{body}'
    formatted_body: '{formatted_body}'
    format: '{format}'
custom_fields:
    source: maubot-webhook
    version: '1.0'
response_template: '{response}'

# Whether to include null/empty fields in the webhook payload
include_empty_fields: false
```

</div>

</div>

</div>

</div>

# Outcome

### Code repository: <a href="https://bitbucket.org/axonivy-prod/epost_maubot_webhook_plugin/src/main/" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/epost_maubot_webhook_plugin/src/main/</a>

### POC

- Register a webhook URL to forward all messages to

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[48674504768-chrome_fnjeA9bFSS.mp4|chrome_fnjeA9bFSS.mp4]]</span>

- Create a webhook URL that can be used to send a message to that room

<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size">[[48674504768-chrome_R69bRZz5SU.mp4|chrome_R69bRZz5SU.mp4]]</span>

# Open questions

1.  After a user registers a webhook URL, should only the messages from that user be forwarded or all messages in that room? Currently, the plugin will forward all messages in the room

2.  Which roles should we allow to interact with the Webhook bot?

3.  Currently, only the Maubot admin can configure the message data structure for webhook forwarding, and this structure applies to all webhooks. However, in practice, each webhook might require a different data structure in the request body. Should we allow users to configure the data structure through chat commands dynamically?

4.  The concept of a webhook is one-way data sharing. This means the webhook may or may not have a response. In case the webhook has no response, how should we handle this? Simply does not send any response message or show some notifications?

%% ai-graph-start %%

**Related notes:**
- _(none above threshold)_

%% ai-graph-end %%