---
title: performOnAppAttribution
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
**Description**

Allows developers to manually re-trigger onAppOpenAttribution and enables developers to access deep link data at any time without connection to the app launch process. This might be needed because the regular onAppOpenAttribution callback is only called **if the app was opened with the deep link**.
 
**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void performOnAppAttribution(Context context, Uri uri);",
      "language": "java"
    },
    {
      "code": "[[AppsFlyerLib shared] performOnAppAttributionWithURL:(NSURL * _Nullable)url];             ",
      "language": "objectivec"
    },
    {
      "code": "AppsFlyerLib.shared().performOnAppAttribution(with: url)               ",
      "language": "swift"
    }
  ]
}
[/block]