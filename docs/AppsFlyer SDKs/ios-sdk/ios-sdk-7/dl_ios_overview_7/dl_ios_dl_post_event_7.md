---
title: iOS Deep Linking post user event
hidden: false
---

## Overview
In some cases the user is required to go through some kind of an event before continuing to application page pointed by deep linking destination.
Examples for such user events:
1. Login process
2. Splash screen 
3. Consenting to usage terms

## Implementation
In order to sync easily and safely between the user event and the deferred deep linking flow, it is recommended to [start](https://dev.appsflyer.com/hc/docs/integrate-ios-sdk-7#with-pre-conditions) the SDK only after the user event is performed. In SDK 7, call `registerSessionReadyListener` after `initialize`, and call `start` once the SDK is ready and the user event is complete, for example after the authentication completes.
It is the developer's responsibility to save the deferred deep linking and direct deep linking data, route the user to the required destination only after the event is performed.

## Code example
In [this](https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/tree/DDL_after_login/swift/basic_app/basic_app) Github branch you can find a code sample which waits for a pseudo-user authencation before continuing to the deep linking destination. Once the authentication is verified, the user is directed to the destination. This flow is relevant for both deferred deep linking and direct deep linking (when the app is already installed).
You can see that the [`AppDelgate`](https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/d0e1eeb3da6213830684e72626af5fd1ad0cea40/swift/basic_app/basic_app/AppDelegate.swift#L20) has no AppsFlyer SDK initiazation, except the optional `AppTrackingTransparency` code. The AppsFlyer SDK moved into the [main view controler](https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/d0e1eeb3da6213830684e72626af5fd1ad0cea40/swift/basic_app/basic_app/MainViewController.swift#L13) which performs the user event (authentication in this case).
