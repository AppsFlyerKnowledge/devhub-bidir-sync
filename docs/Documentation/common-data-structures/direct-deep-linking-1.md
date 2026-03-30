---
title: Input parameters
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
The following table lists the possible parameters OneLink can pass as an input.

The  input map holds two kinds of data:
* [Attribution data](https://support.appsflyer.com/hc/en-us/articles/207447163#attribution-link-parameters)
* Data defined by the marketer in the link (parameters and values)
Parameters can be either:
   * AppsFlyer official parameters.
   * Custom parameters and values chosen by the marketer and developer.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "* The following table is relevant for AppsFlyer **SDK 5.4.1 and above**.\n   * Parameters may not be present or renamed in earlier SDK versions\n* The parameters **not marked as deprecated ** are relevant for all OneLink types:\n   * Short URL\n   * Long URL\n   * All OS's links:\n      * Android App Link\n      * Universal Links\n      * URL schemes (both iOS and Android)"
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Parameter name",
    "h-1": "Type",
    "h-2": "Description",
    "h-3": "Remarks",
    "0-0": "af_dp",
    "0-1": "String",
    "0-2": "URI scheme URL.",
    "0-3": "Fallback to App Link. \nFor example:  afbasicapp://mainactivity",
    "1-0": "link",
    "1-1": "String",
    "1-2": "The full link that was used to perform the deep link.",
    "1-3": "Example: https://onelink-basic-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month",
    "2-0": "media_source",
    "2-1": "String",
    "2-2": "OneLink's media source, e.g. email, SMS, social media.",
    "3-0": "install_time",
    "3-1": "String",
    "3-2": "Time of the first app launch.",
    "3-3": "Example: 2020-05-06 13:51:19",
    "4-0": "scheme",
    "4-1": "String",
    "4-2": "The first word in the URL, that identifies the protocol to be used to access the resource on the Internet. For example: **mygreatapp**://mainactivity or **https**://killerapp.onelink.me/coolactivity/H7JK",
    "4-3": "Never use `http` or `https` for URI schemes",
    "5-0": "host",
    "5-1": "String",
    "5-2": "Identifies the host that holds the resource. For example: mygreatapp://**mainactivity** or \nhttps://**killerapp.onelink.me**/coolactivity/H7JK",
    "6-0": "path",
    "6-1": "String",
    "6-2": "The specific resource in the host that the web client wants to access. For example:  https://killerapp.onelink.me/coolactivity/**H7JK**",
    "6-3": "Not relevant for URI schemes",
    "8-0": "af_web_id",
    "8-1": "String",
    "8-2": "Token for People Based Attribution.",
    "9-0": "af_status",
    "9-1": "String",
    "9-2": "**Deprecated**",
    "9-3": "Passed **only** in URI scenario",
    "10-0": "af_deeplink",
    "10-1": "Boolean",
    "10-2": "**Deprecated**",
    "10-3": "Passed **only** in URI scenario",
    "11-0": "campaign",
    "11-1": "String",
    "11-2": "Name of the marketing campaign.",
    "7-0": "shortlink",
    "7-1": "String",
    "7-2": "A shortened URL, with significantly fewer characters than the original link. For example: https://killerapp.onelink.me/coolactivity/H7JK/**checkitout**",
    "12-0": "is_retargeting",
    "12-1": "Boolean",
    "12-2": "Marks the link as part of a retargeting campaign.",
    "13-0": "af_ios_url",
    "13-1": "String",
    "13-3": "Passed to Android devices as well, even when not relevant",
    "13-2": "Fallback URL when deep linking fails on an iOS device.",
    "14-0": "af_android_url",
    "14-1": "String",
    "14-2": "Fallback URL when deep-linking fails on an Android device.",
    "15-0": "af_sub[1-5]",
    "15-1": "String",
    "15-2": "Optional custom parameter defined by the advertiser.",
    "15-3": "Values set by the marketer in the AppsFlyer dashboard.\nRecommended for passing parameters relevant for in-app routing.",
    "16-0": "af_adset",
    "16-1": "String",
    "16-2": "Adset is an intermediate level in the hierarchy between campaign and ad.",
    "19-0": "af_cost_currency",
    "20-0": "af_cost_value",
    "21-0": "af_click_lookback",
    "17-0": "af_channel",
    "18-0": "ad_adname",
    "17-1": "String",
    "18-1": "String",
    "17-2": "The media source channel through which the ads are distributed. For example:  UAC_Search, UAC_Display, Instagram, Facebook Audience Network etc.",
    "18-2": "Ad name provided by the marketer/publisher.",
    "16-3": "Value set by the marketer in AppsFlyer's dashboard",
    "22-0": "af_force_deeplink",
    "22-1": "Boolean",
    "22-2": "Force deep linking into the activity specified in af_dp value.",
    "22-3": "Relevant for iOS only.\nValue is passed to Android, even when not relevant.",
    "19-1": "String",
    "20-1": "String",
    "21-1": "String",
    "19-2": "3 letter currency code compliant with [ISO-4217](https://support.appsflyer.com/hc/en-us/articles/207040526-Ad-cost-measurement-guide#cost-aggregation-methods). For example, USD, ZAR, EUR\n[Default]: USD",
    "20-2": "Cost value in using cost currency.",
    "21-2": "Configurable number of days for the lookback click attribution period.",
    "17-3": "Value set by the marketer in AppsFlyer's dashboard",
    "18-3": "Value set by the marketer in AppsFlyer's dashboard",
    "19-3": "Value set by the marketer in AppsFlyer's dashboard",
    "20-3": "Value set by the marketer in AppsFlyer's dashboard",
    "21-3": "Value set by the marketer in AppsFlyer's dashboard",
    "12-3": "The value is set by the marketer.",
    "11-3": "The value set by the marketer in the AppsFlyer dashboard."
  },
  "cols": 4,
  "rows": 23
}
[/block]