---
title: onConversionDataSuccess
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

Get conversion data after an install. Useful for deferred deep linking.

**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void onConversionDataSuccess(Map<String, String> conversionData)",
      "language": "java"
    },
    {
      "code": "-(void)onConversionDataSuccess:(NSDictionary*) installData {\n\t  //Handle Conversion Data (Deferred Deep Link)\n}",
      "language": "objectivec"
    },
    {
      "code": " func onConversionDataSuccess(_ installData: [AnyHashable: Any]) {\n\t  //Handle Conversion Data (Deferred Deep Link)\n}",
      "language": "swift"
    }
  ]
}
[/block]