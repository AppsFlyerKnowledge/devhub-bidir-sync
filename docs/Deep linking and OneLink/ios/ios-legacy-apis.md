---
title: Legacy APIs
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
  "title": "This page has a newer [version](https://dev.appsflyer.com/hc/docs/dl_ios_gcd_legacy)"
}
[/block]
**At a glance**: Legacy methods for deep linking and deferred deep linking. **Note: We recommend using the non-legacy [UDL method](https://dev.appsflyer.com/hc/docs/unified-deep-linking-udl-1).**

Deep linking enables marketers to create links that send existing app users to a specific app experience (for example, a specific page in the app). Deferred deep linking directs new users without your app, first to the app store to download your app, and then to a specific in-app experience.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/236078e-ddl_iOS.png",
        "ddl_iOS.png",
        1494,
        663,
        "#d1d2cc"
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Deep linking"
}
[/block]
###Overview

Deep linking directs mobile users into a specific activity or content in an app.

This in-app routing to a specific activity in the app is possible due to the `deep_link_value` that is passed to the app when the OS opens the app and the `onAppOpenAttribution` method is called. AppsFlyer's OneLink ensures that the correct value is passed along with the user's click, thus personalizing the user’s app experience.

**Only the `deep_link_value` is required for deep linking. However, other parameters and values (such as custom attribution parameters) can also be added to the link and returned by the SDK as deep linking data. **

The marketer and developer must coordinate regarding desired app behavior and `deep_link_value`. The marketer uses the parameters to create deep links, and the developer customizes the behavior of the app based on the value received.

The AppsFlyer SDK returns the parameters from the link that the user clicked, and it is the developer's responsibility to make sure the parameters are handled correctly in the app, for both in-app routing, and personalizing data in the link.

**The deep linking flow works as follows**:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2407f56-Ios_DL.png",
        "Ios_DL.png",
        1920,
        2160,
        "#d8eaf7"
      ]
    }
  ]
}
[/block]
1. User clicks the OneLink short URL.
2. iOS reads the app’s Associated Domains Entitlements.
3. iOS opens the app.
4. AppsFlyer SDK is triggered inside the app.
5.  AppsFlyer SDK retrieves the OneLink data.
   * In a short URL, the data is retrieved from the short URL resolver API in AppsFlyer's servers.
   * In a long URL, the data is retrieved directly from the long URL.
6. AppsFlyer SDK triggers `onAppOpenAttribution()` with the retrieved parameters and cached attribution parameters (e.g.`install_time`).
7. Asynchronously, `onConversionDataSuccess()` is called, holding the full cached attribution data. (You can exit this function by checking if `is_first_launch` is `true`.)
8. `onAppOpenAttribution()` uses the `attributionData` map to dispatch other activities in the app and pass relevant data.
   * This creates the personalized experience for the user, which is the main goal of OneLink.

###Procedures

To implement the onAppOpenAttribution method and set up the parameter behaviors, the following action checklist of procedures must be completed. 

[block:parameters]
{
  "data": {
    "h-0": "Procedure checklist",
    "0-0": "1. [Deciding app behavior and `deep_link_value` ](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#deciding-app-behavior)(and other parameter names and values) - with the marketer",
    "1-0": "2. [Planning method input, i.e. `deep_link_value` ](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#planning-method-input)(and other parameter names and values) - with the marketer",
    "2-0": "3. [Implementing the `onAppOpenAttribution()` logic](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#implementing-onappopenattribution-logic)",
    "3-0": "4. [Implementing the `onAppOpenAttributionFailure()` logic](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#implementing-onappopenattributionfailure-logic)"
  },
  "cols": 1,
  "rows": 4
}
[/block]
#### Deciding app behavior

**To decide what the app behavior is when the link is clicked**: 

Get from the marketer: The expected behavior of the link when it is clicked.

#### Planning method input

When a OneLink is clicked and the user has the app installed on their device, the `onAppOpenAttribution` method is called by the AppsFlyer SDK. This is referred to as a retargeting re-engagement.

