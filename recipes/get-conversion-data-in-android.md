---
title: Get Conversion Data in Android
description: >-
  For developers who wish to learn step-by-step how to receive conversion data
  using the GetConversionData interface
hidden: false
recipe:
  color: '#018FF4'
  icon: 🐣
---
```java Java
package com.appsflyer.onelink.appsflyeronelinkbasicapp;

import com.appsflyer.AppsFlyerLib;
import com.appsflyer.AppsFlyerConversionListener;

import android.app.Application;
import android.util.Log;
import java.util.Map;
import java.util.Objects;

public class AppsflyerBasicApp extends Application {
    public static final String LOG_TAG = "AppsFlyerOneLinkSimApp";
    public static final String DL_ATTRS = "dl_attrs";
    Map<String, Object> conversionData = null;

    @Override
    public void onCreate() {
        super.onCreate();
        String afDevKey = AppsFlyerConstants.afDevKey;
        AppsFlyerLib appsflyer = AppsFlyerLib.getInstance();
        appsflyer.setMinTimeBetweenSessions(0);
        appsflyer.setDebugLog(true);

        AppsFlyerConversionListener conversionListener =  new AppsFlyerConversionListener() {
            @Override
            public void onConversionDataSuccess(Map<String, Object> conversionDataMap) {
                for (String attrName : conversionDataMap.keySet())
                    Log.d(LOG_TAG, "Conversion attribute: " + attrName + " = " + conversionDataMap.get(attrName));
                String status = Objects.requireNonNull(conversionDataMap.get("af_status")).toString();
                if(status.equals("Non-organic")){
                    if( Objects.requireNonNull(conversionDataMap.get("is_first_launch")).toString().equals("true")){
                        Log.d(LOG_TAG,"Conversion: First Launch");
                    } else {
                        Log.d(LOG_TAG,"Conversion: Not First Launch");
                    }
                } else {
                    Log.d(LOG_TAG, "Conversion: This is an organic install.");
                }
                conversionData = conversionDataMap;
            }

            @Override
            public void onConversionDataFail(String errorMessage) {
                Log.d(LOG_TAG, "error getting conversion data: " + errorMessage);
            }

            @Override
            public void onAppOpenAttribution(Map<String, String> attributionData) {
                Log.d(LOG_TAG, "onAppOpenAttribution: This is fake call.");
            }

            @Override
            public void onAttributionFailure(String errorMessage) {
                Log.d(LOG_TAG, "error onAttributionFailure : " + errorMessage);
            }
        };
        appsflyer.init(afDevKey, conversionListener, this);
        appsflyer.start(this);
    }
}
```

# Create an AppsFlyerConversionListener object

<!-- java@24 -->

Save the newly created object in a variable (For example, `conversionListener`) and pass it to [`init`](doc:android-sdk-reference-appsflyerlib#init).

# Implement onConversionDataSuccess

<!-- java@25-26 -->

[`onConversionDataSuccess`] is called on **every** app launch. 

The attribution data is passed as a parameter from type `Map<String, Object>`.
The input parameters are listed [here](https://dev.appsflyer.com/hc/docs/android-legacy-apis#input-parameters).

[`onConversionDataSuccess`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerconversionlistener#onconversiondatasuccess

# Detect successful attribution

<!-- java@29-30,36-38 -->

If `af_status` field in [`conversionData`] equals `Non-organic`, this install is **successfully attributed** by AppsFlyer, and other fields are valid.

# Detect first launch

<!-- java@31-35 -->

[`onConversionDataSuccess`] is called on **every** app launch.
If `is_first_launch` field in [`conversionData`] equals `"true"`, this is the very first launch.

[`conversionData`]: https://dev.appsflyer.com/hc/docs/android-legacy-apis#input-parameters
[`onConversionDataSuccess`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerconversionlistener#onconversiondatasuccess

# Handle attribution failures

<!-- java@42-45 -->

If the attribution operations fails (*from the SDK side*), [`onConversionDataFail`] is called. Implement here error handling and reporting 

[`onConversionDataFail`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerconversionlistener#onconversiondatafail

# Implement legacy methods required for compilation

<!-- java@47-55 -->

[`AppsFlyerConversionListener`] requires implementing all public methods. This includes [`onAppOpenAttribution`] and [`onAttributionFailure`].

[`AppsFlyerConversionListener`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerconversionlistener
[`onAppOpenAttribution`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerconversionlistener#onappopenattribution
[`onAttributionFailure`]:
https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerconversionlistener#onattributionfailure