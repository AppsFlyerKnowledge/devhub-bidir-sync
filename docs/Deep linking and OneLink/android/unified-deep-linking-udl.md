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
[block:callout]
{
  "type": "danger",
  "title": "This page has a newer [version](https://dev.appsflyer.com/hc/docs/dl_android_unified_deep_linking)"
}
[/block]
**At a glance:** Unified deep linking (UDL) enables you to send new and existing users to a specific in-app activity (for example, a specific page in the app) as soon as the app is opened.
[block:callout]
{
  "type": "info",
  "body": "For new users, the UDL method only returns parameters relevant to deferred deep linking: `deep_link_value` and `deep_link_sub1-10`. If you try to get any other parameters (`media_source`, `campaign`, `af_sub1-5`, etc.), they return `null`.",
  "title": "UDL privacy protection"
}
[/block]

[block:api-header]
{
  "title": "Flow"
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7309a5f-6577_Unified_Deep_Link_Android.png",
        "6577 Unified Deep Link Android.png",
        1920,
        2680,
        "#f0f5fa"
      ]
    }
  ]
}
[/block]
UDL routes mobile users into a specific activity or content in an app.

The flow works as follows:

1. User clicks a OneLink link.
   * If the user has the app installed, the Android App Links or URI scheme opens the app.
   * If the user doesn't have the app installed, they are redirected to the app store, and after downloading, the user opens the app. 
2. The app open triggers the AppsFlyer SDK.
3. The AppsFlyer SDK runs the UDL API. 
4. The UDL API retrieves OneLink data from AppsFlyer servers. 
5. The UDL API calls back the [`onDeepLinking()`] method in the [`DeepLinkingListener`] class.
6. The [`onDeepLinking()`] method gets a [`DeepLinkResult`] object. 
7. The [`DeepLinkResult`] object includes:
   * Status (Found/Not found/Error)
   * A [`DeepLink`] object that carries the `deep_link_value` and `deep_link_sub1-10` parameters, that the developer uses to route the user to a specific in-app activity, which is the main goal of OneLink.

