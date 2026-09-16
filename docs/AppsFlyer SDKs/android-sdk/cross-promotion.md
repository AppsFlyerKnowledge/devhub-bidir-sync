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

This guide covers cross-promotion attribution for the Android SDK: attributing clicks, attributing impressions, and building the click URL yourself for non-native platforms.

## Attribute cross-promotion clicks

Call [`CrossPromotionHelper.logAndOpenStore`](doc:android-sdk-reference-sharecrosspromotionhelper#logandopenstore) when a user taps a cross-promotion ad. The SDK builds an attribution link, appends the device advertising ID, and opens the promoted app's Play Store listing.

```java
String campaign = "spring_sale_2026";
Map<String, String> parameters = new HashMap<>();
parameters.put("af_sub1", "val");
parameters.put("custom_param", "val2");
CrossPromotionHelper.logAndOpenStore(this, "com.appsflyer.promotedapp", campaign, parameters);
```

`parameters` accepts any of the standard attribution link parameters, including the click lookback window, ad IDs, ad set names, and the incentivized-campaign flag. See [Attribution link structure and parameters](https://support.appsflyer.com/hc/en-us/articles/207447163-Attribution-link-structure-and-parameters#attribution-link-parameters).

## Attribute cross-promotion impressions

Call [`CrossPromotionHelper.logCrossPromoteImpression`](doc:android-sdk-reference-sharecrosspromotionhelper#logcrosspromoteimpression) when a cross-promotion ad becomes visible. Use the promoted app's App ID exactly as it appears in the AppsFlyer dashboard.

```java Java
String appID = "com.appsflyer.promotedapp";
String campaign = "spring_sale_2026";
Map<String, String> parameters = new HashMap<>();
parameters.put("af_sub1", "val");
parameters.put("custom_param", "val2");
CrossPromotionHelper.logCrossPromoteImpression(this, appID, campaign, parameters);
```
```kotlin Kotlin
val appID = "com.appsflyer.promotedapp"
val campaign = "spring_sale_2026"
val parameters = HashMap<String, String>()
parameters["af_sub1"] = "val"
parameters["custom_param"] = "val2"
CrossPromotionHelper.logCrossPromoteImpression(this, appID, campaign, parameters)
```

## Read the original attribution data

On the promoted app's first launch, read the full set of cross-promotion parameters through the SDK's conversion data API. See [Conversion data](doc:conversion-data-android).

## Attribute cross-promotion with non-native platforms

`CrossPromotionHelper` covers native Android. If your promoted app is built on a non-native platform without a dedicated cross-promotion API (for example a WebView-based hybrid app, or a platform not covered by an AppsFlyer SDK plugin), build the attribution link yourself instead.

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
| `promoted_app_id` | `string` | Required. The promoted app's package name, exactly as it appears in the AppsFlyer dashboard. | `com.appsflyer.promotedapp` |
| `pid` | `string` | Required. The media source. Always `af_cross_promotion` for cross-promotion attribution. | `af_cross_promotion` |
| `af_siteid` | `string` | Required. The name of the source app (the app the ad was shown in). | `myapp_android` |

Invoke the click URL when the user taps the ad, and the impression URL when the ad becomes visible. Both accept any of the standard [attribution link parameters](https://support.appsflyer.com/hc/en-us/articles/207447163-Attribution-link-structure-and-parameters#attribution-link-parameters).

## See also

- [CrossPromotionHelper](doc:android-sdk-reference-sharecrosspromotionhelper)
- [Attribution link structure and parameters](https://support.appsflyer.com/hc/en-us/articles/207447163-Attribution-link-structure-and-parameters)
- [Conversion data](doc:conversion-data-android)
