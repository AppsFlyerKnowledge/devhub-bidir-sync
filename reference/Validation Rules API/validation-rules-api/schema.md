---
title: Schema
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
The following table describes the schema of a validation rule JSON.
[block:parameters]
{
  "data": {
    "h-3": "Name in AppsFlyer UI",
    "h-0": "Attribute",
    "h-1": "Value",
    "h-2": "Type",
    "0-0": "name",
    "1-0": "description",
    "2-0": "event-type",
    "3-0": "app-ids",
    "4-0": "status",
    "5-0": "action",
    "6-0": "rule-type",
    "7-0": "cond-oper",
    "8-0": "cond-group-oper",
    "9-0": "population",
    "10-0": "rule-conditions",
    "0-1": "*",
    "1-1": "*",
    "2-1": "<ul class=\"af_list\">\n<li>install</li>\n<li>in-app event</li>\n</ul>",
    "3-1": "All app IDs for which the rule is active",
    "4-1": "<ul class=\"af_list\">\n<li>enabled</li>\n<li>disabled</li>\n<li>deleted</li>\n</ul>",
    "5-1": "<ul class=\"af_list\">\n<li>block-event</li>\n<li>block-candidate</li>\n</ul>",
    "6-1": "<ul class=\"af_list\">\n<li>blocking</li>\n<li>allow-only</li>\n</ul>",
    "7-1": "<ul class=\"af_list\">\n<li>and</li>\n<li>or</li>\n</ul>",
    "8-1": "<ul class=\"af_list\">\n<li>and</li>\n<li>or</li>\n</ul>",
    "9-1": "{\n    \"cond-oper\": “and”,\n    \"conds\": Array<{\n        \"attr\": population attribute,\n        \"oper\": operator appropriate for the att type,\n        \"values\": Array<att type>\n      }>\n  }",
    "10-1": "{\n    \"cond-group-oper\": cond-group-oper,\n    \"cond-groups\": Array<{\n        \"cond-oper\": cond-oper,\n        \"conds\": Array<{\n           \"attr\": rule attribute,\n           \"oper\": operator appropriate for the att type,\n           \"values\": Array<att type>\n        }>\n     }>\n}",
    "0-2": "String",
    "1-2": "String",
    "2-2": "String",
    "4-2": "String",
    "5-2": "String",
    "6-2": "String",
    "7-2": "String",
    "8-2": "String",
    "3-2": "Array<String>",
    "9-2": "Json",
    "10-2": "Json",
    "0-3": "N/A",
    "1-3": "N/A",
    "2-3": "<ul class=\"af_list\">\n<li>Installs</li>\n<li>In-app events</li>\n</ul>",
    "3-3": "Apps",
    "4-3": "Active (on/off)",
    "5-3": "Action \n(Block install/Block attribution)",
    "6-3": "Considered\n(Invalid/Valid)",
    "7-3": "N/A",
    "8-3": "N/A",
    "9-3": "Sources",
    "10-3": "Conditions"
  },
  "cols": 4,
  "rows": 11
}
[/block]