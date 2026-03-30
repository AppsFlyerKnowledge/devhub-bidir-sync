---
title: Deferred deep linking
excerpt: This page discusses deferred deep linking
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
        "https://files.readme.io/43463fd-direct_and_deferred_deep_linking.png",
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
Deferred deep linking directs new users first to the correct app store to install the app, and then, after the first open, to a specific app experience (e.g. a specific page in the app).

When the user first launches the app, the onConversionDataSuccess callback function receives both the conversion data of the new user, and OneLink data. The OneLink data makes in-app routing possible due to the additional parameters that are passed to the app when the OS opens the app. AppsFlyer's OneLink ensures that the correct parameters are passed along with the user's click, thus personalizing the user’s app experience. 

The marketer and developer must coordinate regarding desired app behavior and parameter names. The marketer will use the parameters to create the deferred deep links. 

**It is the developer's responsibility to make sure the parameters are handled correctly in the app** - for both in-app routing, and personalizing data in the link.

**The deep linking flow works as follows**:

1. User clicks the OneLink on a device on which the app is not installed.
2. AppsFlyer registers the click and redirects the user to the correct app store or landing page.
3. The user installs the application and launches it.
4. AppsFlyer SDK is initialized and the install is attributed in AppsFlyers's servers.
5. The SDK triggers the onConversionDataSuccess method. The function receives input that includes both attribution data and the parameters defined in the OneLink data.
6. The parameter `is_first_launch` has the value `true`, that signals the deferred deep link flow. 
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

The onConversionDataSuccess method gets variables as an input like this: Map <String, Object>. View the list of variables (parameters).  

The map holds two kinds of data:
* [Attribution data](https://support.appsflyer.com/hc/en-us/articles/207447163#attribution-link-parameters)
* Data defined by the marketer in the link (parameters and values)
Parameters can be either:
   * AppsFlyer official parameters.
   * Custom parameters and values chosen by the marketer and developer.

The input data structure is described [here](https://dev.appsflyer.com/docs/direct-deep-linking-1).

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
      "code": "@Override\n public void onConversionDataSuccess(Map<String, Object> conversionData) {\n     for (String attrName : conversionData.keySet())\n         Log.d(LOG_TAG, \"Conversion attribute: \" + attrName + \" = \" + conversionData.get(attrName));\n     //TODO - remove this\n     String status = Objects.requireNonNull(conversionData.get(\"af_status\")).toString();\n     if(status.equals(\"Non-organic\")){\n         if( Objects.requireNonNull(conversionData.get(\"is_first_launch\")).toString().equals(\"true\")){\n             Log.d(LOG_TAG,\"Conversion: First Launch\");\n             if (conversionData.containsKey(\"fruit_name\")){\n                 Log.d(LOG_TAG,\"Conversion: This is deferred deep linking.\");\n                 //  TODO SDK in future versions - match the input types\n                 Map<String,String> newMap = new HashMap<>();\n                 for (Map.Entry<String, Object> entry : conversionData.entrySet()) {\n                         newMap.put(entry.getKey(), String.valueOf(entry.getValue()));\n                 }\n                 onAppOpenAttribution(newMap);\n             }\n         } else {\n             Log.d(LOG_TAG,\"Conversion: Not First Launch\");\n         }\n     } else {\n         Log.d(LOG_TAG,\"Conversion: This is an organic install.\");\n     }\n }\n",
      "language": "java",
      "gist": "fdc41a7b963acf1c656e4655c239307d"
    }
  ]
}
[/block]
⇲ Github links: [Java][ocds_java]

[ocds_java]: https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/5b202b983b33d62bd5d80102ab27f17e2b1cb25f/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/MainActivity.java#L33-L56

### Implementing onConversionDataFailure method

The onConversionDataFailure method is called whenever the call to onConversionDataSuccess fails. The function should report the error and create an expected experience for the user.

**To implement the onConversionDataFailure method**:

Enter the following code:
[block:code]
{
  "codes": [
    {
      "code": "@Override\npublic void onConversionDataFail(String errorMessage) {\n    Log.d(LOG_TAG, \"error getting conversion data: \" + errorMessage);\n}",
      "language": "java",
      "gist": "98d82baa29686dc6c1183420013bae5a"
    }
  ]
}
[/block]
⇲ Github links: [Java][ocdf_java]

[ocdf_java]: https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/5b202b983b33d62bd5d80102ab27f17e2b1cb25f/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/MainActivity.java#L75-L78