The `onAppOpenAttribution` method gets variables as an input like this: 
[block:code]
{
  "codes": [
    {
      "code": "attributionData: [AnyHashable: Any]",
      "language": "swift"
    }
  ]
}
[/block]
The input data structure is described [here](https://dev.appsflyer.com/docs/direct-deep-linking-1).

The marketer and developers need to plan the `deep_link_value` (and possible other parameters and values) together based on the desired app behavior when the link is clicked.

**To plan the `deep_link_value`, and other parameter names and values based on the expected link behavior**:

1. Tell the marketer what parameters and values are needed in order to implement the desired app behavior.
2. Decide on naming conventions for the `deep_link_value` and other parameters and values.
**Note**: Custom parameters will not appear in raw data collected in AppsFlyer.
[block:callout]
{
  "type": "info",
  "title": "Tip",
  "body": "The marketer and developers need to decide together on the best long term system for the `deep_link_value` (and any other parameters/values) to minimize additional app updates.\n\nThe `deep_link_value` can be based on a SKU, post ID, path, or anything else. We strongly recommend agreeing on a system that allows for you to enter dynamic values on your chosen parameter, so you can generate many different deep links that go to different content within the app, without any further changes to the app code by the developers. \n\nSee the following URL examples. The `deep_link_value` of a fruit type was chosen by the marketer and developer together. And the developers made the values dynamic, so the marketer could enter any fruit without the need for further work by the dev team. \n\nhttps://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month**&deep_link_value=apples**...\nhttps://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month**&deep_link_value=bananas**...\nhttps://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month**&deep_link_value=peaches**..."
}
[/block]
#### Implementing onAppOpenAttribution() logic

The deep link opens the `onAppOpenAttribution` method in the AppDelegate. The `deep_link_value` and other parameters in the method input are used to implement the specific user experience when the application is opened.

**To implement the logic**: 
1. Implement the logic based on the chosen parameters and values. See the following code example.
2. Implement application function to allow:
     * Universal Links support with [`continue`](https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyerlib#continue)
     * URI scheme support with [`handleOpen`](https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyerlib#handleopen)
3. Once completed, send confirmation to the marketer that the app behaves accordingly.

[block:callout]
{
  "type": "info",
  "title": "Sample Code"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "func onAppOpenAttribution(_ attributionData: [AnyHashable: Any]) {\n    //Handle Deep Link Data\n    print(\"onAppOpenAttribution data:\")\n    for (key, value) in attributionData {\n        print(key, \":\",value)\n    }\n    walkToSceneWithParams(params: attributionData)\n}\n\n// User logic\nfileprivate func walkToSceneWithParams(params: [AnyHashable:Any]) {\n    let storyBoard: UIStoryboard = UIStoryboard(name: \"Main\", bundle: nil)\n    UIApplication.shared.windows.first?.rootViewController?.dismiss(animated: true, completion: nil)\n\n    var fruitNameStr = \"\"\n\n    if let thisFruitName = params[\"deep_link_value\"] as? String {\n        fruitNameStr = thisFruitName\n    } else if let linkParam = params[\"link\"] as? String {\n        guard let url = URLComponents(string: linkParam) else {\n            print(\"Could not extract query params from link\")\n            return\n        }\n        if let thisFruitName = url.queryItems?.first(where: { $0.name == \"deep_link_value\" })?.value {\n            fruitNameStr = thisFruitName\n        }\n    }\n\n    let destVC = fruitNameStr + \"_vc\"\n    if let newVC = storyBoard.instantiateVC(withIdentifier: destVC) {\n\n        print(\"AppsFlyer routing to section: \\(destVC)\")\n        newVC.attributionData = params\n\n        UIApplication.shared.windows.first?.rootViewController?.present(newVC, animated: true, completion: nil)\n    } else {\n        print(\"AppsFlyer: could not find section: \\(destVC)\")\n    }\n}",
      "language": "swift"
    }
  ]
}
[/block]
⇲ Github links: [Swift][oaoa_swift]

[oaoa_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/07f6d6d4b6897756942787774a8adb69c26838a5/swift/basic_app/basic_app/AppDelegate.swift#L151-L159


#### Implementing onAppOpenAttributionFailure() logic

The `onAttributionFailure` method is called whenever the call to `onAppOpenAttribution` fails. The function should report the error and create an expected experience for the user.

**To implement the onAppOpenAttributionFailure method**:

Enter the following code:

[block:code]
{
  "codes": [
    {
      "code": "func onAppOpenAttributionFailure(_ error: Error) {\n    print(\"\\(error)\")\n}",
      "language": "swift"
    }
  ]
}
[/block]
⇲ Github links: [Swift][oafailure_swift]

[oafailure_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/07f6d6d4b6897756942787774a8adb69c26838a5/swift/basic_app/basic_app/AppDelegate.swift#L161-L163
[block:api-header]
{
  "title": "Deferred deep linking"
}
[/block]

[block:callout]
{
  "type": "danger",
  "body": "Deferred deep linking using the legacy method of onConversionDataSuccess may not work for iOS 14.5+, since it requires attribution data that may not be available due to privacy protection.\nWe recommend using [unified deep linking (UDL)](https://dev.appsflyer.com/hc/docs/unified-deep-linking-udl-1). UDL conforms to the iOS 14.5+ privacy standards and only returns parameters relevant to deep linking and deferred deep linking: `deep_link_value` and `deep_link_sub1`. Attribution parameters (such as `media_source`, `campaign`, `af_sub1-5`, etc.), return null and can’t be used for deep linking purposes.\n\n[Learn more](https://content.appsflyer.com/ios-14-hub/deep-linking-deferred-deep-linking/)",
  "title": "Important!"
}
[/block]
### Overview

Deferred deep linking directs new users first to the correct app store to install the app, and then, after the first open, to a specific app experience (for example, a specific page in the app).

When the user first launches the app, the `onConversionDataSuccess` callback function receives both the conversion data of the new user, and OneLink data. The OneLink data makes in-app routing possible due to the `deep_link_value` that is passed to the app when the OS opens the app. 

Only the `deep_link_value` is required for deep linking. However, other parameters and values (such as custom attribution parameters) can also be added to the link and returned by the SDK as deep linking data. The AppsFlyer OneLink ensures that the correct parameters are passed along with the user's click, thus personalizing the user’s app experience.

The marketer and developer must coordinate regarding desired app behavior and `deep_link_value`. The marketer uses the parameters to create deep links, and the developer customizes the behavior of the app based on the value received.

It is the developer's responsibility to make sure the parameters are handled correctly in the app, for both in-app routing, and personalizing data in the link.

**The deep linking flow works as follows**:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4db3218-Ios_DDL.png",
        "Ios_DDL.png",
        1920,
        2160,
        "#d8ebf8"
      ]
    }
  ]
}
[/block]
1. User clicks the OneLink on a device on which the app is not installed.
2. AppsFlyer registers the click and redirects the user to the correct app store or landing page.
3. The user installs the application and launches it.
4. iOS verifies the Universal Link ownership using the *Apple App Site Association* file hosted by AppsFlyer in the location: `https://myapp.onelink.me/.well-known/apple-app-site-association`
5. AppsFlyer SDK is initialized and the install is attributed in the AppsFlyer servers.
6. The SDK triggers the `onConversionDataSuccess` method. The function receives input that includes both the `deep_link_value`, and the attribution data/parameters defined in the OneLink data.
7. The parameter `is_first_launch` has the value `true`, which signals the deferred deep link flow. 
   The developer uses the data received in the onConversionDataSuccess function to create a personalized experience for the user for the application’s first launch.

