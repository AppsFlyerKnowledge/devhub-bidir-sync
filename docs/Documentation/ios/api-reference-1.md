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
      "code": "- (void)onAppOpenAttribution:(NSDictionary *)attributionData;",
      "language": "objectivec",
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
      "code": "- (void)performOnAppAttributionWithURL:(NSURL * _Nullable)URL;",
      "language": "objectivec",
      "name": "performOnAppAttribution"
    }
  ]
}
[/block]
### onAppOpenAttributionFailure

**Description**

Handle errors in getting deep link data.

**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "- (void)onAppOpenAttributionFailure:(NSError *)error;",
      "language": "objectivec",
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
      "code": "- (void)onConversionDataSuccess:(NSDictionary *)installData;",
      "language": "objectivec",
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
      "code": "- (void)onConversionDataFail:(NSError *)error;",
      "language": "objectivec",
      "name": "onConversionDataFail"
    }
  ]
}
[/block]