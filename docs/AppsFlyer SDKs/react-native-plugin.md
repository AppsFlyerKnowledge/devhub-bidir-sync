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

[block:html]
{
  "html": "<style>\n  .containerBox {\n    right: 0;\n    display: flex;\n    justify-content: flex-start;\n    border-radius: 10px;\n    padding: 20px 10px;\n    padding-right: 50px;\n    padding-top: 10px;\n  }\n .djButton {\n    padding: 8px 16px;\n    border-radius: 4px;\n    text-decoration: none;\n    color: white;\n    font-weight: 600;\n   \tcursor: pointer;\n    border: none;\n    background-color: rgb(3, 109, 235) !important;\n  }\n  \n  .djButton:hover {\n  \tbackground-color: #0360ce !important;\n    transition: 0.3s;\n  }\n</style>\n\n<div class=\"containerBox\">\n  <img src=\"https://dj.dev.appsflyer.com/images/DJ_illustratration.svg\" style=\"width: 120px; margin: 0 0; margin-right: 20px\">\n  <div>\n  \n      <h3>\n        Get started with our React Native integration wizard\n    </h3>\n    <button onclick=\"window.open('https://dj.dev.appsflyer.com/?sourceos=reactnative_android&utm_source=devhub&utm_medium=install-rn-android-sdk');gtag('event', 'click', {'event_category': 'DJ_Banner', 'event_label': 'DJ_Rn_Anrd_install', 'value': '1'});\" target=\"_blank\" class=\"djButton\">\n      Android device\n    </button>\n    <button onclick=\"window.open('https://dj.dev.appsflyer.com/?sourceos=reactnative_ios&utm_source=devhub&utm_medium=install-rn-ios-sdk');gtag('event', 'click', {'event_category': 'DJ_Banner', 'event_label': 'DJ_Rn_ios_install', 'value': '1'});\" target=\"_blank\" class=\"djButton\">\n      iOS device\n      </button>\n\n  </div>\n</div>\n"
}
[/block]


# Sample App

[React Native Expo Sample App](https://github.com/AppsFlyerSDK/appsflyer-expo-sample-app)

![](https://files.readme.io/6f31ea9-Screenshot_2023-05-23_at_17.05.33.png)

## Plugin Github Repository

> 📘 Github repository for this plugin is [here](https://github.com/AppsFlyerSDK/appsflyer-react-native-plugin)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)  
[![npm version](https://badge.fury.io/js/react-native-appsflyer.svg)](https://badge.fury.io/js/react-native-appsflyer)  
[![Downloads](https://img.shields.io/npm/dm/react-native-appsflyer.svg)](https://www.npmjs.com/package/react-native-appsflyer)

### Native SDKs compatibility

- Android AppsFlyer SDK **v6.13.0**
- iOS AppsFlyer SDK **v6.13.1**

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