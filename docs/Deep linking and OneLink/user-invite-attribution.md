---
title: User-invite attribution
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
[block:callout]
{
  "type": "danger",
  "title": "This page has a newer [version](https://dev.appsflyer.com/hc/docs/dl_user_invite)"
}
[/block]
## Overview
Attributing user-invites lets marketers gain insight into how much traffic your app gets from existing users inviting new users to join.

Generally, it consists of the following:
1. Defining the user-invitation flow and implementing the generation of OneLink links accordingly.
2. Logging the invitation as an in-app event. This results in:
   - An `af_invite` event that's visible in AppsFlyer dashboards and reports.
   - The `pid` (media source) parameter being set with the default value `af_app_invites`. To change the value, you need to add a custom parameter called `pid` with the value you want. **Note**: For Android, this only works for AppsFlyer SDK V6.4.2+.
<!-- 3. Rewarding users for invites that bring active users to your app. -->

<!-- This is achieved by utilizing the AppsFlyer SDKs to generate OneLink URLs and optionally reward referrers via Unified Deeplinking (UDL), raw-data -->

## Implementation guides
[block:html]
{
  "html": "<div class=\"button-container\">\n  <a class=\"button android\" href=\"https://dev.appsflyer.com/hc/docs/user-invite-attribution-android\">Android</a>\n  <a class=\"button ios\" href=\"https://dev.appsflyer.com/hc/docs/user-invite-attribution-ios\">iOS</a>\n</div>\n\n<style>\n  .button-container {\n  \tdisplay: flex;\n  }\n  .button {\n    display: flex;\n    justify-content: center;\n    align-items: center;\n    width: 150px;\n\t  border-radius: 6px;\n    padding: 8px;\n    margin-right: 4px;\n\t}\n  \n  .button:before {\n  \tmargin-right: 4px;\n  }\n  .button.android {\n    border: solid 2px #3DDC84;\n  }\n  .ios {\n  \tborder-radius: 6px;\n    padding: 8px;\n    border: solid 2px #7D7D7D;\n  }\n  .ios:before {\n        content: url(\"https://files.readme.io/19fdc72-apple-icon.svg\");\n  }\n\n  .android:before {\n        content: url(\"https://files.readme.io/d7dc5a3-android-icon.svg\");\n  }\n</style>"
}
[/block]