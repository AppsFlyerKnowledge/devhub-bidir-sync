---
title: Unified Deep Linking (UDL)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
> ❗️ This page has a newer [version](https://dev.appsflyer.com/hc/docs/dl_android_unified_deep_linking)

**At a glance:** Unified deep linking (UDL) enables you to send new and existing users to a specific in-app activity (for example, a specific page in the app) as soon as the app is opened.

> 📘 UDL privacy protection
> 
> For new users, the UDL method only returns parameters relevant to deferred deep linking: `deep_link_value` and `deep_link_sub1-10`. If you try to get any other parameters (`media_source`, `campaign`, `af_sub1-5`, etc.), they return `null`.

## Flow

![](https://files.readme.io/7309a5f-6577_Unified_Deep_Link_Android.png "6577 Unified Deep Link Android.png")

UDL routes mobile users into a specific activity or content in an app.

The flow works as follows:

1. User clicks a OneLink link.
   - If the user has the app installed, the Android App Links or URI scheme opens the app.
   - If the user doesn't have the app installed, they are redirected to the app store, and after downloading, the user opens the app. 
2. The app open triggers the AppsFlyer SDK.
3. The AppsFlyer SDK runs the UDL API. 
4. The UDL API retrieves OneLink data from AppsFlyer servers. 
5. The UDL API calls back the [`onDeepLinking()`] method in the [`DeepLinkingListener`] class.
6. The [`onDeepLinking()`] method gets a [`DeepLinkResult`] object. 
7. The [`DeepLinkResult`] object includes:
   - Status (Found/Not found/Error)
   - A [`DeepLink`] object that carries the `deep_link_value` and `deep_link_sub1-10` parameters, that the developer uses to route the user to a specific in-app activity, which is the main goal of OneLink.

[`onDeepLinking()`]: https://dev.appsflyer.com/hc/docs/deeplinklistener#ondeeplinking

[`DeepLinkingListener`]: https://dev.appsflyer.com/hc/docs/deeplinklistener

[`DeepLinkResult`]: https://dev.appsflyer.com/hc/docs/deeplinkresult

[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink

## Prerequisites

- UDL requires AppsFlyer Android SDK V6.1+.

## Planning

When setting up OneLinks, the marketer uses parameters to create the links, and the developer customizes the behavior of the app based on the values received. It is the developer's responsibility to make sure the parameters are handled correctly in the app, for both in-app routing, and personalizing data in the link.

**To plan the OneLink:**

1. Get from the marketer the desired behavior and personal experience a user gets when they click the URL.
2. Based on the desired behavior, plan the `deep_link_value` and other parameters that are needed to give the user the desired personal experience.
   - The `deep_link_value` is set by the marketer in the URL and used by the developer to redirect the user to a specific place inside the app. For example, if you have a fruit store and want to direct users to apples, the value of `deep_link_value` can be `apples`.
   - The `deep_link_sub1-10` parameters can also be added to the URL to help personalize the user experience. For example, to give a 10% discount, the value of `deep_link_sub1` can be `10`. 

## Implementation


[block:tutorial-tile]
{
  "backgroundColor": "#5fd3a7",
  "emoji": "🤖",
  "id": "6457ad6ec05fc3006a322fc2",
  "link": "https://dev.appsflyer.com/v0.1/recipes/unified-deep-linking-udl-api-in-android",
  "slug": "unified-deep-linking-udl-api-in-android",
  "title": "Unified Deep Linking (UDL) API in Android"
}
[/block]




Implement the UDL API logic based on the chosen parameters and values.

1. Use the [`subscribeForDeepLink()`](https://dev.appsflyer.com/hc/docs/appsflyerlib#subscribefordeeplink) method (from `AppsFlyerLib`) to register the  [`DeepLinkListener`](https://dev.appsflyer.com/hc/docs/deeplinklistener) interface listener.
2. Make sure you override the callback function [`onDeepLinking()`](https://dev.appsflyer.com/hc/docs/deeplinklistener#ondeeplinking).  
   `onDeepLinking() ` accepts as an argument a [`DeepLinkResult`](https://dev.appsflyer.com/hc/docs/deeplinkresult) object. 
3. Use [`getStatus()`](https://dev.appsflyer.com/hc/docs/deeplinkresult#getstatus) to query whether the deep linking match is found.
4. For when the status is an error, call [`getError()`](https://dev.appsflyer.com/hc/docs/deeplinkresult#geterror) and run your error flow.
5. For when the status is found, use [`getDeepLink()`](https://dev.appsflyer.com/hc/docs/deeplinkresult#getdeeplink) to retrieve the [`DeepLink`](https://dev.appsflyer.com/hc/docs/deeplink) object.  
   The `DeepLink `object contains the deep linking information and helper functions to easily retrieve values from well-known OneLink keys, for example, [`getDeepLinkValue()`](https://dev.appsflyer.com/hc/docs/deeplink#getdeeplinkvalue).
6. Use [`getDeepLinkValue()`](https://dev.appsflyer.com/hc/docs/deeplink#getdeeplinkvalue) to retrieve the `deep_link_value`. 
7. Use [`getStringValue("deep_link_sub1")`](https://dev.appsflyer.com/hc/docs/deeplink#getstringvalue) to retrieve `deep_link_sub1`. Do the same for `deep_link_sub2-10` parameters, changing the string value as required.
8. Once `deep_link_value` and `deep_link_sub1-10` are retrieved, pass them to an in-app router and use them to personalize the user experience.

> 📘 Note
> 
> `onDeepLinking` is not called when the app is running in the background and Application LaunchMode is not standard. 
> 
> To correct this, call setIntent(intent) method to set the intent value inside the overridden method onNewIntent if the application is using a non-standard LaunchMode.
> 
> ```java
> import android.content.Intent;
> ...
> ...
> ...
> @Override
> protected void onNewIntent(Intent intent) 
> { 
>   super.onNewIntent(intent);     
>   setIntent(intent);
> }
> ```

### Code example

```java
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
                
                // ** Next if statement is optional **
                // Our sample app's user-invite carries the referrerID in deep_link_sub2
                // See the user-invite section in FruitActivity.java
                if (dlData.has("deep_link_sub2")){
                    referrerId = deepLinkObj.getStringValue("deep_link_sub2");
                    Log.d(LOG_TAG, "The referrerID is: " + referrerId);
                } else {
                    Log.d(LOG_TAG, "deep_link_sub2/Referrer ID not found");
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
                goToFruit(fruitName, deepLinkObj);
            }
        });
```
```kotlin
AppsFlyerLib.getInstance().subscribeForDeepLink(object : DeepLinkListener{
            override fun onDeepLinking(deepLinkResult: DeepLinkResult) {
                when (deepLinkResult.status) {
                    DeepLinkResult.Status.FOUND -> {
                        Log.d(
                            LOG_TAG,"Deep link found"
                        )
                    }
                    DeepLinkResult.Status.NOT_FOUND -> {
                        Log.d(
                            LOG_TAG,"Deep link not found"
                        )
                        return
                    }
                    else -> {
                        // dlStatus == DeepLinkResult.Status.ERROR
                        val dlError = deepLinkResult.error
                        Log.d(
                            LOG_TAG,"There was an error getting Deep Link data: $dlError"
                        )
                        return
                    }
                }
                var deepLinkObj: DeepLink = deepLinkResult.deepLink
                try {
                    Log.d(
                        LOG_TAG,"The DeepLink data is: $deepLinkObj"
                    )
                } catch (e: Exception) {
                    Log.d(
                        LOG_TAG,"DeepLink data came back null"
                    )
                    return
                }

                // An example for using is_deferred
                if (deepLinkObj.isDeferred == true) {
                    Log.d(LOG_TAG, "This is a deferred deep link");
                } else {
                    Log.d(LOG_TAG, "This is a direct deep link");
                }

                try {
                    val fruitName = deepLinkObj.deepLinkValue
                    Log.d(LOG_TAG, "The DeepLink will route to: $fruitName")
                } catch (e:Exception) {
                    Log.d(LOG_TAG, "There's been an error: $e");
                    return;
                }
            }
        })
```



⇲ Github links: [Java][udl_java]

[udl_java]: https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/master/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/AppsflyerBasicApp.java#L31-L70