[`onDeepLinking()`]: https://dev.appsflyer.com/hc/docs/deeplinklistener#ondeeplinking
[`DeepLinkingListener`]: https://dev.appsflyer.com/hc/docs/deeplinklistener
[`DeepLinkResult`]: https://dev.appsflyer.com/hc/docs/deeplinkresult
[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink
[block:api-header]
{
  "title": "Prerequisites"
}
[/block]
* UDL requires AppsFlyer Android SDK V6.1+.
[block:api-header]
{
  "title": "Planning"
}
[/block]
When setting up OneLinks, the marketer uses parameters to create the links, and the developer customizes the behavior of the app based on the values received. It is the developer's responsibility to make sure the parameters are handled correctly in the app, for both in-app routing, and personalizing data in the link.

**To plan the OneLink:**
1. Get from the marketer the desired behavior and personal experience a user gets when they click the URL.
2. Based on the desired behavior, plan the `deep_link_value` and other parameters that are needed to give the user the desired personal experience.
   * The `deep_link_value` is set by the marketer in the URL and used by the developer to redirect the user to a specific place inside the app. For example, if you have a fruit store and want to direct users to apples, the value of `deep_link_value` can be `apples`.
   * The `deep_link_sub1-10` parameters can also be added to the URL to help personalize the user experience. For example, to give a 10% discount, the value of `deep_link_sub1` can be `10`. 

[block:api-header]
{
  "title": "Implementation"
}
[/block]

[block:tutorial-tile]
{
  "title": "Unified Deep Linking (UDL) API in Android",
  "emoji": "🤖",
  "backgroundColor": "#5fd3a7",
  "slug": "unified-deep-linking-udl-api-in-android",
  "_id": "63d045aa5e96a4004651464c",
  "id": "63d045aa5e96a4004651464c",
  "link": "https://dev.appsflyer.com/v0.1/recipes/unified-deep-linking-udl-api-in-android"
}
[/block]
Implement the UDL API logic based on the chosen parameters and values.

1. Use the [`subscribeForDeepLink()`](https://dev.appsflyer.com/hc/docs/appsflyerlib#subscribefordeeplink) method (from `AppsFlyerLib`) to register the  [`DeepLinkListener`](https://dev.appsflyer.com/hc/docs/deeplinklistener) interface listener.
2. Make sure you override the callback function [`onDeepLinking()`](https://dev.appsflyer.com/hc/docs/deeplinklistener#ondeeplinking). 
`onDeepLinking() ` accepts as an argument a [`DeepLinkResult`](https://dev.appsflyer.com/hc/docs/deeplinkresult) object. 
4. Use [`getStatus()`](https://dev.appsflyer.com/hc/docs/deeplinkresult#getstatus) to query whether the deep linking match is found.
5. For when the status is an error, call [`getError()`](https://dev.appsflyer.com/hc/docs/deeplinkresult#geterror) and run your error flow.
6. For when the status is found, use [`getDeepLink()`](https://dev.appsflyer.com/hc/docs/deeplinkresult#getdeeplink) to retrieve the [`DeepLink`](https://dev.appsflyer.com/hc/docs/deeplink) object. 
The `DeepLink `object contains the deep linking information and helper functions to easily retrieve values from well-known OneLink keys, for example, [`getDeepLinkValue()`](https://dev.appsflyer.com/hc/docs/deeplink#getdeeplinkvalue).
7. Use [`getDeepLinkValue()`](https://dev.appsflyer.com/hc/docs/deeplink#getdeeplinkvalue) to retrieve the `deep_link_value`. 
8. Use [`getStringValue("deep_link_sub1")`](https://dev.appsflyer.com/hc/docs/deeplink#getstringvalue) to retrieve `deep_link_sub1`. Do the same for `deep_link_sub2-10` parameters, changing the string value as required.
9. Once `deep_link_value` and `deep_link_sub1-10` are retrieved, pass them to an in-app router and use them to personalize the user experience.

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "`onDeepLinking` is not called when the app is running in the background and Application LaunchMode is not standard. \n\nTo correct this, call setIntent(intent) method to set the intent value inside the overridden method onNewIntent if the application is using a non-standard LaunchMode.\n```java\nimport android.content.Intent;\n...\n...\n...\n@Override\nprotected void onNewIntent(Intent intent) \n{ \n  super.onNewIntent(intent);     \n  setIntent(intent);\n}\n```"
}
[/block]
### Code example
[block:code]
{
  "codes": [
    {
      "code": " appsflyer.subscribeForDeepLink(new DeepLinkListener(){\n            @Override\n            public void onDeepLinking(@NonNull DeepLinkResult deepLinkResult) {\n                DeepLinkResult.Status dlStatus = deepLinkResult.getStatus();\n                if (dlStatus == DeepLinkResult.Status.FOUND) {\n                    Log.d(LOG_TAG, \"Deep link found\");\n                } else if (dlStatus == DeepLinkResult.Status.NOT_FOUND) {\n                    Log.d(LOG_TAG, \"Deep link not found\");\n                    return;\n                } else {\n                    // dlStatus == DeepLinkResult.Status.ERROR\n                    DeepLinkResult.Error dlError = deepLinkResult.getError();\n                    Log.d(LOG_TAG, \"There was an error getting Deep Link data: \" + dlError.toString());\n                    return;\n                }\n                DeepLink deepLinkObj = deepLinkResult.getDeepLink();\n                try {\n                    Log.d(LOG_TAG, \"The DeepLink data is: \" + deepLinkObj.toString());\n                } catch (Exception e) {\n                    Log.d(LOG_TAG, \"DeepLink data came back null\");\n                    return;\n                }\n                // An example for using is_deferred\n                if (deepLinkObj.isDeferred()) {\n                    Log.d(LOG_TAG, \"This is a deferred deep link\");\n                } else {\n                    Log.d(LOG_TAG, \"This is a direct deep link\");\n                }\n                \n                // ** Next if statement is optional **\n                // Our sample app's user-invite carries the referrerID in deep_link_sub2\n                // See the user-invite section in FruitActivity.java\n                if (dlData.has(\"deep_link_sub2\")){\n                    referrerId = deepLinkObj.getStringValue(\"deep_link_sub2\");\n                    Log.d(LOG_TAG, \"The referrerID is: \" + referrerId);\n                } else {\n                    Log.d(LOG_TAG, \"deep_link_sub2/Referrer ID not found\");\n                }\n                // An example for using a generic getter\n                String fruitName = \"\";\n                try {\n                    fruitName = deepLinkObj.getDeepLinkValue();\n                    Log.d(LOG_TAG, \"The DeepLink will route to: \" + fruitName);\n                } catch (Exception e) {\n                    Log.d(LOG_TAG, \"Custom param fruit_name was not found in DeepLink data\");\n                    return;\n                }\n                goToFruit(fruitName, deepLinkObj);\n            }\n        });",
      "language": "java"
    },
    {
      "code": "AppsFlyerLib.getInstance().subscribeForDeepLink(object : DeepLinkListener{\n            override fun onDeepLinking(deepLinkResult: DeepLinkResult) {\n                when (deepLinkResult.status) {\n                    DeepLinkResult.Status.FOUND -> {\n                        Log.d(\n                            LOG_TAG,\"Deep link found\"\n                        )\n                    }\n                    DeepLinkResult.Status.NOT_FOUND -> {\n                        Log.d(\n                            LOG_TAG,\"Deep link not found\"\n                        )\n                        return\n                    }\n                    else -> {\n                        // dlStatus == DeepLinkResult.Status.ERROR\n                        val dlError = deepLinkResult.error\n                        Log.d(\n                            LOG_TAG,\"There was an error getting Deep Link data: $dlError\"\n                        )\n                        return\n                    }\n                }\n                var deepLinkObj: DeepLink = deepLinkResult.deepLink\n                try {\n                    Log.d(\n                        LOG_TAG,\"The DeepLink data is: $deepLinkObj\"\n                    )\n                } catch (e: Exception) {\n                    Log.d(\n                        LOG_TAG,\"DeepLink data came back null\"\n                    )\n                    return\n                }\n\n                // An example for using is_deferred\n                if (deepLinkObj.isDeferred == true) {\n                    Log.d(LOG_TAG, \"This is a deferred deep link\");\n                } else {\n                    Log.d(LOG_TAG, \"This is a direct deep link\");\n                }\n\n                try {\n                    val fruitName = deepLinkObj.deepLinkValue\n                    Log.d(LOG_TAG, \"The DeepLink will route to: $fruitName\")\n                } catch (e:Exception) {\n                    Log.d(LOG_TAG, \"There's been an error: $e\");\n                    return;\n                }\n            }\n        })",
      "language": "kotlin"
    }
  ]
}
[/block]
⇲ Github links: [Java][udl_java]

[udl_java]: https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/master/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/AppsflyerBasicApp.java#L31-L70