### Procedures

To implement the `onConversionDataSuccess` method and set up the parameter behaviors, the following action checklist of procedures need to be completed. 
[block:parameters]
{
  "data": {
    "0-0": "1. [Deciding app behavior on first launch, and `deep_link_value`](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#deciding-app-behavior-on-first-launch) (and other parameter names and values) - with the marketer",
    "h-0": "Procedure checklist",
    "1-0": "2. [Planning method input, i.e. `deep_link_value` ](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#planning-method-input-1)(and other parameter names and values) - with the marketer",
    "2-0": "3. [Implementing the `onConversionDataSuccess()` logic](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#implementing-onconversiondatasuccess-logic)",
    "3-0": "4. [Implementing the `onConversionDataFail()` logic](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#implementing-onconversiondatafailure-logic)"
  },
  "cols": 1,
  "rows": 4
}
[/block]
#### Deciding app behavior on first launch

**To decide app behavior on first launch**: 

Get from the marketer: The expected behavior of the link when it is clicked and the app opens for the first time.

#### Planning method input

For deferred deep linking, the `onConversionDataSuccess` method input must be planned and the input decided in the previous section (for deep linking) is made relevant for the first time the app is launched.

The `onConversionDataSuccess` method gets the `deep_link_value` and other variables as an input like this: 

