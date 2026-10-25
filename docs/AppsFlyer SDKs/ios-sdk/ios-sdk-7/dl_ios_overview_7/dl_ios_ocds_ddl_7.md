---
title: iOS Extended Deferred Deep Linking
hidden: false
---

## Overview

Extended deferred deep linking allows deep linking for new users in certain scenarios:

- When UDL returns `NOT_FOUND` even though a relevant install occurred.
  Main example for such a scenario:
  - Time between click and install exceeds the UDL lookback window (15 minutes).
- When UDL returns `FOUND` but the deep linking data is missing parameters, which are not `deep_link_value` and `deep_link_sub1-10`.  
  Main examples for such scenarios:
  - Clicking a link that doesn't contain `deep_link_value` or `deep_link_sub1-10` used for deep-linking, for example, old links created before `deep_link_value` existed that are still in use.
  - Time between click and install exceeds the UDL lookback window (15 minutes).

To allow deferred deep linking when UDL returns `NOT_FOUND`, `onConversionDataSuccess` callback should check whether it should handle the deferred deep linking.  
`onConversionDataSuccess` is part of the Get Conversion Data(GCD) API. Its main purpose was is to [gather conversion data inside the device](https://dev.appsflyer.com/hc/docs/conversion-data-ios-7).  
In the use case outlined here `onConversionDataSuccess` takes advantage of the fact that all deferred deep linking parameters are passed to the callback, on top of the conversion data. 

## Prerequisites

- Implement [Unified Deep Linking](dl_ios_unified_deep_linking_7) to handle both deferred deep linking and direct deep linking.
- Implement `onConversionDataSuccess` to handle deferred deep linking. See [Get conversion data](conversion-data-ios-7).

## Implementation

1. `onConversionDataSuccess` should detect cases where deferred deep linking should occur that UDL didn't handle. 
   > See detailed [code dissection](#code-dissect)
2. `onConversionDataSuccess` should route the user to the deferred deep linking destination based on the deep linking parameters passed to the callback.

## Code example

### Code dissect

1. Implement the Get Conversion Data API delegate `AppsFlyerLibDelegate`. 
   >  Implement only `onConversionDataSuccess` and `onConversionDataFail`.  
   > The methods `onAppOpenAttribution` and `onAppOpenAttributionFailure` are removed in SDK 7.
2. Detect deferred deep linking scenarios by filtering-in the conversion data payload with:
   - `af_status == Non-organic`
   - `is_first_launch == true`
3. When deferred deep linking is detected, filter-out the cases that were already handled by UDL.  
   In the example that follows, all the links contain `deep_link_value`.  
   It is recommended for UDL to signal with a flag that deferred deep linking was already handled, and `onConversionDataSuccess` should skip.
4. `onConversionDataSuccess` should verify the conversion data holds parameters that are used to route users inside the application. For example `fruit_name` in the example that follows.       
5. Route the user to the deferred deep linking destination.

### Code snippet

```swift
extension AppDelegate: AppsFlyerLibDelegate {
     
    // Handle Organic/Non-organic installation
    func onConversionDataSuccess(_ data: [AnyHashable: Any]) {
        ConversionData = data
        print("onConversionDataSuccess data:")
        for (key, value) in data {
            print(key, ":", value)
        }
        if let conversionData = data as NSDictionary? as! [String:Any]? {
        
            if let status = conversionData["af_status"] as? String {
                if (status == "Non-organic") {
                    if let sourceID = conversionData["media_source"],
                        let campaign = conversionData["campaign"] {
                        NSLog("[AFSDK] This is a Non-Organic install. Media source: \(sourceID)  Campaign: \(campaign)")
                    }
                } else {
                    NSLog("[AFSDK] This is an organic install.")
                }
                
                if let is_first_launch = conversionData["is_first_launch"] as? Bool,
                    is_first_launch {
                    NSLog("[AFSDK] First Launch")
                    if !conversionData.keys.contains("deep_link_value") && conversionData.keys.contains("fruit_name"){
                        switch conversionData["fruit_name"] {
                            case let fruitNameStr as String:
                            NSLog("This is a deferred deep link opened using conversion data")
                            walkToSceneWithParams(fruitName: fruitNameStr, deepLinkData: conversionData)
                            default:
                                NSLog("Could not extract deep_link_value or fruit_name from deep link object using conversion data")
                                return
                        }
                    }
                } else {
                    NSLog("[AFSDK] Not First Launch")
                }
            }
        }
    }
    
    func onConversionDataFail(_ error: Error) {
        NSLog("[AFSDK] \(error)")
    }
}
```



⇲ Github links: [Swift](https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/7c58363b01a184863d3b3fc07ba707a72d76bcda/swift/basic_app/basic_app/AppDelegate.swift#L168-L212)

## Testing

> 📘 **Important**
> 
> The following testing scenario demonstrates the handling of deferred deep linking from links that contain custom parameters but not `deep_link_value` and `deep_link_sub1-10` parameters.  
> This testing scenario is also relevant for all extended deferred deep linking described [earlier](#overview).

### Before you begin

- Complete the implementation described earlier.
- [Register your testing device](https://support.appsflyer.com/hc/en-us/articles/207031996).
- [Enable debug mode](https://dev.appsflyer.com/hc/docs/integrate-ios-sdk#enabling-debug-mode) in the app.
- Make sure the app isn't installed on your device.

### The test link

You can use an existing OneLink link or ask your marketer to create a new one for testing. Both short and long OneLink URLs can be used.

#### Adding ad-hoc parameters to the link

- Use only the domain and OneLink template of your link, for example: `https://onelink-basic-app.onelink.me/H5hv`.
- Add OneLink custom parameters other than `deep_link_value` and `deep_link_sub1-10`, as expected by your application. 
- The parameters should be added as _query parameters_.
  - Example: `https://onelink-basic-app.onelink.me/H5hv?my_inapp_dest=apples&my_inapp_value=23`  

### Perform the test

- Click the link on your device.
- OneLink redirects you according to the link setup to wither the App Store or a website. 
- Install the application.
  > **Important**
  >
  > - If the application is still in development and not uploaded to the store yet, the following image displays:  
  >   <img src="https://files.readme.io/8d43627-Screenshot_20221205-191054_Chrome.jpg" alt="drawing" width="250" style={{textAlign: "center"}} />
  > - Install the application from Xcode or any other IDE you use.
- UDL detects the deferred deep linking, matches the install to the click, and retrieves the OneLink parameters to `didResolveDeepLink` callback. **UDL will not find any parameters to route and exit**.
- `onConversionDataSuccess` callback is called with the conversion data, which holds both custom parameters and attribution data.
- `onConversionDataSuccess` sets the custom parameters to route the user inside the application.
