---
title: Copy partner integration settings
excerpt: >
  > ⚠️ Important

  >

  > All API V2 tokens generated before March 10, 2026, 19:00 UTC have been
  revoked. If your token was generated before this date, please regenerate it.
  [Learn
  how](https://support.appsflyer.com/hc/en-us/articles/360004562377-Managing-AppsFlyer-tokens).


  Copy the integration settings from one app to another. Copying the parameters
  can be done only for apps on the same platform. To retrieve the partner's
  unique parameters needed for the integration, use the [GET
  request](https://dev.appsflyer.com/hc/reference/get_v1-partner-params-pid-platform).

  > 📘 Note

  > Only parameters of event names with the `af_` prefix can be copied.  
api:
  file: partner-integration-settings-api.json
  operationId: post_v1-copy
hidden: false
---