[block:code]
{
  "codes": [
    {
      "code": "data: [AnyHashable: Any]",
      "language": "swift"
    }
  ]
}
[/block]
The map holds two kinds of data:
* [Attribution data](https://support.appsflyer.com/hc/en-us/articles/207447163#attribution-link-parameters)
* Data defined by the marketer in the link (`deep_link_value` and other parameters and values)
  Other parameters can be either:
   * AppsFlyer official parameters.
   * Custom parameters and values chosen by the marketer and developer.
   * The input data structure is described [here](https://dev.appsflyer.com/docs/direct-deep-linking-1).

The marketer and developers need to plan the `deep_link_value` (and other possible parameters and values) together based on the desired app behavior when the link is clicked.

**To plan the `deep_link_value`, and other parameter names and values based on the expected link behavior**:

1. Tell the marketer what parameters and values are needed in order to implement the desired app behavior.
2. Decide on naming conventions for the `deep_link_value` and other parameters and values.
    **Note**: 
    * Custom parameters will not appear in raw data collected in AppsFlyer.
    * Conversion data will not return a custom parameter named "name, " with a lowercase "n".

#### Implementing onConversionDataSuccess() logic

When the app is opened for the first time, the `onConversionDataSuccess` method is triggered in the main activity. The `deep_link_value` and other parameters in the method input are used to implement the specific user experience when the app is first launched.

**To implement the logic**: 
1. Implement the logic based on the chosen parameters and values. See the following code example.
2. Once completed, send confirmation to the marketer that the app behaves accordingly.


[block:callout]
{
  "type": "info",
  "title": "Sample code"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "// Handle Organic/Non-organic installation\nfunc onConversionDataSuccess(_ data: [AnyHashable: Any]) {\n\n    print(\"onConversionDataSuccess data:\")\n    for (key, value) in data {\n        print(key, \":\", value)\n    }\n\n    if let status = data[\"af_status\"] as? String {\n        if (status == \"Non-organic\") {\n            if let sourceID = data[\"media_source\"],\n                let campaign = data[\"campaign\"] {\n                print(\"This is a Non-Organic install. Media source: \\(sourceID)  Campaign: \\(campaign)\")\n            }\n        } else {\n            print(\"This is an organic install.\")\n        }\n        if let is_first_launch = data[\"is_first_launch\"] as? Bool,\n            is_first_launch {\n            print(\"First Launch\")\n            if let fruit_name = data[\"deep_link_value\"]\n            {\n                // The key 'deep_link_value' exists only in OneLink originated installs\n                print(\"deferred deep-linking to \\(fruit_name)\")\n                walkToSceneWithParams(params: data)\n            }\n            else {\n                print(\"Install from a non-owned media\")\n            }\n        } else {\n            print(\"Not First Launch\")\n        }\n    }\n}",
      "language": "swift"
    }
  ]
}
[/block]
⇲ Github links: [Swift][ocds_swift]

