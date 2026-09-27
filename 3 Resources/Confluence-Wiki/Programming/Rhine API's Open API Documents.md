---
title: "Rhine API's Open API Documents"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47438889033/Rhine+API+s+Open+API+Documents
space: "TS"
topic: programming
relevance: 0.762
depth: 2.73
updated: 2024-04-04
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# Rhine API's Open API Documents

> [!info] Imported from Confluence
> Space **TS** · updated 2024-04-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47438889033/Rhine+API+s+Open+API+Documents)
> Relevance 0.762 · topic `programming`

# Introduction

This page archived the Open API document from <a href="https://axonivy.atlassian.net/wiki/x/AYBSBgs" rel="nofollow">Rhine API v2</a>

# Open API

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c86c4229-b568-46c9-84c9-f3c3c8e40cd1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "openapi": "3.0.1",
  "info": {
    "title": "KLARA Rhine API",
    "contact": {
      "name": "KLARA Business AG",
      "url": "https://www.klara.ch/en/contact"
    },
    "license": {
      "name": "KLARA Software License",
      "url": "https://www.klara.ch/en/terms-and-conditions"
    },
    "version": "v2"
  },
  "servers": [
    {
      "url": "http://192.168.1.74/api/v2"
    }
  ],
  "paths": {
    "/consumer/{subscriptionId}": {
      "get": {
        "summary": "Receives JSON records from the named subscription",
        "operationId": "pollJsonWithLatestSchema",
        "parameters": [
          {
            "name": "subscriptionId",
            "in": "path",
            "description": "Identifier of the subscription to receive records from",
            "required": true,
            "schema": {
              "type": "string"
            },
            "example": "ai.sns.analyze.Event"
          },
          {
            "name": "limit",
            "in": "query",
            "description": "Maximum number of records to return",
            "schema": {
              "type": "integer",
              "format": "int32",
              "default": 3
            },
            "example": 16
          }
        ],
        "responses": {
          "200": {
            "description": "Records have been successfully received"
          },
          "400": {
            "description": "Invalid request",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorInfo"
                }
              },
              "*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorInfo"
                }
              }
            }
          },
          "404": {
            "description": "Invalid subscription"
          },
          "503": {
            "description": "Service unavailable"
          }
        }
      },
      "post": {
        "summary": "Acknowledges the receival of specific records",
        "operationId": "acknowledge",
        "parameters": [
          {
            "name": "subscriptionId",
            "in": "path",
            "description": "Identifier of the subscription to receive records from",
            "required": true,
            "schema": {
              "type": "string"
            },
            "example": "ai.sns.analyze.Event"
          }
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "type": "array",
                "items": {
                  "type": "string"
                }
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "Records have been successfully acknowledged"
          },
          "400": {
            "description": "Invalid request",
            "content": {
              "*/*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorInfo"
                }
              }
            }
          },
          "404": {
            "description": "Invalid subscription"
          },
          "503": {
            "description": "Service unavailable"
          }
        }
      }
    },
    "/consumer/{subscriptionId}:{version}": {
      "get": {
        "summary": "Receives JSON records from the named subscription",
        "operationId": "pollJson",
        "parameters": [
          {
            "name": "subscriptionId",
            "in": "path",
            "description": "Identifier of the subscription to receive records from",
            "required": true,
            "schema": {
              "type": "string"
            },
            "example": "ai.sns.analyze.Event"
          },
          {
            "name": "version",
            "in": "path",
            "description": "Version of the schema",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            },
            "example": 3
          },
          {
            "name": "limit",
            "in": "query",
            "description": "Maximum number of records to return",
            "schema": {
              "type": "integer",
              "format": "int32",
              "default": 3
            },
            "example": 16
          }
        ],
        "responses": {
          "200": {
            "description": "Records have been successfully received"
          },
          "400": {
            "description": "Invalid request",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorInfo"
                }
              },
              "*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorInfo"
                }
              }
            }
          },
          "404": {
            "description": "Invalid subscription"
          },
          "503": {
            "description": "Service unavailable"
          }
        }
      }
    },
    "/producer/{topic}:{version}": {
      "post": {
        "summary": "Publishes the JSON record to the named topic",
        "operationId": "sendJson",
        "parameters": [
          {
            "name": "topic",
            "in": "path",
            "description": "Name of the topic to sent the records to",
            "required": true,
            "schema": {
              "type": "string"
            },
            "example": "ai.sns.analyze.Event"
          },
          {
            "name": "version",
            "in": "path",
            "description": "Version of the schema",
            "required": true,
            "schema": {
              "type": "integer",
              "format": "int32"
            },
            "example": 3
          }
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "type": "object"
              }
            }
          }
        },
        "responses": {
          "204": {
            "description": "Record has been published to the named topic"
          },
          "400": {
            "description": "Invalid request",
            "content": {
              "*/*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorInfo"
                }
              }
            }
          },
          "404": {
            "description": "Invalid topic"
          },
          "503": {
            "description": "Service unavailable"
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "ErrorInfo": {
        "required": [
          "message",
          "type"
        ],
        "type": "object",
        "properties": {
          "message": {
            "type": "string",
            "description": "Human readable error message"
          },
          "type": {
            "type": "string",
            "description": "Error type",
            "enum": [
              "UNSPECIFIED",
              "INPUT",
              "IO",
              "SCHEMA",
              "TIMEOUT",
              "TOPIC"
            ]
          }
        }
      },
      "PollResult": {
        "required": [
          "schema",
          "schemaVersion",
          "size",
          "subscriptionId",
          "topicName"
        ],
        "type": "object",
        "properties": {
          "subscriptionId": {
            "type": "string",
            "description": "The subscription ID"
          },
          "topicName": {
            "type": "string",
            "description": "Name of the topic"
          },
          "schemaVersion": {
            "type": "integer",
            "description": "Version of the schema",
            "format": "int32"
          },
          "schema": {
            "type": "string",
            "description": "The Avro schema",
            "format": "binary"
          },
          "size": {
            "type": "integer",
            "description": "Number of elements",
            "format": "int32"
          },
          "elements": {
            "type": "object"
          }
        }
      }
    }
  }
}
```

</div>

</div>
