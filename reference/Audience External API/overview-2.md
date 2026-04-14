---
title: Overview
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
**At a glance**: The Imported Audience Management API allows advertisers to create and configure imported audiences; connect imported audiences to partners; and initiate upload of imported audiences to partners.

> 📘 Updating existing audiences
> 
> Once an audience has been created (either via this API or the AppsFlyer user interface), you can add, remove, and update users/devices for that audience via the [Import Audience API V2.0](https://support.appsflyer.com/hc/en-us/articles/360013291857-Audiences-import-an-audience#use-the-import-audience-api) or via [CSV upload](https://support.appsflyer.com/hc/en-us/articles/360013291857-Audiences-import-an-audience#import-a-csv-file).

## Endpoints and methods

The Imported Audience Management API includes the following methods:

- [Create a new imported audience](https://dev.appsflyer.com/hc/reference/post_audience) **(POST)**
- [List an audience's partner connections](https://dev.appsflyer.com/hc/reference/get_audience-audience-id-connections) **(GET)**
- [Connect audience to existing partners](https://dev.appsflyer.com/hc/reference/put_audience-audience-id-connections) **(PUT)**
- [List an audience's split sync connections](https://dev.appsflyer.com/hc/reference/get_audience-audience-id-split-syncs) **(GET)**
- [Update an audience's split sync connections](https://dev.appsflyer.com/hc/reference/put_audience-audience-id-split-syncs) **(PUT)**
- [Initiate upload of an audience to partners](https://dev.appsflyer.com/hc/reference/post_audience-audience-id-upload-now) **(POST)**
- [List account's partner connections](https://dev.appsflyer.com/hc/reference/get_connections) **(GET)**
- [List account's split sync connections](https://dev.appsflyer.com/hc/reference/get_split-syncs) **(GET)**

## Error response codes

The API can return the following response error codes:

- 1000 - must be Android or iOS
- 1001 - Audience name already exists under your account
- 1002 - Audience does not exist under your account
- 1003 - The following integrations are invalid: <integration ID>
- 1004 - Creation is no longer allowed for this account
- 1005 - This audience is not a split audience
- 1006 - There are no partners connected; upload is not possible
- 1009 - Total connections (120) exceeds the maximum limit of 115
- 1020 - Internal error occurred