[ocds_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/07f6d6d4b6897756942787774a8adb69c26838a5/swift/basic_app/basic_app/AppDelegate.swift#L113-L145

####Implementing onConversionDataFailure() logic

The `onConversionDataFailure` method is called whenever the call to `onConversionDataSuccess` fails. The function should report the error and create an expected experience for the user.

**To implement the onConversionDataFailure method**:

Enter the following code:

[block:code]
{
  "codes": [
    {
      "code": "func onConversionDataFail(_ error: Error) {\n    print(\"\\(error)\")\n}",
      "language": "swift"
    }
  ]
}
[/block]
⇲ Github links: [Swift][ocdf_swift]

[ocdf_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/07f6d6d4b6897756942787774a8adb69c26838a5/swift/basic_app/basic_app/AppDelegate.swift#L147-L149


[block:api-header]
{
  "title": "Full code example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "// For Swift version < 4.2 replace function signature with the commented out code\n// func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([Any]?) -> Void) -> Bool { // this line for Swift < 4.2\nfunc application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool {\n  AppsFlyerLib.shared().continue(userActivity, restorationHandler: nil)\n  return true\n}\n\n// Open URI-scheme for iOS 9 and above\nfunc application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {\n  AppsFlyerLib.shared().handleOpen(url, options: options)\n  return true\n}\n\nextension AppDelegate: AppsFlyerLibDelegate {\n     \n    // Handle Organic/Non-organic installation\n    func onConversionDataSuccess(_ data: [AnyHashable: Any]) {\n        \n        print(\"onConversionDataSuccess data:\")\n        for (key, value) in data {\n            print(key, \":\", value)\n        }\n        \n        if let status = data[\"af_status\"] as? String {\n            if (status == \"Non-organic\") {\n                if let sourceID = data[\"media_source\"],\n                    let campaign = data[\"campaign\"] {\n                    print(\"This is a Non-Organic install. Media source: \\(sourceID)  Campaign: \\(campaign)\")\n                }\n            } else {\n                print(\"This is an organic install.\")\n            }\n            if let is_first_launch = data[\"is_first_launch\"] as? Bool,\n                is_first_launch {\n                print(\"First Launch\")\n            } else {\n                print(\"Not First Launch\")\n            }\n        }\n    }\n    \n    func onConversionDataFail(_ error: Error) {\n        print(\"\\(error)\")\n    }\n   \n    // Handle Deeplink\n    func onAppOpenAttribution(_ attributionData: [AnyHashable: Any]) {\n        //Handle Deep Link Data\n        print(\"onAppOpenAttribution data:\")\n        for (key, value) in attributionData {\n            print(key, \":\",value)\n        }\n        walkToSceneWithParams(params: attributionData)\n    }\n    \n    func onAppOpenAttributionFailure(_ error: Error) {\n        print(\"\\(error)\")\n    }\n}\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Common data structures"
}
[/block]
This section contains information on data structure and possible parameters that OneLink can pass as an input.

### Input parameters

The following table lists the possible parameters OneLink can pass as an input.

The  input map holds two kinds of data:
* [Attribution data](https://support.appsflyer.com/hc/en-us/articles/207447163#attribution-link-parameters)
* Data defined by the marketer in the link (parameters and values)
Parameters can be either:
   * AppsFlyer official parameters.
   * Custom parameters and values chosen by the marketer and developer.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "* The following table is relevant for AppsFlyer **SDK 5.4.1 and above**. Parameters may not be present or renamed in earlier SDK versions\n* The parameters **not marked as deprecated ** are relevant for all OneLink types:\n   * Short URL\n   * Long URL\n   * All OS's links:\n      * Android App Link\n      * Universal Links\n      * URL schemes (both iOS and Android)"
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Parameter name",
    "h-1": "Type",
    "h-2": "Description",
    "h-3": "Remarks",
    "0-0": "af_dp",
    "0-1": "String",
    "1-1": "String",
    "2-1": "String",
    "3-1": "String",
    "4-1": "String",
    "5-1": "String",
    "6-1": "String",
    "7-1": "String",
    "8-1": "String",
    "9-1": "String",
    "10-1": "String",
    "11-1": "Boolean",
    "12-1": "String",
    "13-1": "Boolean",
    "14-1": "String",
    "15-1": "String",
    "16-1": "String",
    "17-1": "String",
    "18-1": "String",
    "19-1": "String",
    "20-1": "String",
    "21-1": "String",
    "22-1": "String",
    "23-1": "Boolean",
    "0-2": "URI scheme URL.",
    "0-3": "Fallback to App Link. \nFor example:  afbasicapp://mainactivity",
    "1-0": "link",
    "1-2": "The full link that was used to perform the deep link.",
    "1-3": "Example: https://onelink-basic-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month",
    "2-0": "deep_link_value",
    "2-2": "The value name for the specific in-app content that users will be directed to.",
    "3-0": "pid (media source)",
    "3-2": "OneLink's media source, e.g. email, SMS, social media.",
    "4-0": "install_time",
    "4-2": "Time of the first app launch.",
    "4-3": "**Deprecated**\nExample: 2020-05-06 13:51:19",
    "5-0": "scheme",
    "5-2": "The first word in the URL, that identifies the protocol to be used to access the resource on the Internet. For example: **mygreatapp**://mainactivity or **https**://killerapp.onelink.me/coolactivity/H7JK",
    "5-3": "**Deprecated**\nNever use `http` or `https` for URI schemes",
    "6-0": "host",
    "7-0": "path",
    "8-0": "shortlink",
    "9-0": "af_web_id",
    "10-0": "af_status",
    "11-0": "af_deeplink",
    "12-0": "c (campaign)",
    "13-0": "is_retargeting",
    "14-0": "af_ios_url",
    "15-0": "af_android_url",
    "16-0": "af_sub[1-5]",
    "17-0": "af_adset",
    "18-0": "af_channel",
    "19-0": "ad_adname",
    "20-0": "af_cost_currency",
    "21-0": "af_cost_value",
    "22-0": "af_click_lookback",
    "23-0": "af_force_deeplink",
    "6-2": "Identifies the host that holds the resource. For example: mygreatapp://**mainactivity** or \nhttps://**killerapp.onelink.me**/coolactivity/H7JK",
    "6-3": "**Deprecated**",
    "7-2": "The specific resource in the host that the web client wants to access. For example:  https://killerapp.onelink.me/coolactivity/**H7JK**",
    "7-3": "**Deprecated**\nNot relevant for URI schemes",
    "8-2": "A shortened URL, with significantly fewer characters than the original link. For example: https://killerapp.onelink.me/coolactivity/H7JK/**checkitout**",
    "9-2": "Token for People Based Attribution.",
    "10-2": "**Deprecated**",
    "11-2": "**Deprecated**",
    "10-3": "Passed **only** in URI scenario",
    "11-3": "Passed **only** in URI scenario",
    "12-2": "Name of the marketing campaign.",
    "12-3": "The value set by the marketer in the AppsFlyer dashboard.",
    "13-2": "Marks the link as part of a retargeting campaign.",
    "13-3": "The value is set by the marketer.",
    "14-2": "Fallback URL when deep linking fails on an iOS device.",
    "14-3": "Passed to Android devices as well, even when not relevant",
    "15-2": "Fallback URL when deep-linking fails on an Android device.",
    "16-2": "Optional custom parameter defined by the advertiser.",
    "16-3": "Values set by the marketer in the AppsFlyer dashboard.\nRecommended for passing parameters relevant for in-app routing.",
    "17-2": "Adset is an intermediate level in the hierarchy between campaign and ad.",
    "17-3": "Value set by the marketer in AppsFlyer's dashboard",
    "18-2": "The media source channel through which the ads are distributed. For example:  UAC_Search, UAC_Display, Instagram, Facebook Audience Network etc.",
    "18-3": "Value set by the marketer in AppsFlyer's dashboard",
    "19-2": "Ad name provided by the marketer/publisher.",
    "19-3": "Value set by the marketer in AppsFlyer's dashboard",
    "20-2": "3 letter currency code compliant with [ISO-4217](https://support.appsflyer.com/hc/en-us/articles/207040526-Ad-cost-measurement-guide#cost-aggregation-methods). For example, USD, ZAR, EUR\n[Default]: USD",
    "20-3": "Value set by the marketer in AppsFlyer's dashboard",
    "21-2": "Cost value in using cost currency.",
    "22-2": "Configurable number of days for the lookback click attribution period.",
    "23-2": "Force deep linking into the activity specified in af_dp value.",
    "21-3": "Value set by the marketer in AppsFlyer's dashboard",
    "22-3": "Value set by the marketer in AppsFlyer's dashboard",
    "23-3": "Relevant for iOS only.\nValue is passed to Android, even when not relevant."
  },
  "cols": 4,
  "rows": 24
}
[/block]
### iOS sample payloads

See the following sample payloads for Universal Links, URI schemes, and deferred deep linking. The samples contain a full payload, relevant for when all parameters in the Onelink custom link setup page  contain data.

**Note**: Payloads return as a map. However, for clarity, the sample payloads that follow are displayed in JSON form. 

#### Universal Links

Input to `onAppOpenAttribution(_ attributionData: [AnyHashable: Any])`
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"af_ad\": \"my_adname\",\n   \"af_adset\": \"my_adset\",\n   \"af_android_url\": \"https://isitchristmas.com/\",\n   \"af_channel\": \"my_channel\",\n   \"af_click_lookback\": \"20d\",\n   \"af_cost_currency\": \"USD\",\n   \"af_cost_value\": 6,\n   \"af_dp\": \"afbasicapp://mainactivity\",\n   \"af_ios_url\": \"https://isitchristmas.com/\",\n   \"af_sub1\": \"my_sub1\",\n   \"af_sub2\": \"my_sub2\",\n   \"c\": \"fruit_of_the_month\",\n   \"campaign\": \"fruit_of_the_month\",\n   \"fruit_amount\": 26,\n   \"fruit_name\": \"apples\",\n   \"is_retargeting\": true,\n   \"link\": \"https://onelink-basic-app.onelink.me/H5hv/6d66214a\",\n   \"media_source\": \"Email\",\n   \"pid\": \"Email\"\n}",
      "language": "json",
      "name": "Short link"
    },
    {
      "code": "{\n   \"path\": \"/H5hv\",\n   \"af_android_url\": \"https://my_android_lp.com\",\n   \"af_channel\": \"my_channel\",\n   \"host\": \"onelink-basic-app.onelink.me\",\n   \"af_adset\": \"my_adset\",\n   \"pid\": \"Email\",\n   \"scheme\": \"https\",\n   \"af_dp\": \"afbasicapp://mainactivity\",\n   \"af_sub1\": \"my_sub1\",\n   \"fruit_name\": \"apples\",\n   \"af_ad\": \"my_adname\",\n   \"af_click_lookback\": \"20d\",\n   \"fruit_amount\": 16,\n   \"af_sub2\": \"my_sub2\",\n   \"link\": \"https://onelink-basic-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&af_channel=my_channel&af_adset=my_adset&af_ad=my_adname&af_sub1=my_sub1&af_sub2=my_sub2&fruit_name=apples&fruit_amount=16&af_cost_currency=USD&af_cost_value=6&af_click_lookback=20d&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_android_url=https%3A%2F%2Fmy_android_lp.com\",\n   \"af_cost_currency\": \"USD\",\n   \"c\": \"fruit_of_the_month\",\n   \"af_ios_url\": \"https://my_ios_lp.com\",\n   \"af_cost_value\": 6\n}",
      "language": "json",
      "name": "Long link"
    }
  ]
}
[/block]
#### URI scheme

