---
title: Unified Deep Linking (UDL) API in Android
description: >-
  Step-by-step guidance for easy integration of Unified Deep Linking in an
  Android application
hidden: false
recipe:
  color: '#5fd3a7'
  icon: 🤖
---
```java AppsflyerBasicApp
package com.appsflyer.onelink.appsflyeronelinkbasicapp;


import com.appsflyer.AppsFlyerLib;
import com.appsflyer.deeplink.DeepLink;
import com.appsflyer.deeplink.DeepLinkListener;
import com.appsflyer.deeplink.DeepLinkResult;
import com.appsflyer.AppsFlyerConversionListener;

import android.app.Application;
import android.content.Intent;
import android.util.Log;
import com.google.gson.Gson;
import androidx.annotation.NonNull;
import java.util.Map;
import java.util.Objects;

public class AppsflyerBasicApp extends Application {
    public static final String LOG_TAG = "AppsFlyerFeedMeApp";
    public static final String DL_ATTRS = "dl_attrs";
    @Override
    public void onCreate() {
        super.onCreate();
        
        String afDevKey = AppsFlyerConstants.afDevKey;
        AppsFlyerLib appsflyer = AppsFlyerLib.getInstance();
        // The following line is for debug. Consider removing in production
        // appsflyer.setDebugLog(true);

        appsflyer.subscribeForDeepLink(new DeepLinkListener(){
            @Override
            public void onDeepLinking(@NonNull DeepLinkResult deepLinkResult) {
                DeepLinkResult.Status dlStatus = deepLinkResult.getStatus();
                if (dlStatus == DeepLinkResult.Status.FOUND) {
                    Log.d(LOG_TAG, "Deep link found");
                } else if (dlStatus == DeepLinkResult.Status.NOT_FOUND) {
                    Log.d(LOG_TAG, "Deep link not found");
                    return;
                } else {
                    // dlStatus == DeepLinkResult.Status.ERROR
                    DeepLinkResult.Error dlError = deepLinkResult.getError();
                    Log.d(LOG_TAG, "There was an error getting Deep Link data: " + dlError.toString());
                    return;
                }
                DeepLink deepLinkObj = deepLinkResult.getDeepLink();
                try {
                    Log.d(LOG_TAG, "The DeepLink data is: " + deepLinkObj.toString());
                } catch (Exception e) {
                    Log.d(LOG_TAG, "DeepLink data came back null");
                    return;
                }
                // An example for using is_deferred
                if (deepLinkObj.isDeferred()) {
                    Log.d(LOG_TAG, "This is a deferred deep link");
                } else {
                    Log.d(LOG_TAG, "This is a direct deep link");
                }
                // An example for using a generic getter
                String fruitName = "";
                try {
                    fruitName = deepLinkObj.getDeepLinkValue();
                    Log.d(LOG_TAG, "The DeepLink will route to: " + fruitName);
                } catch (Exception e) {
                    Log.d(LOG_TAG, "Custom param fruit_name was not found in DeepLink data");
                    return;
                }
                // ** Next if statement is optional **
                // Our sample app's user-invite carries the referrerID in deep_link_sub2
                // See the user-invite section in FruitActivity.java
                if (dlData.has("deep_link_sub2")){
                    referrerId = deepLinkObj.getStringValue("deep_link_sub2");
                    Log.d(LOG_TAG, "The referrerID is: " + referrerId);
                } else {
                    Log.d(LOG_TAG, "deep_link_sub2/Referrer ID not found");
                }
                goToFruit(fruitName, deepLinkObj);
            }
        });

        AppsFlyerConversionListener conversionListener =  new AppsFlyerConversionListener() {
            @Override
            public void onConversionDataSuccess(Map<String, Object> conversionData) {
                for (String attrName : conversionData.keySet())
                    Log.d(LOG_TAG, "Conversion attribute: " + attrName + " = " + conversionData.get(attrName));
                String status = Objects.requireNonNull(conversionData.get("af_status")).toString();
                if(status.equals("Non-organic")){
                    if( Objects.requireNonNull(conversionData.get("is_first_launch")).toString().equals("true")){
                        Log.d(LOG_TAG,"Conversion: First Launch");
                    } else {
                        Log.d(LOG_TAG,"Conversion: Not First Launch");
                    }
                } else {
                    Log.d(LOG_TAG,"Conversion: This is an organic install.");
                }
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

    private void goToFruit(String fruitName, DeepLink dlData) {
        String fruitClassName = fruitName.concat("Activity");
        try {
            Class fruitClass = Class.forName(this.getPackageName().concat(".").concat(fruitClassName));
            Log.d(LOG_TAG, "Looking for class " + fruitClass);
            Intent intent = new Intent(getApplicationContext(), fruitClass);
            if (dlData != null) {
                // TODO - make DeepLink Parcelable
                String objToStr = new Gson().toJson(dlData);
                intent.putExtra(DL_ATTRS, objToStr);
            }
            intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            startActivity(intent);
        } catch (ClassNotFoundException e) {
            Log.d(LOG_TAG, "Deep linking failed looking for " + fruitName);
            e.printStackTrace();
        }
    }

}
```

