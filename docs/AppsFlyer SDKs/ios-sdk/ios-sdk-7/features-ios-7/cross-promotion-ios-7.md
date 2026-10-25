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
This guide covers iOS SDK 7.0 and above. For SDK 6.x, see [Cross-promotion for SDK 6](doc:cross-promotion-ios).

Cross-promotion attribution uses AppsFlyer SDK methods instead of a standard attribution link: the SDK builds the link and calls it directly when a user taps or views a cross-promotion ad.

[`AppsFlyerCrossPromotionHelper`](doc:ios-sdk-reference-appsflyercrosspromotionhelper) provides those methods: `logAndOpenStore` sends the click to AppsFlyer and opens the App Store listing, and `logCrossPromoteImpression` sends the impression to AppsFlyer.

For background on cross-promotion campaigns, see [Cross-promotion attribution for marketers](https://support.appsflyer.com/hc/en-us/articles/115004481946-Cross-promotion-attribution-for-marketers).

## Attribute cross-promotion clicks

Call [`AppsFlyerCrossPromotionHelper logAndOpenStore`](doc:ios-sdk-reference-appsflyercrosspromotionhelper#logandopenstore) when a user taps a cross-promotion ad. Use `StoreKit`'s `SKStoreProductViewController` to open the App Store while staying in the context of your app; the method falls back to an attribution link when `SKStoreProductViewController` isn't available.

```swift Swift
let crossPromotedAppID = "123456789"
let crossPromotedCampaign = "spring_sale_2026"

func crossPromotion() {
    let userParams = ["af_sub1": "val", "custom_param": "val2"]
    AppsFlyerCrossPromotionHelper.logAndOpenStore(crossPromotedAppID,
        campaign: crossPromotedCampaign,
        userParams: userParams) { urlSession, clickURL in
        // Handle the generated click URL if SKStoreProductViewController isn't available.
    }
}
```
```objc Objective-C
static NSString *const kCrossPromotedAppId = @"123456789";
static NSString *const kCrossPromotedCampaign = @"spring_sale_2026";

- (void)crossPromotion {
  NSDictionary *userParams = @{@"af_sub1": @"val", @"custom_param": @"val2"};
  [AppsFlyerCrossPromotionHelper logAndOpenStore:kCrossPromotedAppId
                                         campaign:kCrossPromotedCampaign
                                       userParams:userParams
                                        openStore:^(NSURLSession *urlSession, NSURL *clickURL) {
    // Handle the generated click URL if SKStoreProductViewController isn't available.
  }];
}
```

## Attribute cross-promotion impressions

Call [`AppsFlyerCrossPromotionHelper logCrossPromoteImpression`](doc:ios-sdk-reference-appsflyercrosspromotionhelper#logcrosspromoteimpression) when a cross-promotion ad becomes visible.

```swift Swift
let crossPromotedAppID = "123456789"
let crossPromotedCampaign = "spring_sale_2026"

override func viewDidLoad() {
    super.viewDidLoad()
    let userParams = ["af_sub1": "val", "custom_param": "val2"]
    AppsFlyerCrossPromotionHelper.logCrossPromoteImpression(crossPromotedAppID,
        campaign: crossPromotedCampaign,
        userParams: userParams)
}
```
```objc Objective-C
static NSString *const kCrossPromotedAppId = @"123456789";
static NSString *const kCrossPromotedCampaign = @"spring_sale_2026";

- (void)viewDidLoad {
  [super viewDidLoad];
  NSDictionary *userParams = @{@"af_sub1": @"val", @"custom_param": @"val2"};
  [AppsFlyerCrossPromotionHelper logCrossPromoteImpression:kCrossPromotedAppId
                                                   campaign:kCrossPromotedCampaign
                                                 userParams:userParams];
}
```

## See also

- [AppsFlyerCrossPromotionHelper](doc:ios-sdk-reference-appsflyercrosspromotionhelper)
- [Migrate iOS SDK to V7](doc:migrate-ios-sdk-to-v7)
- [Conversion data](doc:conversion-data-ios)
