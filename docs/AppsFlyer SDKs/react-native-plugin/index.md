---
title: React Native Plugin
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Recommended

<HTMLBlock>{`
<style>
  .containerBox {
    right: 0;
    display: flex;
    justify-content: flex-start;
    border-radius: 10px;
    padding: 20px 10px;
    padding-right: 50px;
    padding-top: 10px;
  }
 .djButton {
    padding: 8px 16px;
    border-radius: 4px;
    text-decoration: none;
    color: white;
    font-weight: 600;
   	cursor: pointer;
    border: none;
    background-color: rgb(3, 109, 235) !important;
  }
  
  .djButton:hover {
  	background-color: #0360ce !important;
    transition: 0.3s;
  }
</style>

<div class="containerBox">
  <img src="https://dj.dev.appsflyer.com/images/DJ_illustratration.svg" style="width: 120px; margin: 0 0; margin-right: 20px">
  <div>
  
      <h3>
        Get started with our React Native integration wizard
    </h3>
    <button onclick="window.open('https://dj.dev.appsflyer.com/?sourceos=reactnative_android&utm_source=devhub&utm_medium=install-rn-android-sdk');gtag('event', 'click', {'event_category': 'DJ_Banner', 'event_label': 'DJ_Rn_Anrd_install', 'value': '1'});" target="_blank" class="djButton">
      Android device
    </button>
    <button onclick="window.open('https://dj.dev.appsflyer.com/?sourceos=reactnative_ios&utm_source=devhub&utm_medium=install-rn-ios-sdk');gtag('event', 'click', {'event_category': 'DJ_Banner', 'event_label': 'DJ_Rn_ios_install', 'value': '1'});" target="_blank" class="djButton">
      iOS device
      </button>

  </div>
</div>
`}</HTMLBlock>

# Sample App

[React Native Expo Sample App](https://github.com/AppsFlyerSDK/appsflyer-expo-sample-app)

![](https://files.readme.io/6f31ea9-Screenshot_2023-05-23_at_17.05.33.png)

## Plugin Github Repository

<Callout icon="📘" theme="info">
  ### Github repository for this plugin is [here](https://github.com/AppsFlyerSDK/appsflyer-react-native-plugin)
</Callout>

### Native SDKs compatibility

- Android AppsFlyer SDK _v7.0.1_
- iOS AppsFlyer SDK _v7.0.2_

  Requires React Native _≥0.76.0_ with the New Architecture (TurboModules). On older architecture, stay on the plugin's `6.x` line — see [MIGRATION.md](https://github.com/AppsFlyerSDK/appsflyer-react-native-plugin/blob/master/MIGRATION.md)

## ❗ Breaking changes when updating to v6.x.x❗❗

- From version `6.3.0`, we use `xcframework` for iOS platform. Then you need to use cocoapods version >= 1.10

- From version `6.2.30`, `logCrossPromotionAndOpenStore` api will register as `af_cross_promotion` instead of `af_app_invites` in your dashboard.<br /><br />Click on a link that was generated using `generateInviteLink` api will be register as `af_app_invites`.

- From version `6.0.0` we have renamed the following APIs:

| Old API                       | New API                       |
| ----------------------------- | ----------------------------- |
| trackEvent                    | logEvent                      |
| trackLocation                 | logLocation                   |
| stopTracking                  | stop                          |
| trackCrossPromotionImpression | logCrossPromotionImpression   |
| trackAndOpenStore             | logCrossPromotionAndOpenStore |
| setDeviceTrackingDisabled     | anonymizeUser                 |
| AppsFlyerTracker              | AppsFlyerLib                  |

And removed the following ones:

- trackAppLaunch -> no longer needed. See new init guide
- sendDeepLinkData -> no longer needed. See new init guide
- enableUninstallTracking -> no longer needed. See new uninstall measurement guide

If you have used 1 of the removed APIs, please check the integration guide for the updated instructions.

***

## 🚀 Getting Started

- [Installation](https://dev.appsflyer.com/hc/docs/rn_installation)
- [Expo Installation](https://dev.appsflyer.com/hc/docs/rn_expoinstallation)
- [Integration](https://dev.appsflyer.com/hc/docs/rn_integration)
- [Test integration](/Docs/Testing.md)
- [In-app events](https://dev.appsflyer.com/hc/docs/rn_inappevents)
- [Uninstall measurement](/Docs/UninstallMeasurement.md)

## 🔗 Deep Linking

- [Integration](https://dev.appsflyer.com/hc/docs/rn_deeplinkintegrate)
- [Expo Integration](https://dev.appsflyer.com/hc/docs/rn_expodeeplinkintegration)
- [Unified Deep Link (UDL)](https://dev.appsflyer.com/hc/docs/rn_unifieddeeplink)
- [User Invite](https://dev.appsflyer.com/hc/docs/rn_userinvite)

## 🧪 Sample Apps

- [React-Native Sample App](/demos/appsflyer-react-native-app)
- [Expo Sample App](/demos/appsflyer-expo-app)

### [API reference](https://dev.appsflyer.com/hc/docs/rn_api)
