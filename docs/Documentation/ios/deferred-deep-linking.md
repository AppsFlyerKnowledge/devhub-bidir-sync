---
title: Deferred deep linking
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
**At a glance**: Deferred deep linking setup enables the marketer to create links that will send new app users first to the correct app store to install the app, and then, after the first open, to a specific app experience (e.g. a specific page in the app).
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bab6c00-43463fd-direct_and_deferred_deep_linking.png",
        "43463fd-direct_and_deferred_deep_linking.png",
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
  "title": "Overview"
}
[/block]
Deferred deep linking directs new users first to the correct app store to install the app, and then, after the first open, to a specific app experience (e.g. a specific page in the app).

When the user first launches the app, the onConversionDataSuccess callback function receives both the conversion data of the new user, and OneLink data. The OneLink data makes in-app routing possible due to the additional parameters that are passed to the app when the OS opens the app. AppsFlyer's OneLink ensures that the correct parameters are passed along with the user's click, thus personalizing the user’s app experience. 

The marketer and developer must coordinate regarding desired app behavior and parameter names. The marketer will use the parameters to create the deferred deep links. 

**It is the developer's responsibility to make sure the parameters are handled correctly in the app** - for both in-app routing, and personalizing data in the link.

**The deep linking flow works as follows**:

1. User clicks the OneLink on a device on which the app is not installed.
2. AppsFlyer registers the click and redirects the user to the correct app store or landing page.
3. The user installs the application and launches it.
4. iOS verifies the Universal Link ownership using the *Apple App Site Association* file hosted by AppsFlyer in the location: `https://myapp.onelink.me/.well-known/apple-app-site-association`
5. AppsFlyer SDK is initialized and the install is attributed in AppsFlyers's servers.
6. The SDK triggers the onConversionDataSuccess method. The function receives input that includes both attribution data and the parameters defined in the OneLink data.
7. The parameter `is_first_launch` has the value `true`, that signals the deferred deep link flow. 
The developer uses the data received in the onConversionDataSuccess function to create a personalized experience for the user for the application’s first launch.
[block:api-header]
{
  "title": "Procedures"
}
[/block]
To implement the onConversionDataSuccess method and set up the parameter behaviors, the following action checklist of procedures need to be completed. 
[block:parameters]
{
  "data": {
    "h-0": "Procedure checklist",
    "0-0": "1. Deciding app behavior on first launch, and parameter names and values (with marketer)",
    "1-0": "2. Planning method input, i.e. parameter names and values (with marketer)",
    "2-0": "3. Implementing the logic",
    "3-0": "4. Implementing onConversionDataFail method method"
  },
  "cols": 1,
  "rows": 4
}
[/block]
### Deciding app behavior on first launch

**To decide app behavior on first launch**: 

Get from the marketer: The expected behavior of the link when it is clicked and the app opens for the first time.

### Planning method input

For deferred deep linking, the onConversionDataSuccess method input must be planned and the input decided in the previous section (for deep linking) is made relevant for the first time the app is launched.

The onConversionDataSuccess method gets variables as an input like this: 
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
[View the list of variables (parameters).  
](https://dev.appsflyer.com/docs/direct-deep-linking-1)

The map holds two kinds of data:
* [Attribution data](https://support.appsflyer.com/hc/en-us/articles/207447163#attribution-link-parameters)
* Data defined by the marketer in the link (parameters and values)
Parameters can be either:
   * AppsFlyer official parameters.
   * Custom parameters and values chosen by the marketer and developer.

The input data structure is described [here](https://dash.readme.io/project/afonelink/v0.1/docs/direct-deep-linking-1).

The marketer and developers need to plan the parameters and values together based on the desired app behavior when the link is clicked.

**To plan parameter names and values based on the expected link behavior**:

1. Tell the marketer what parameters and values are needed in order to implement the desired app behavior.
2. Decide on naming conventions for the parameters and values.

### Implementing the logic

When the app is opened for the first time, the onConversionDataSuccess method is triggered in the main activity. The parameters in the method input are used to implement the specific user experience when the app is first launched.

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
      "code": "// Handle Organic/Non-organic installation\nfunc onConversionDataSuccess(_ data: [AnyHashable: Any]) {\n\n    print(\"onConversionDataSuccess data:\")\n    for (key, value) in data {\n        print(key, \":\", value)\n    }\n\n    if let status = data[\"af_status\"] as? String {\n        if (status == \"Non-organic\") {\n            if let sourceID = data[\"media_source\"],\n                let campaign = data[\"campaign\"] {\n                print(\"This is a Non-Organic install. Media source: \\(sourceID)  Campaign: \\(campaign)\")\n            }\n        } else {\n            print(\"This is an organic install.\")\n        }\n        if let is_first_launch = data[\"is_first_launch\"] as? Bool,\n            is_first_launch {\n            print(\"First Launch\")\n            if let fruit_name = data[\"fruit_name\"]\n            {\n                // The key 'fruit_name' exists only in OneLink originated installs\n                print(\"deferred deep-linking to \\(fruit_name)\")\n                walkToSceneWithParams(params: data)\n            }\n            else {\n                print(\"Install from a non-owned media\")\n            }\n        } else {\n            print(\"Not First Launch\")\n        }\n    }\n}",
      "language": "swift",
      "gist": "a17489e6bc043f3c0742bdc0068600ba"
    }
  ]
}
[/block]
⇲ Github links: [Swift][ocds_swift]

[ocds_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/07f6d6d4b6897756942787774a8adb69c26838a5/swift/basic_app/basic_app/AppDelegate.swift#L113-L145

### Implementing onConversionDataFailure method

The onConversionDataFailure method is called whenever the call to onConversionDataSuccess fails. The function should report the error and create an expected experience for the user.

**To implement the onConversionDataFailure method**:

Enter the following code:
[block:code]
{
  "codes": [
    {
      "code": "func onConversionDataFail(_ error: Error) {\n    print(\"\\(error)\")\n}",
      "language": "swift",
      "gist": "36a283f9fae1ffe9b5f7d1977b2f65eb"
    }
  ]
}
[/block]
⇲ Github links: [Swift][ocdf_swift]

[ocdf_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/07f6d6d4b6897756942787774a8adb69c26838a5/swift/basic_app/basic_app/AppDelegate.swift#L147-L149