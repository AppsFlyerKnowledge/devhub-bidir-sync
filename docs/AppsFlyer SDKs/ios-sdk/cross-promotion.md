---
title: Cross-promotion
excerpt: Attribute clicks and impressions from cross-promotion campaigns between your own apps.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Cross-promotion ads promote one of your own apps from inside another. Unlike a regular mobile ad, a cross-promotion ad doesn't open a URL when a user clicks or views it. Instead, it calls an AppsFlyer SDK method directly.

This guide covers cross-promotion attribution for the iOS SDK: attributing clicks, attributing impressions, and building the click URL yourself for non-native platforms.

## Attribute cross-promotion clicks

Call [`AppsFlyerCrossPromotionHelper logAndOpenStore`](doc:ios-sdk-reference-appsflyercrosspromotionhelper#logandopenstore) when a user taps a cross-promotion ad. Use `StoreKit`'s `SKStoreProductViewController` to open the App Store while staying in the context of your app; the method falls back to an attribution link when `SKStoreProductViewController` isn't available.

```objc
static NSString *const kCrossPromotedAppId = @"123456789";
static NSString *const kCrossPromotedCampaign = @"spring_sale_2026";

- (void)crossPromotion {
  NSDictionary *parameters = @{@"af_sub1": @"val", @"custom_param": @"val2"};
  [AppsFlyerCrossPromotionHelper logAndOpenStore:kCrossPromotedAppId
                                         campaign:kCrossPromotedCampaign
                                       parameters:parameters
                                        openStore:^(NSURLSession *urlSession, NSURL *clickURL) {
    // Handle the generated click URL if SKStoreProductViewController isn't available.
  }];
}
```

`parameters` accepts any of the standard attribution link parameters, including the click lookback window, ad IDs, ad set names, and the incentivized-campaign flag. See [Attribution link structure and parameters](https://support.appsflyer.com/hc/en-us/articles/207447163-Attribution-link-structure-and-parameters#attribution-link-parameters).

## Attribute cross-promotion impressions

Call [`AppsFlyerCrossPromotionHelper logCrossPromoteImpression`](doc:ios-sdk-reference-appsflyercrosspromotionhelper#logcrosspromoteimpression) when a cross-promotion ad becomes visible. Use the promoted app's App ID exactly as it appears in the AppsFlyer dashboard.

```objc Objective-C
static NSString *const kCrossPromotedAppId = @"123456789";
static NSString *const kCrossPromotedCampaign = @"spring_sale_2026";

- (void)viewDidLoad {
  [super viewDidLoad];
  NSDictionary *parameters = @{@"af_sub1": @"val", @"custom_param": @"val2"};
  [AppsFlyerCrossPromotionHelper logCrossPromoteImpression:kCrossPromotedAppId
                                                   campaign:kCrossPromotedCampaign
                                                 parameters:parameters];
}
```
```swift Swift
let appID = "123456789"
let campaign = "spring_sale_2026"
let parameters = ["af_sub1": "val", "custom_param": "val2"]
AppsFlyerCrossPromotionHelper.logCrossPromoteImpression(appID, campaign: campaign, parameters: parameters)
```

## Read the original attribution data

On the promoted app's first launch, read the full set of cross-promotion parameters through the SDK's conversion data API. See [Conversion data](doc:conversion-data-ios).

## Attribute cross-promotion with non-native platforms

`AppsFlyerCrossPromotionHelper` covers native iOS. If your promoted app is built on a non-native platform without a dedicated cross-promotion API (for example a WebView-based hybrid app, or a platform not covered by an AppsFlyer SDK plugin), build the attribution link yourself instead.

The link must carry the media source `af_cross_promotion` and the source app's site ID.

Click URL format:

```http
https://app.appsflyer.com/{promoted_app_id}?pid=af_cross_promotion&af_siteid={source_app_name}
```

Impression URL format:

```http
https://impression.appsflyer.com/{promoted_app_id}?pid=af_cross_promotion&af_siteid={source_app_name}
```

| Parameter | Type | Description | Example |
|:-----|:-----|:-----|:-----|
| `promoted_app_id` | `string` | Required. The promoted app's numeric App Store ID, exactly as it appears in the AppsFlyer dashboard. | `123456789` |
| `pid` | `string` | Required. The media source. Always `af_cross_promotion` for cross-promotion attribution. | `af_cross_promotion` |
| `af_siteid` | `string` | Required. The name of the source app (the app the ad was shown in). | `myapp_ios` |

Invoke the click URL when the user taps the ad, and the impression URL when the ad becomes visible. Both accept any of the standard [attribution link parameters](https://support.appsflyer.com/hc/en-us/articles/207447163-Attribution-link-structure-and-parameters#attribution-link-parameters).

## See also

- [AppsFlyerCrossPromotionHelper](doc:ios-sdk-reference-appsflyercrosspromotionhelper)
- [Attribution link structure and parameters](https://support.appsflyer.com/hc/en-us/articles/207447163-Attribution-link-structure-and-parameters)
- [Conversion data](doc:conversion-data-ios)
