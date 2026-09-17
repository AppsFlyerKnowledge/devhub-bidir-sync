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
Cross-promotion attribution uses AppsFlyer SDK methods instead of a standard attribution link: the SDK builds the link and calls it directly when a user taps or views a cross-promotion ad.

[`AppsFlyerCrossPromotionHelper`](doc:ios-sdk-reference-appsflyercrosspromotionhelper) provides those methods: `logAndOpenStore` sends the click to AppsFlyer and opens the App Store listing, and `logCrossPromoteImpression` sends the impression to AppsFlyer.

For background on cross-promotion campaigns, see [Cross-promotion attribution for marketers](https://support.appsflyer.com/hc/en-us/articles/115004481946-Cross-promotion-attribution-for-marketers).

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

## Attribute cross-promotion impressions

Call [`AppsFlyerCrossPromotionHelper logCrossPromoteImpression`](doc:ios-sdk-reference-appsflyercrosspromotionhelper#logcrosspromoteimpression) when a cross-promotion ad becomes visible.

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

## See also

- [AppsFlyerCrossPromotionHelper](doc:ios-sdk-reference-appsflyercrosspromotionhelper)
- [Conversion data](doc:conversion-data-ios)
