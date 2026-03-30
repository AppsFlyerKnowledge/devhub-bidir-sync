---
title: Direct deep linking
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
**At a glance**: Direct deep linking setup enables the marketer to create links that will send existing app users to a specific app experience (e.g. a specific page in the app). Deep link setup is also a prerequisite for deferred deep linking.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/88a6648-43463fd-direct_and_deferred_deep_linking.png",
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
Deep linking directs mobile users into a specific activity or content in an app.

This in-app routing to a specific activity in the app is possible due to the parameters that are passed to the app when the OS opens the app and the onAppOpenAttribution method is called. AppsFlyer's OneLink ensures that the correct parameters are passed along with the user's click, thus personalizing the user’s app experience. 

The marketer and developer must coordinate regarding desired app behavior and parameter names. The marketer will use the parameters to create deep links. 

**It is the developer's responsibility to make sure the parameters are handled correctly in the app** - for both in-app routing, and personalizing data in the link.

**The deep linking flow works as follows**:

1. User clicks the OneLink short URL.
2. iOS reads the app’s Associated Domains Entitlements.
3. iOS opens the app.
4. AppsFlyer SDK is triggered inside the app.
5.  The data is retrieved from the short URL resolver API in AppsFlyer's servers.
   * In a long URL, the data is retrieved directly from the long URL.
6. AppsFlyer SDK triggers `onAppOpenAttribution()` with the retrieved parameters and cached attribution parameters (e.g.`install_time`).
7. Asynchronously, `onConversionDataSuccess()` is called, holding the full cached attribution data. (You can exit this function by checking if `is_first_launch` is `true`.)
8. `onAppOpenAttribution()` uses the `attributionData` map to dispatch other activities in the app and pass relevant data.
   * This creates the personalized experience for the user, which is the main goal of OneLink.
[block:api-header]
{
  "title": "Procedures"
}
[/block]
To implement the onAppOpenAttribution method and set up the parameter behaviors, the following action checklist of procedures need to be completed. 
[block:parameters]
{
  "data": {
    "h-0": "Procedure checklist",
    "0-0": "1. Deciding app behavior, and parameter names and values (with marketer)",
    "1-0": "2. Planning method input, i.e. parameter names and values (with marketer)",
    "2-0": "3. Implementing the logic",
    "3-0": "4. Implementing the onAttributionFailure method"
  },
  "cols": 1,
  "rows": 4
}
[/block]
### Deciding app behavior

**To decide what the app behavior is when the link is clicked**: 

Get from the marketer: The expected behavior of the link when it is clicked.

### Planning method input

When a OneLink is clicked and the user has the app installed on their device, the onAppOpenAttribution method is called by the AppsFlyer SDK. This is referred to as a retargeting re-engagement.

The onAppOpenAttribution method gets variables as an input like this: 
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

The marketer and developers need to plan the parameters and values together based on the desired app behavior when the link is clicked.

**To plan parameter names and values based on the expected link behavior**:

1. Tell the marketer what parameters and values are needed in order to implement the desired app behavior.
2. Decide on naming conventions for the parameters and values.
**Note**: Custom parameters will not appear in raw data collected in AppsFlyer.
[block:callout]
{
  "type": "info",
  "title": "Tip",
  "body": "The marketers and developers need to decide *together* on the best long term system for naming parameters to minimize subsequent app updates.\n\nWe strongly recommend agreeing with your marketer on a system that allows for them to enter dynamic values on your chosen parameter, **so they can generate many different deep links that go to different content within the app, without any further changes to the app code by you.**\n\nSee the following URL examples. The custom parameter **fruit_name** was chosen by the marketer and developer together. And the developers made the values dynamic, so the marketer could enter any fruit without the need for further work by the dev team. \n\nhttps://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&**fruit_name=apples&discount=24**...\nhttps://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&**fruit_name=bananas&discount=18**...\nhttps://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&**fruit_name=peaches&discount=33**..."
}
[/block]
### Implementing the logic

The deep link opens the onAppOpenAttribution method in the AppDelegate. The parameters in the method input are used to implement the specific user experience when the application is opened.

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
      "code": "func onAppOpenAttribution(_ attributionData: [AnyHashable: Any]) {\n    //Handle Deep Link Data\n    print(\"onAppOpenAttribution data:\")\n    for (key, value) in attributionData {\n        print(key, \":\",value)\n    }\n    walkToSceneWithParams(params: attributionData)\n}\n\n// User logic\nfileprivate func walkToSceneWithParams(params: [AnyHashable:Any]) {\n    let storyBoard: UIStoryboard = UIStoryboard(name: \"Main\", bundle: nil)\n    UIApplication.shared.windows.first?.rootViewController?.dismiss(animated: true, completion: nil)\n\n    var fruitNameStr = \"\"\n\n    if let thisFruitName = params[\"fruit_name\"] as? String {\n        fruitNameStr = thisFruitName\n    } else if let linkParam = params[\"link\"] as? String {\n        guard let url = URLComponents(string: linkParam) else {\n            print(\"Could not extract query params from link\")\n            return\n        }\n        if let thisFruitName = url.queryItems?.first(where: { $0.name == \"fruit_name\" })?.value {\n            fruitNameStr = thisFruitName\n        }\n    }\n\n    let destVC = fruitNameStr + \"_vc\"\n    if let newVC = storyBoard.instantiateVC(withIdentifier: destVC) {\n\n        print(\"AppsFlyer routing to section: \\(destVC)\")\n        newVC.attributionData = params\n\n        UIApplication.shared.windows.first?.rootViewController?.present(newVC, animated: true, completion: nil)\n    } else {\n        print(\"AppsFlyer: could not find section: \\(destVC)\")\n    }\n}",
      "language": "swift",
      "gist": "84fefc0e933f27c1378bba2c7e8f1121"
    }
  ]
}
[/block]
⇲ Github links: [Swift][oaoa_swift]

[oaoa_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/07f6d6d4b6897756942787774a8adb69c26838a5/swift/basic_app/basic_app/AppDelegate.swift#L151-L159

### Implementing onAttributionFailure method

The onAttributionFailure method is called whenever the call to onAppOpenAttribution fails. The function should report the error and create an expected experience for the user.

**To implement the onAttributionFailure method**:

Enter the following code:
[block:code]
{
  "codes": [
    {
      "code": "func onAppOpenAttributionFailure(_ error: Error) {\n    print(\"\\(error)\")\n}",
      "language": "swift",
      "gist": "88435ea072029dbf43b3c6269bfc727b"
    }
  ]
}
[/block]
⇲ Github links: [Swift][oafailure_swift]

[oaoa_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/07f6d6d4b6897756942787774a8adb69c26838a5/swift/basic_app/basic_app/AppDelegate.swift#L161-L163