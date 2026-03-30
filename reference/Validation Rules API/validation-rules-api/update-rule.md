---
title: Update rule
excerpt: >-
  Update the specifications of a rule which was previously created. **Note**:
  When you enter the API token and click **Try it**, the rule is updated.
api:
  file: validation-rules-api.json
  operationId: update-rule
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
[block:api-header]
{
  "title": "Prerequisites"
}
[/block]
- The [API V2.0 token](https://support.appsflyer.com/hc/en-us/articles/360004562377) from your AppsFlyer admin to access AppsFlyer data.
-  We recommend you use the JSON of a rule already created in the [AppsFlyer platform](https://support.appsflyer.com/hc/en-us/articles/115004703926) as a template.
[block:api-header]
{
  "title": "Validation rule JSON"
}
[/block]
We recommend you copy/paste the JSON of a pre-existing rule into the API to create a new rule. 

**To get the JSON:**

  1. In AppsFlyer, go to **Configuration** > **Validation Rules**.
  2. In the browser window, add #dev to the URL or go to https://hq1.appsflyer.com/vr2/validation-rules#dev.
      A code icon displays near the top right of every Validation rule.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/15db89b-dev.jpg",
        "dev.jpg",
        236,
        53,
        "#f1edef"
      ]
    }
  ]
}
[/block]
3. Select a Validation rule. 
4. Click the code icon.
    A JSON with the API Validation rule code request displays. 
Click copy.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"action\": \"block-candidate\",\n  \"app-ids\": [\n    \"some.app\"\n  ],\n  \"description\": \"some NEW description\",\n  \"event-type\": \"install\",\n  \"name\": \"some name\",\n  \"population\": {\n    \"cond-oper\": \"and”,\n    \"conds\": [\n      {\n        \"attr\": \"engagement.media_source\",\n        \"oper\": \"s.in\",\n        \"values\": [\n          \"some_media_source_1\",\n          \"some_media_source_2\"\n        ]\n      }\n    ]\n  },\n  \"rule-conditions\": {\n    \"cond-group-oper\": \"or\",\n    \"cond-groups\": [\n      {\n        \"cond-oper\": \"and\",\n        \"conds\": [\n          {\n            \"attr\": \"geo\",\n            \"oper\": \"s.in\",\n            \"values\": [\n              \"us/*/*\",\n              \"my/*/*\",\n              \"ca/*/*\"\n            ]\n          }\n        ]\n      }\n    ]\n  },\n  \"rule-type\": \"allow-only\",\n  \"status\": \"enabled\"\n}\n",
      "language": "json",
      "name": "Request example"
    }
  ]
}
[/block]