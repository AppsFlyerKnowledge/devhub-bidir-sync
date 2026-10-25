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
This guide covers Android SDK 7.0 and above. For SDK 6.x, see [Cross-promotion for SDK 6](doc:cross-promotion-android).

Cross-promotion attribution uses AppsFlyer SDK methods instead of a standard attribution link: the SDK builds the link and calls it directly when a user taps or views a cross-promotion ad.

[`CrossPromotionHelper`](doc:android-sdk-reference-sharecrosspromotionhelper) provides those methods: `logAndOpenStore` sends the click to AppsFlyer and opens the promoted app's Play Store listing, and `logCrossPromoteImpression` sends the impression to AppsFlyer.

For background on cross-promotion campaigns, see [Cross-promotion attribution for marketers](https://support.appsflyer.com/hc/en-us/articles/115004481946-Cross-promotion-attribution-for-marketers).

## Attribute cross-promotion clicks

Call [`CrossPromotionHelper.logAndOpenStore`](doc:android-sdk-reference-sharecrosspromotionhelper#logandopenstore) when a user taps a cross-promotion ad. The SDK builds an attribution link, appends the device advertising ID, and opens the promoted app's Play Store listing.

```java Java
String campaign = "spring_sale_2026";
Map<String, String> userParams = new HashMap<>();
userParams.put("af_sub1", "val");
userParams.put("custom_param", "val2");
CrossPromotionHelper.logAndOpenStore(this, "com.appsflyer.promotedapp", campaign, userParams);
```
```kotlin Kotlin
val campaign = "spring_sale_2026"
val userParams = HashMap<String, String>()
userParams["af_sub1"] = "val"
userParams["custom_param"] = "val2"
CrossPromotionHelper.logAndOpenStore(this, "com.appsflyer.promotedapp", campaign, userParams)
```

## Attribute cross-promotion impressions

Call [`CrossPromotionHelper.logCrossPromoteImpression`](doc:android-sdk-reference-sharecrosspromotionhelper#logcrosspromoteimpression) when a cross-promotion ad becomes visible.

```java Java
String appID = "com.appsflyer.promotedapp";
String campaign = "spring_sale_2026";
Map<String, String> userParams = new HashMap<>();
userParams.put("af_sub1", "val");
userParams.put("custom_param", "val2");
CrossPromotionHelper.logCrossPromoteImpression(this, appID, campaign, userParams);
```
```kotlin Kotlin
val appID = "com.appsflyer.promotedapp"
val campaign = "spring_sale_2026"
val userParams = HashMap<String, String>()
userParams["af_sub1"] = "val"
userParams["custom_param"] = "val2"
CrossPromotionHelper.logCrossPromoteImpression(this, appID, campaign, userParams)
```

## See also

- [CrossPromotionHelper](doc:android-sdk-reference-sharecrosspromotionhelper)
- [Conversion data](doc:conversion-data-android)
