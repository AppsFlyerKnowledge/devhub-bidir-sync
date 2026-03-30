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
        "https://files.readme.io/979e788-direct_and_deferred_deep_linking.png",
        "direct and deferred deep linking.png",
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
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/fc5cee9-5472_IOS_OneLink_AppLink_1.png",
        "5472_IOS_OneLink_AppLink (1).png",
        1920,
        2160,
        "#d6e7f6"
      ]
    }
  ]
}
[/block]
**The deep linking flow works as follows**:

1. User clicks the OneLink short URL.
2. Android launches the app based on the relevant activity in the `AndroidManifest.xml`.
3. AppsFlyer SDK is triggered in the app.
4. AppsFlyer SDK retrieves the OneLink data.
   * In a short URL, the data is retrieved from the short URL resolver API in AppsFlyer's servers.
   * In a long URL, the data is retrieved directly from the long URL.
5. AppsFlyer SDK triggers `onAppOpenAttribution()` with the retrieved parameters and cached attribution parameters (e.g.`install_time`).
6. Asynchronously, `onConversionDataSuccess()` is called, holding the full cached attribution data. (You can exit this function by checking if `is_first_launch` is `true`.)
7. `onAppOpenAttribution()` uses the `attributionData` map to dispatch other activities in the app and pass relevant data.
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
    "0-0": "1. Deciding app behavior, and parameter names and values (with marketer)",
    "1-0": "2. Planning method input, i.e. parameter names and values (with marketer)",
    "2-0": "3. Implementing the logic",
    "3-0": "4. Implementing the onAttributionFailure method",
    "h-0": "Procedure checklist"
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

The onAppOpenAttribution method gets variables as an input like this: Map <String, String>. 

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

The deep link opens the onAppOpenAttribution method in the main activity. The parameters in the method input are used to implement the specific user experience when the application is opened.

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
      "code": "@Override\npublic void onAppOpenAttribution(Map<String, String> attributionData) {\n    if (!attributionData.containsKey(\"is_first_launch\"))\n        Log.d(LOG_TAG, \"onAppOpenAttribution: This is NOT deferred deep linking\");\n    for (String attrName : attributionData.keySet()) {\n        String deepLinkAttrStr = attrName + \" = \" + attributionData.get(attrName);\n        Log.d(LOG_TAG, \"Deeplink attribute: \" + deepLinkAttrStr);\n    }\n    Log.d(LOG_TAG, \"onAppOpenAttribution: Deep linking into \" + attributionData.get(\"fruit_name\"));\n    goToFruit(attributionData.get(\"fruit_name\"), attributionData);\n}\n\nprivate void goToFruit(String fruitName, Map<String, String> dlData) {\n    String fruitClassName = fruitName.concat(\"Activity\");\n    try {\n        Class fruitClass = Class.forName(this.getPackageName().concat(\".\").concat(fruitClassName));\n        Log.d(LOG_TAG, \"Looking for class \" + fruitClass);\n        Intent intent = new Intent(getApplicationContext(), fruitClass);\n        if (dlData != null) {\n            // Map is casted HashMap since it is easier to pass serializable data to an intent\n            HashMap<String, String> copy = new HashMap<String, String>(dlData);\n            intent.putExtra(DL_ATTRS, copy);\n        }\n        startActivity(intent);\n    } catch (ClassNotFoundException e) {\n        Log.d(LOG_TAG, \"Deep linking failed looking for \" + fruitName);\n        e.printStackTrace();\n    }\n}",
      "language": "java",
      "gist": "fbb43768ad899ec7c4027868af507c71"
    }
  ]
}
[/block]
⇲ Github links: [Java][oaoa_java]

[oaoa_java]: https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/5b202b983b33d62bd5d80102ab27f17e2b1cb25f/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/MainActivity.java#L64-L73

### Implementing onAttributionFailure method

The onAttributionFailure method is called whenever the call to onAppOpenAttribution fails. The function should report the error and create an expected experience for the user.

**To implement the onAttributionFailure method**:

Enter the following code:
[block:code]
{
  "codes": [
    {
      "code": "@Override\npublic void onAttributionFailure(String errorMessage) {\n    Log.d(LOG_TAG, \"error onAttributionFailure : \" + errorMessage);\n}",
      "language": "java",
      "gist": "6db22970c12cd530c4226f28ac57e64f"
    }
  ]
}
[/block]
⇲ Github links: [Java][oaf_java]

[oaf_java]: https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/5b202b983b33d62bd5d80102ab27f17e2b1cb25f/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/MainActivity.java#L75-L78