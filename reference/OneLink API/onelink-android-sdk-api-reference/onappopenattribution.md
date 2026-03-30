---
title: onAppOpenAttribution
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

Get data for users when the app opens via a deep link (not via deferred deep linking).

**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void onAppOpenAttribution(Map<String, String> conversionData);",
      "language": "java",
      "name": "Android - Java"
    },
    {
      "code": "(void) onAppOpenAttribution:(NSDictionary*) attributionData {\n\t\t//Handle Deep Link\n\t}",
      "language": "objectivec",
      "name": "iOS - Objective-C"
    },
    {
      "code": "func onAppOpenAttribution(_ attributionData: [AnyHashable: Any]) {\n\t\t//Handle Deep Link Data\n}",
      "language": "swift",
      "name": "iOS - Swift"
    }
  ]
}
[/block]