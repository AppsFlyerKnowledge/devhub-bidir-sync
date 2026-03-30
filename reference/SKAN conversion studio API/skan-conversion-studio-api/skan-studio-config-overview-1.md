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
If you have many apps for which you want similar SKAN conversion value (CV) configurations, it may be time-consuming and challenging to design the schema for each app separately in the Conversion Studio UI. To save time and effort, the SKAN schema can be configured in the UI once, and then use the SKAN Conversion Studio API to use that same schema in multiple apps. **Note**: The API only supports schemas configured in the UI using SKAN 4 or custom measurement modes.

**To use the SKAN Conversion Studio API:**

1. Get the [API token](https://support.appsflyer.com/hc/en-us/articles/360004562377) from your marketer. The API token is used in the authentication header.
2. Get from your marketer: The app IDs of:  
   The app for which the SKAN schema is configured in the UI (meaning the app from which you want to copy the SKAN schema).  
   All the apps that you copy that schema to.

**Note**:

- The API can be used to copy the schema to one app per API call. However, you can then create a script to copy the required schema to multiple apps.
- You can use the GET API call to get the SKAN CV schema for a specific app.