Input to `onAppOpenAttribution(_ attributionData: [AnyHashable: Any])`
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"af_click_lookback \": \"25d\",\n\t\"af_sub1 \": \"my_sub1\",\n\t\"shortlink \": \"9270d092\",\n\t\"af_deeplink \": true,\n\t\"media_source \": \"Email\",\n\t\"campaign \": \"my_campaign\",\n\t\"af_cost_currency \": \"NZD\",\n\t\"host \": \"mainactivity\",\n\t\"af_ios_url \": \"https://my_ios_lp.com\",\n\t\"scheme \": \"afbasicapp\",\n\t\"path \": \"\",\n\t\"af_cost_value \": 5,\n\t\"af_adset \": \"my_adset\",\n\t\"af_ad \": \"my_adname\",\n\t\"af_android_url \": \"https://my_android_lp.com\",\n\t\"af_sub2 \": \"my_sub2\",\n\t\"af_force_deeplink \": true,\n\t\"fruit_amount \": 15,\n\t\"af_dp \": \"afbasicapp://mainactivity\",\n\t\"link \": \"afbasicapp://mainactivity?af_ad=my_adname&af_adset=my_adset&af_android_url=https%3A%2F%2Fmy_android_lp.com&af_channel=my_channel&af_click_lookback=25d&af_cost_currency=NZD&af_cost_value=5&af_deeplink=true&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_force_deeplink=true&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_sub1=my_sub1&af_sub2=my_sub2&af_web_id=56441f02-377b-47c6-9648-7a7f88268130-o&campaign=my_campaign&fruit_amount=15&fruit_name=apples&is_retargeting=true&media_source=Email&shortlink=9270d092\",\n\t\"af_channel \": \"my_channel\",\n\t\"is_retargeting \": true,\n\t\"af_web_id \": \"56441f02-377b-47c6-9648-7a7f88268130-o\",\n\t\"fruit_name \": \"apples\"\n}",
      "language": "json",
      "name": "Short link"
    },
    {
      "code": "{\n\t\"af_ad \": \"my_adname\",\n\t\"fruit_name \": \"apples\",\n\t\"host \": \"mainactivity\",\n\t\"af_channel \": \"my_channel\",\n\t\"link \": \"afbasicapp://mainactivity?af_ad=my_adname&af_adset=my_adset&af_android_url=https%3A%2F%2Fmy_android_lp.com&af_channel=my_channel&af_click_lookback=25d&af_cost_currency=NZD&af_cost_value=5&af_deeplink=true&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_force_deeplink=true&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_sub1=my_sub1&af_sub2=my_sub2&af_web_id=56441f02-377b-47c6-9648-7a7f88268130-o&campaign=my_campaign&fruit_amount=15&fruit_name=apples&is_retargeting=true&media_source=Email\",\n\t\"af_deeplink \": true,\n\t\"campaign \": \"my_campaign\",\n\t\"af_sub1 \": \"my_sub1\",\n\t\"af_click_lookback \": \"25d\",\n\t\"af_web_id \": \"56441f02-377b-47c6-9648-7a7f88268130-o\",\n\t\"path \": \"\",\n\t\"af_sub2 \": \"my_sub2\",\n\t\"af_ios_url \": \"https://my_ios_lp.com\",\n\t\"af_cost_value \": 5,\n\t\"fruit_amount \": 15,\n\t\"is_retargeting \": true,\n\t\"scheme \": \"afbasicapp\",\n\t\"af_force_deeplink \": true,\n\t\"af_adset \": \"my_adset\",\n\t\"media_source \": \"Email\",\n\t\"af_cost_currency \": \"NZD\",\n\t\"af_dp \": \"afbasicapp://mainactivity\",\n\t\"af_android_url \": \"https://my_android_lp.com\"\n}",
      "language": "json",
      "name": "Long link"
    }
  ]
}
[/block]
#### Deferred deep linking

