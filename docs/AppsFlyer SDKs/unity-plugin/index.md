---
title: Unity Plugin
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Getting started

- [Installation](https://dev.appsflyer.com/hc/docs/installation)
- [Integration](https://dev.appsflyer.com/hc/docs/basicintegration)
- [Testing](https://dev.appsflyer.com/hc/docs/testing)
- [In-app events](https://dev.appsflyer.com/hc/docs/inappevents)
- [Conversion data](https://dev.appsflyer.com/hc/docs/conversion-data-unity)
- [Push notifications](https://dev.appsflyer.com/hc/docs/pushnotifications)
- [Uninstall measurement](https://dev.appsflyer.com/hc/docs/uninstallmeasurement)
- [Ad revenue](https://dev.appsflyer.com/hc/docs/ad-revenue-unity)
- [Send consent for DMA compliance](https://dev.appsflyer.com/hc/docs/dmaconsent)

## Purchases and subscriptions

- [Validate and log (manual integration)](https://dev.appsflyer.com/hc/docs/validate-and-log-unity)
- [Purchase Connector (ROI360, automated integration)](https://dev.appsflyer.com/hc/docs/purchase-connector-unity)

## Reference

- [API reference](https://dev.appsflyer.com/hc/docs/api)
- [Troubleshooting](https://dev.appsflyer.com/hc/docs/troubleshooting)

🛠 In order for us to provide optimal support, please contact AppsFlyer support through the Customer Assistant Chatbot for assistance with troubleshooting issues or product guidance.<br />To do so, please follow [this article](https://support.appsflyer.com/hc/en-us/articles/23583984402193-Using-the-Customer-Assistant-Chatbot)

## ButterFlyer Sample App

> [ButterFlyer - Unity Sample App](https://github.com/AppsFlyerSDK/appsflyer-unity-sample-app)

![](https://files.readme.io/3cdc241-banner-butterflyer.png "banner-butterflyer.png")

## Plugin Github Repository

> 📘 Github repository for this plugin is [here](https://github.com/AppsFlyerSDK/appsflyer-unity-plugin)

### This plugin is built for

- Android AppsFlyer SDK v6.17.5
- Android Purchase Connector 2.2.0
- iOS AppsFlyer SDK v6.17.8
- iOS Purchase Connector 6.17.8

---

## Release notes and known issues

> 📌 Two versions of Unity Plugin v6.17.7
>
> We have released **two** versions of the AppsFlyer Unity plugin to support teams at different stages of migrating to **Google Play Billing Library v8.0.0**.
>
> **Option A — `v6.17.81` (Billing Library v8)**: Support for Google Play Billing Library 8.0.0 on Android (Android Purchase Connector version 2.2.0). May introduce breaking changes for apps that have not yet migrated to the Billing v8 APIs. If you choose this option, update Unity IAP (`com.unity.purchasing`) to version 5.0.0 or newer.
>
> **Option B — `v6.17.80` (Billing Library v7)**: For developers not ready to adopt Billing v8. Bundled SDKs: iOS SDK 6.17.8 and Android SDK 6.17.5, Android Purchase Connector 2.1.2. Lets you update the AppsFlyer SDKs without changing your existing (pre-v8) billing integration.

**New in 6.17.1 — Purchase Connector Integration**: Starting from version 6.17.1, the Purchase Connector is integrated directly into the main AppsFlyer Unity plugin. You no longer need to download, import, or maintain a separate Purchase Connector package. If you were previously using the standalone Purchase Connector from a separate repository, remove any references to `using AppsFlyerConnector;`, its functionality is now included in the main plugin under the `AppsFlyerSDK` namespace. The Purchase Connector now supports StoreKit 2 for iOS 15+ alongside the existing StoreKit 1 support.

**Breaking changes when updating to 6.17.5**: The `validateAndSendInAppPurchase` method signatures have been updated for better type safety and cleaner code. V2 methods using structured data classes (`AFPurchaseDetailsAndroid`/`AFSDKPurchaseDetailsIOS`) are now recommended. The old string-based parameter methods are deprecated but maintained for backward compatibility.

**Breaking changes when updating to 6.12.20**: Starting from version 6.12.20, the UPM branches no longer hold a dependency for `com.google.external-dependency-manager` (EDM4U), since that dependency isn't reliably available via UPM. It's still required to use the plugin, download a suitable version of EDM4U separately via `.unitypackage` or `.tgz`, or opt for [an installation without EDM4U](https://github.com/AppsFlyerSDK/appsflyer-unity-plugin/blob/master/docs/Installation.md#installation-without-unity-jar-resolver).

**Breaking changes when updating to 6.6.0**: Since version 6.6.0, there's no need to differentiate between iOS and Android APIs, all APIs are called with the `AppsFlyer` class. Since version 6.10.10, most APIs require `initSDK` to be called before use, except `setIsDebug`, `setCurrencyCode`, `setHost`, and `disableSKAdNetwork`.

```c#
// Before 6.6.0
#if UNITY_IOS && !UNITY_EDITOR
    AppsFlyeriOS.waitForATTUserAuthorizationWithTimeoutInterval(60);
#endif

// After 6.6.0
#if UNITY_IOS && !UNITY_EDITOR
    AppsFlyer.waitForATTUserAuthorizationWithTimeoutInterval(60);
#endif
```

## Strict Mode

The plugin supports a Strict Mode which completely removes the IDFA collection functionality and AdSupport framework dependencies. Use Strict Mode when developing apps for kids, for example. More information about how to install Strict Mode is available [here](/docs/Installation.md).

### AD_ID permission for Android

In v6.8.0 of the AppsFlyer SDK, we added the normal permission `com.google.android.gms.permission.AD_ID` to the SDK's AndroidManifest, to allow the SDK to collect the Android Advertising ID on apps targeting API 33. If your app is targeting children, you need to revoke this permission to comply with Google's Data policy. Read more [here](https://dev.appsflyer.com/hc/docs/install-android-sdk#the-ad_id-permission).
