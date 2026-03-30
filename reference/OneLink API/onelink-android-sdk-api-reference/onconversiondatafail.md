---
title: onConversionDataFail
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

Handle errors when failing to get conversion data from installs.

**Method signature**
[block:code]
{
  "codes": [
    {
      "code": "public void onConversionDataFail(String errorMessage)",
      "language": "java"
    },
    {
      "code": "-(void)onConversionDataFail:(NSError *) error {\n\t  NSLog(@\"%@\",error);\n\t  // handle conversion data failure\n}",
      "language": "objectivec"
    },
    {
      "code": "func onConversionDataFail(_ error: Error?) {\n\t\t//    print(\"\\(error)\")\n\t\t// handle conversion data failure\n}",
      "language": "swift"
    }
  ]
}
[/block]