# Set up your app for deep linking



View the [guide](https://dev.appsflyer.com/hc/docs/initial-setup-for-deep-linking-and-deferred-deep-linking) for app-opening and deep linking setup.

# Start the SDK to run UDL API

<!-- java@4,113-114 -->

1. Import [`AppsFlyerLib`]
2. Run `init()` 
3. Run `start()`

View a detailed recipe to [start the SDK and get attribution data](https://dev.appsflyer.com/hc/recipes/starting-the-sdk-and-getting-attribution-data)

[`AppsFlyerLib`]: https://dev.appsflyer.com/hc/docs/appsflyerlib

# Import UDL objects

<!-- java@5-7 -->



# Retrieve an AppsFlyerLib instance

<!-- java@26 -->

It will be hosted in the `appsflyer` variable.

# Create and subscribe the DeepLinkListener

<!-- java@30 -->

Pass a newly created [`DeepLinkListener`] object to [`subscribeForDeepLink()`].

[`subscribeForDeepLink()`]: https://dev.appsflyer.com/hc/docs/appsflyerlib#subscribefordeeplink
[`DeepLinkListener`]: https://dev.appsflyer.com/hc/docs/deeplinklistener

# Implement onDeepLinking()

<!-- java@31-32 -->

Override [`onDeepLinking()`] in the [`DeepLinkListener`] object.

[`onDeepLinking()`] is called when a UDL API gets a result from AppsFlyer servers.
The result is summarized in a [`DeepLinkResult`] object that is passed back to the [`onDeepLinking()`].

[`DeepLinkListener`]: https://dev.appsflyer.com/hc/docs/deeplinklistener
[`onDeepLinking()`]: https://dev.appsflyer.com/hc/docs/deeplinklistener#ondeeplinking
[`DeepLinkResult`]: https://dev.appsflyer.com/hc/docs/deeplinkresult

# Retrieve UDL status

<!-- java@33-44 -->

Use [`getStatus()`] to get the [`Status`] of the UDL API query. 
- `Found` allows you to continue the deep linking flow.
- `Not Found` ends the deep linking flow
- `Error` should trigger your error reporting.

[`getStatus()`]: https://dev.appsflyer.com/hc/docs/deeplinkresult#getstatus
[`Status`]: https://dev.appsflyer.com/hc/docs/deeplinkresult#status

# Get the deep linking data

<!-- java@45-51 -->

Use [`getDeepLink()`] to retrieve the [`DeepLink`] object. This object contains the deep link data, accessible using predefined methods in the object.

[`getDeepLink()`]: https://dev.appsflyer.com/hc/docs/deeplinkresult#getdeeplink
[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink

# Check for deferred deep linking flow

<!-- java@53-57 -->

[`isDeferred()`] checks if this flow is a deferred deep linking flow.

Deferred deep linking is when the app isn't installed when the user clicks the OneLink URL, and deep linking data is retrieved after the user installs and opens the app.

[`isDeferred()`]: https://dev.appsflyer.com/hc/docs/deeplink#isdeferred
[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink

# Use deep_link_value for in-app routing

<!-- java@61 -->

A user can be immediately routed to an activity inside the app based on the `deep_link_value` associated with the link the user clicked.

The `deep_link_value` is retrieved using [`getDeepLinkValue()`] in the [`DeepLink`] object.

View [additional methods](https://dev.appsflyer.com/hc/docs/deeplink#methods) to retrieve other deep link data.

[`getDeepLinkValue()`]: https://dev.appsflyer.com/hc/docs/deeplink#getdeeplinkvalue
[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink

# Implement in-app routing

<!-- java@76,117-134 -->

In this example we implemented in-app routing in the method `goToFruit`.

This in-app router receives the fruit names in the `deep_link_value` and routes the user to the correct fruit page (*apples*, *bananas*, or *peaches*).

Together with the fruit name, we pass the [`DeepLink`] object to the fruit page just in case we need more data for a better user experience.

[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink

# [Optional] Get conversion data

<!-- java@80-111 -->

In some cases the developer wants to retrieve the attribution data of the install. 

View the recipe to [get conversion data](https://dev.appsflyer.com/hc/recipes/get-conversion-data-in-android).