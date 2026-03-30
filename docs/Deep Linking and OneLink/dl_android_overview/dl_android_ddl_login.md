---
title: Android Deep Linking post user event
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
## Overview

In some cases the user is required to go through some kind of an event in his journey from the main page to an application page pointed by deep linking destination.

This is relevant for both kinds of deep linking: deferred (new user) or direct (application already installed).

Examples for such user events:

1. Login process
2. Splash screen 
3. Consenting to usage terms.

## Implementation

In order to sync easily and safely between the user event and the deep linking flow, it is recommended to [initiate](https://dev.appsflyer.com/hc/docs/integrate-android-sdk#initializing-the-android-sdk) and [start](https://dev.appsflyer.com/hc/docs/integrate-android-sdk#deferring-sdk-start) the SDK in the `activity context` of the main page where the user event is performed.  This is different from the normal flow, where the SDK and initiated and started in the `application context`.  
The callbacks which are used in the [Extended Deferred Deep Linking](dl_android_ocds_ddl) flow should also be called in the `activity context`.  
It is the developer's responsibility to save the deep linking data and route the user to the required destination only after the event is performed.

## Code example

In [this](https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/tree/DDL_after_Login/java/basic_app) Github branch you can find a code sample which presents the main page before continuing to the deep linking destination. Once an authentication is performed, the user is directed to the destination.  
You can see that the [application context](https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/8dcb03c48199d5123e776463ae74e7dd274c6fdc/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/AppsflyerBasicApp.java#L11) has no AppsFlyer SDK code. The AppsFlyer code moved entirely into the main [activity](https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/8dcb03c48199d5123e776463ae74e7dd274c6fdc/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/LoginActivity.java#L29) which performs the user event.