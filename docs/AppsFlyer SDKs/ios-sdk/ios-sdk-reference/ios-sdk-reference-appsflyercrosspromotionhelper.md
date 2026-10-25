---
title: AppsFlyerCrossPromotionHelper
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

Signatures and argument names are shown for SDK 6.x, followed by the SDK 7.0 changes. For the full list of SDK 7.0 changes, see [Migrate iOS SDK to V7](doc:migrate-ios-sdk-to-v7).

### logCrossPromoteImpression
**Method signature, SDK 6.x**
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

**Method signature, SDK 7.0 and above**
```swift Swift
class func logCrossPromoteImpression(_ appId: String,
                                     campaign: String,
                                     userParams: [AnyHashable: Any]?)
```
```objc Objective-C
(void)logCrossPromoteImpression:(id)appId
                         campaign:(id)campaign
                       userParams:(id)userParams;
```

**Description**
Logs an impression as part of a cross-promotion campaign. Make sure to use the promoted App ID as it appears within the AppsFlyer dashboard.

**Input arguments**

| Type | Name | Description |
|:------|:-----|:-----|
| `NSString` | `appID` | Promoted App ID. |
| `NSString` | `campaign` | Name of cross-promotion campaign. |
| `NSDictionary` | `parameters` | Optional. Custom parameters that are added to the link. |

**Changed in v7.0:** `appID` is renamed `appId`, and `parameters` is renamed `userParams`. For code changes, see [Update CrossPromotionHelper parameter labels](doc:migrate-ios-sdk-to-v7#9-update-crosspromotionhelper-parameter-labels).

**Returns**
`void`.

### logAndOpenStore
**Method signature, SDK 6.x**
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

**Method signature, SDK 7.0 and above**
```swift Swift
class func logAndOpenStore(_ promotedAppId: String,
                           campaign: String,
                           userParams: [AnyHashable: Any]?,
                           openStore: @escaping (URLSession, URL) -> Void)
```
```objc Objective-C
(void)logAndOpenStore:(id)promotedAppId
               campaign:(id)campaign
             userParams:(id)userParams
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

**Changed in v7.0:** `appID` is renamed `promotedAppId`, and `parameters` is renamed `userParams`. For code changes, see [Update CrossPromotionHelper parameter labels](doc:migrate-ios-sdk-to-v7#9-update-crosspromotionhelper-parameter-labels).

**Returns**
`void`.
