---
title: Starting the SDK in Android
description: For developers who wish to learn step-by-step how to start the SDK
hidden: false
recipe:
  color: '#0197f4'
  icon: 🥇
---
```java AppsflyerBasicApp
package com.appsflyer.onelink.appsflyeronelinkbasicapp;

import com.appsflyer.AppsFlyerLib;

import android.app.Application;
import java.util.Map;

public class AppsflyerBasicApp extends Application {
    public static final String LOG_TAG = "AppsFlyerOneLinkSimApp";
    public static final String DL_ATTRS = "dl_attrs";
    Map<String, Object> conversionData = null;

    @Override
    public void onCreate() {
        super.onCreate();
        String afDevKey = AppsFlyerConstants.afDevKey;
        AppsFlyerLib appsflyer = AppsFlyerLib.getInstance();
        // For debug - remove in production
        appsflyer.setDebugLog(true);
        //optional
        appsflyer.setMinTimeBetweenSessions(0);
        
        appsflyer.init(afDevKey, null, this);
        appsflyer.start(this);
    }
}
```

# Import AppsFlyerLib

<!-- java@3 -->

This class contains core SDK functionality, including [`init`](doc:android-sdk-reference-appsflyerlib#init) and [`start`](doc:android-sdk-reference-appsflyerlib#start).

# Init the SDK

<!-- java@23 -->

Initialize the SDK using the [`init`](doc:android-sdk-reference-appsflyerlib#init) method.
Instruction on how to obtain the *dev key* can be found [here](https://support.appsflyer.com/hc/en-us/articles/207032126-Android-SDK-integration-for-developers#integration-31-retrieve-your-dev-key).
> The second argument is a Nullable [`AppsFlyerConversionListener`]. You can pass `null` if you don't use [conversion data](doc:conversion-data-android).

[`AppsFlyerConversionListener`]: https://dev.appsflyer.com/hc/docs/appsflyerconversionlistener

# Start the SDK

<!-- java@24 -->

Run [`start`](doc:android-sdk-reference-appsflyerlib#start) to start the SDK.

This is **required** for [`AppsFlyerConversionListener`](doc:android-sdk-reference-appsflyerconversionlistener)

[`AppsFlyerConversionListener`]: https://dev.appsflyer.com/hc/docs/appsflyerconversionlistener