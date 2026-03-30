---
title: Overview
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
**At a glance**: Validation rules API lets you [get](https://dev.appsflyer.com/hc/reference/get-rules), [create](https://dev.appsflyer.com/hc/reference/create-rule), [update](https://dev.appsflyer.com/hc/reference/update-rule), and [update rule status](https://dev.appsflyer.com/hc/reference/update-rule-status).
[block:api-header]
{
  "title": "Using Validation rules API"
}
[/block]
Validation rules add a custom layer of protection against wrongly-targeted campaigns and fraud. The rules enable app owners to control which installs and in-app events are blocked, or which installs are attributed to the most recent valid source.

Validation rules API is used to:

- [Get rules:](https://dev.appsflyer.com/hc/reference/get-rules) Provides a list of all your validation rules with details regarding all the rule conditions.
- [Create rule](https://dev.appsflyer.com/hc/reference/create-rule): Create a new rule with specific conditions. When defining rule conditions, you will need to refer to the rule [schema](https://dev.appsflyer.com/hc/reference/schema), [attributes](https://dev.appsflyer.com/hc/reference/attributes), and [operators](https://dev.appsflyer.com/hc/reference/operators) sections.
- [Update rule](https://dev.appsflyer.com/hc/reference/update-rule): Change/edit the conditions of an existing rule. When updating rule conditions, you will need to refer to the rule schema, attributes, and operators sections.
- [Update rule status](https://dev.appsflyer.com/hc/reference/update-rule-status): Enable, disable, or delete an existing rule.

### Prerequisites

- The [API V2.0 token](https://support.appsflyer.com/hc/en-us/articles/360004562377) from your AppsFlyer admin. You need the API token as the authorization for each API command. The API token is passed in the authorization field in HTTP header.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c1c6eba-140d802-Screenshot_2021-05-06_at_14.27.19.png",
        "140d802-Screenshot_2021-05-06_at_14.27.19.png",
        587,
        207,
        "#cacccd"
      ]
    }
  ]
}
[/block]
- To easily create additional rules or update existing rules using the API, we recommend you use the JSON of a rule already created in the [AppsFlyer platform](https://support.appsflyer.com/hc/en-us/articles/115004703926) as a template.

**To get the JSON**:

 1. In AppsFlyer, go to **Configuration** > **Validation Rules**.
 2. In the browser window, add #dev to the URL or go to https://hq1.appsflyer.com/vr2/validation-rules#dev.
     A code icon displays near the top right of every Validation rule.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2e519c3-dev.jpg",
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
 5. Click copy.