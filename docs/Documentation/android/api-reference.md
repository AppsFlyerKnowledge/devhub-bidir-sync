---
title: API reference
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
### onAppOpenAttribution

**Description**

Get data for users when the app opens via a deep link (not via deferred deep linking).

**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void onAppOpenAttribution(Map<String, String> conversionData);",
      "language": "java",
      "name": "OnAppOpenAttribution"
    }
  ]
}
[/block]
### performOnAppAttribution

**Description**

This function allows developers to manually re-trigger onAppOpenAttribution and enables developers to access deep link data at any time without connection to the app launch process. This might be needed because the regular onAppOpenAttribution callback is only called **if the app was opened with the deep link**.
 
**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void performOnAppAttribution(Context context, Uri uri);",
      "language": "java",
      "name": "performOnAppAttribution"
    }
  ]
}
[/block]
### onAttributionFailure

**Description**

Handle errors in getting deep link data.

**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void onAttributionFailure(String errorMessage)",
      "language": "java",
      "name": "onAppOpenAttributionFailure"
    }
  ]
}
[/block]
### onConversionDataSuccess

**Description**

Get conversion data after an install. Useful for deferred deep linking.

**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void onConversionDataSuccess(Map<String, String> conversionData);",
      "language": "java",
      "name": "onConversionDataSuccess"
    }
  ]
}
[/block]
### onConversionDataFail

**Description**

Handle errors when failing to get conversion data from installs.

**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void onConversionDataFail(String errorMessage);",
      "language": "java",
      "name": "onConversionDataFail"
    }
  ]
}
[/block]