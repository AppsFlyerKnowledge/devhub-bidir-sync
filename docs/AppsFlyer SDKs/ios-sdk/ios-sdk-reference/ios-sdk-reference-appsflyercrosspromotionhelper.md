---
title: AppsFlyerCrossPromotionHelper
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
## Overview
AppsFlyer allows you to log and attribute installs originating from cross-promotion campaigns of your own apps. This enables marketers to optimize cross-promotion campaigns for better results.

Go back to the [SDK reference index](doc:ios-sdk-reference).

**Interface declaration**
```swift Swift
class AppsFlyerCrossPromotionHelper
```
```objc Objective-C
@interface AppsFlyerCrossPromotionHelper
```

To access `AppsFlyerCrossPromotionHelper`, import [`AppsFlyerLib`](doc:ios-sdk-reference-appsflyerlib).

## Methods

### logCrossPromoteImpression
**Method signature**
```swift Swift
class func logCrossPromoteImpression(_ appID: String,
                                     campaign: String,
                                     parameters: [AnyHashable: Any]?)
```
```objc Objective-C
(void)logCrossPromoteImpression:(id)appID
                         campaign:(id)campaign
                       parameters:(id)parameters;
```

**Description**
Logs an impression as part of a cross-promotion campaign. Make sure to use the promoted App ID as it appears within the AppsFlyer dashboard.

**Input arguments**

| Type | Name | Description |
|:------|:-----|:-----|
| `NSString` | `appID` | Promoted App ID. |
| `NSString` | `campaign` | Name of cross-promotion campaign. |
| `NSDictionary` | `parameters` | Optional. Custom parameters that are added to the link. |

**Returns**
`void`.

### logAndOpenStore
**Method signature**
```swift Swift
class func logAndOpenStore(_ appID: String,
                           campaign: String,
                           parameters: [AnyHashable: Any]?,
                           openStore: @escaping (URLSession, URL) -> Void)
```
```objc Objective-C
(void)logAndOpenStore:(id)appID
               campaign:(id)campaign
             parameters:(id)parameters
              openStore:(void (^)(NSURLSession *urlSession, NSURL *clickURL))openStoreBlock;
```

**Description**
You can utilize the StoreKit component to open the App Store while remaining in the context of your app. Learn more in [Cross-promotion](doc:cross-promotion-ios).

**Input arguments**

| Type | Name | Description |
|:------|:-----|:-----|
| `NSString` | `appID` | Promoted App ID. |
| `NSString` | `campaign` | Name of cross-promotion campaign. |
| `NSDictionary` | `parameters` | Optional. Custom parameters that are added to the link. |
| `void (^)(NSURLSession *, NSURL *)` | `openStoreBlock` | Contains the promoted click URL. |

**Returns**
`void`.
