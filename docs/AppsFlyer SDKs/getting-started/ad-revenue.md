---
title: Ad revenue
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
Ad revenue from partners can be reported to AppsFlyer with impression-level granularity via SDK. Impression-level data via SDK:
- Has better data freshness and earlier availability in AppsFlyer.
- Supports SKAN. 

The ad revenue SDK connector sends impression revenue data to the AppsFlyer SDK. An ad revenue event, af_ad_revenue, is generated and sent to the platform. These impression events are collected and processed in AppsFlyer, and the revenue is attributed to the original UA source.

AppsFlyer ad revenue SDK connector instructions for various platforms:
[block:html]
{
  "html": "<div class=\"button-container\">\n  <a class=\"button android\" href=\"https://dev.appsflyer.com/hc/docs/ad-revenue-1\">Android SDK</a>\n  <a class=\"button ios\" href=\"https://dev.appsflyer.com/hc/docs/ad-revenue-2\">iOS SDK</a>\n</div>\n\n<style>\n  .button-container {\n  \tdisplay: flex;\n  }\n  .button {\n    display: flex;\n    justify-content: center;\n    align-items: center;\n    width: 150px;\n\t  border-radius: 6px;\n    padding: 8px;\n    margin-right: 4px;\n\t}\n  \n  .button:before {\n  \tmargin-right: 4px;\n  }\n  .button.android {\n    border: solid 2px #3DDC84;\n  }\n  .ios {\n  \tborder-radius: 6px;\n    padding: 8px;\n    border: solid 2px #7D7D7D;\n  }\n  .ios:before {\n        content: url(\"https://files.readme.io/19fdc72-apple-icon.svg\");\n  }\n\n  .android:before {\n        content: url(\"https://files.readme.io/d7dc5a3-android-icon.svg\");\n  }\n</style>"
}
[/block]