Input to `onConversionDataSuccess(_ data: [AnyHashable: Any])`

[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"adgroup\": null,\n\t\"adgroup_id\": null,\n\t\"adset\": null,\n\t\"adset_id\": null,\n\t\"af_ad\": \"my_adname\",\n\t\"af_adset\": \"my_adset\",\n\t\"af_android_url\": \"https://isitchristmas.com/\",\n\t\"af_channel\": \"my_channel\",\n\t\"af_click_lookback\": \"20d\",\n\t\"af_cost_currency\": \"USD\",\n\t\"af_cost_value\": 6,\n\t\"af_cpi\": null,\n\t\"af_dp\": \"afbasicapp://mainactivity\",\n\t\"af_ios_url\": \"https://isitchristmas.com/\",\n\t\"af_siteid\": null,\n\t\"af_status\": \"Non-organic\",\n\t\"af_sub1\": \"my_sub1\",\n\t\"af_sub2\": \"my_sub2\",\n\t\"af_sub3\": null,\n\t\"af_sub4\": null,\n\t\"af_sub5\": null,\n\t\"agency\": null,\n\t\"campaign\": \"fruit_of_the_month \",\n\t\"campaign_id\": null,\n\t\"click_time\": \"2020-08-12 15:08:00.770\",\n\t\"cost_cents_USD\": 600,\n\t\"engmnt_source\": null,\n\t\"esp_name\": null,\n\t\"fruit_amount\": 26,\n\t\"fruit_name\": \"apples\",\n\t\"http_referrer\": null,\n\t\"install_time\": \"2020-08-12 15:08:33.335\",\n\t\"is_branded_link\": null,\n\t\"is_first_launch\": 1,\n\t\"is_retargeting\": true,\n\t\"is_universal_link\": null,\n\t\"iscache\": 1,\n\t\"match_type\": \"probabilistic\",\n\t\"media_source\": \"Email\",\n\t\"orig_cost\": \"6.0\",\n\t\"redirect_response_data\": null,\n\t\"retargeting_conversion_type\": \"none\",\n\t\"shortlink\": \"6d66214a\"\n}",
      "language": "json",
      "name": "Short link"
    }
  ]
}
[/block]