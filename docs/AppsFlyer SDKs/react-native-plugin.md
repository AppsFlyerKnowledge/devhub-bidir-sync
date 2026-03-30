---
title: React Native Plugin
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
## Sample App

[React Native Expo Sample App](https://github.com/AppsFlyerSDK/appsflyer-expo-sample-app)

![](https://files.readme.io/6f31ea9-Screenshot_2023-05-23_at_17.05.33.png)

## Plugin Github Repository

> 📘 Github repository for this plugin is [here](https://github.com/AppsFlyerSDK/appsflyer-react-native-plugin)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)  
[![npm version](https://badge.fury.io/js/react-native-appsflyer.svg)](https://badge.fury.io/js/react-native-appsflyer)  
[![Downloads](https://img.shields.io/npm/dm/react-native-appsflyer.svg)](https://www.npmjs.com/package/react-native-appsflyer)

### Native SDKs compatibility

- Android AppsFlyer SDK **v6.10.3**
- iOS AppsFlyer SDK **v6.10.1**

## ❗ Breaking changes when updating to v6.x.x❗❗

- From version `6.3.0`, we use `xcframework` for iOS platform. Then you need to use cocoapods version >= 1.10

- From version `6.2.30`, `logCrossPromotionAndOpenStore` api will register as `af_cross_promotion` instead of `af_app_invites` in your dashboard.<br>  
  Click on a link that was generated using `generateInviteLink` api will be register as `af_app_invites`.

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