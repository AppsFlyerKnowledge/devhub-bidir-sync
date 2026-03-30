---
title: Unified deep linking (UDL)
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
> ❗️ This page has a newer [version](https://dev.appsflyer.com/hc/docs/dl_ios_unified_deep_linking)

**At a glance:** Unified deep linking (UDL) enables you to send new and existing users to a specific in-app activity (for example, a specific page in the app) as soon as the app is opened.

> 📘 UDL privacy protection
> 
> For new users, the UDL method only returns parameters relevant to deferred deep linking: `deep_link_value` and `deep_link_sub1-10`. If you try to get any other parameters (`media_source`, `campaign`, `af_sub1-5`, etc.), they return `null`.

## Flow

![](https://files.readme.io/b1079fb-6577_Unified_Deep_Link_flow_iOS.png "6577 Unified Deep Link flow iOS.png")

UDL routes mobile users into a specific activity or content in an app.

The flow works as follows:

1. User clicks a OneLink link.
   - If the user has the app installed, the Universal Links or URI scheme opens the app. 
   - If the user doesn’t have the app installed, they are redirected to the app store, and after downloading, the user opens the app. 
2. The app open triggers the AppsFlyer SDK.
3. The AppsFlyer SDK runs the UDL API. 
4. The UDL API retrieves OneLink data from AppsFlyer servers. 
5. The UDL API calls back the [`didResolveDeepLink()`] in the [`DeepLinkDelegate`].
6. The [`didResolveDeepLink()`] method gets a [`DeepLinkResult`] object. 
7. The [`DeepLinkResult`] object includes:
   - Status (Found/Not found/Failure)
   - A [`DeepLink`] object that carries the `deep_link_value` and `deep_link_sub1-10` parameters that the developer uses to route the user to a specific in-app activity, which is the main goal of OneLink.

[`didResolveDeepLink()`]: https://dev.appsflyer.com/hc/docs/deeplinkdelegate#didresolvedeeplink

[`DeepLinkDelegate`]: https://dev.appsflyer.com/hc/docs/appsflyerlib-1#deeplinkdelegate

[`DeepLinkResult`]: https://dev.appsflyer.com/hc/docs/deeplinkresult-1

[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink-1

## Prerequisites

- UDL requires AppsFlyer iOS SDK V6.1+.

## Planning

When setting up OneLinks, the marketer uses parameters to create the links, and the developer customizes the behavior of the app based on the values received. It is the developer's responsibility to make sure the parameters are handled correctly in the app, for both in-app routing, and personalizing data in the link.

**To plan the OneLink:**

1. Get from the marketer the desired behavior and personal experience a user gets when they click the URL.
2. Based on the desired behavior, plan the `deep_link_value` and other parameters that are needed to give the user the desired personal experience.
   - The `deep_link_value` is set by the marketer in the URL and used by the developer to redirect the user to a specific place inside the app. For example, if you have a fruit store and want to direct users to apples, the value of `deep_link_value` can be `apples`.
   - The `deep_link_sub1-10`  parameters can also be added to the URL to help personalize the user experience. For example, to give a 10% discount, the value of `deep_link_sub1` can be `10`. 

## Implementation


[block:tutorial-tile]
{
  "backgroundColor": "#cef2d1",
  "emoji": "🍏",
  "id": "6457ad6ec05fc3006a322fc4",
  "link": "https://dev.appsflyer.com/v0.1/recipes/unified-deep-linking-udl-api-in-ios",
  "slug": "unified-deep-linking-udl-api-in-ios",
  "title": "Unified Deep Linking (UDL) API in iOS"
}
[/block]




Implement the UDL API logic based on the chosen parameters and values.

1. Assign the `AppDelegate` using `self` to [`AppsFlyerLib.shared().deepLinkDelegate`](https://dev.appsflyer.com/hc/docs/appsflyerlib-1#deeplinkdelegate).
2. Implement application function to allow:
   - Universal Links support with [`continue`](https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyerlib#continue)
   - URI scheme support with [`handleOpen`](https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyerlib#handleopen)
3. Create [`DeepLinkDelegate`](https://dev.appsflyer.com/hc/docs/deeplinkdelegate) as an extension of `AppDelegate`.
4. Add `application` functions to support Universal Links and URI schemes. 
5. In `DeepLinkDelegate`, make sure you override the callback function, [`didResolveDeepLink()`](https://dev.appsflyer.com/hc/docs/deeplinkdelegate#didresolvedeeplink).  
   `didResolveDeepLink()` accepts a [`DeepLinkResult`](https://dev.appsflyer.com/hc/docs/deeplinkresult-1) object as an argument. 
6. Use [`DeepLinkResult.status`](https://dev.appsflyer.com/hc/docs/deeplinkresult-1#status) to query whether the deep linking match is found.
7. For when the status is an error, call [`DeepLinkResult.error`](https://dev.appsflyer.com/hc/docs/deeplinkresult-1#error) and run your error flow.
8. For when the status is found, use [`DeepLinkResult.deepLink`](https://dev.appsflyer.com/hc/docs/deeplinkresult-1#deeplink) to retrieve the [`DeepLink`](https://dev.appsflyer.com/hc/docs/deeplink-1) object.  
   The `DeepLink` object contains the deep linking information arranged in public variables to retrieve the values from well-known OneLink keys, for example, [`DeepLink.deeplinkValue`](https://dev.appsflyer.com/hc/docs/deeplink-1#deeplinkvalue) for `deep_link_value`.
9. Use [`deepLinkObj.clickEvent["deep_link_sub1"]`](https://dev.appsflyer.com/hc/docs/deeplink-1#clickevent) to retrieve `deep_link_sub1`. Do the same for `deep_link_sub2-10` parameters, changing the string value as required.
10. Once `deep_link_value` and `deep_link_sub1-10` are retrieved, pass them to an in-app router and use them to personalize the user experience.

### Code example

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
  ...
  AppsFlyerLib.shared().deepLinkDelegate = self
  ...
}

// For Swift version < 4.2 replace function signature with the commented out code
// func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([Any]?) -> Void) -> Bool { // this line for Swift < 4.2
func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool {
  AppsFlyerLib.shared().continue(userActivity, restorationHandler: nil)
  return true
}

// Open URI-scheme for iOS 9 and above
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
  AppsFlyerLib.shared().handleOpen(url, options: options)
  return true
}

extension AppDelegate: DeepLinkDelegate {
    func didResolveDeepLink(_ result: DeepLinkResult) {
        var fruitNameStr: String?
        switch result.status {
        case .notFound:
            NSLog("[AFSDK] Deep link not found")
            return
        case .failure:
            print("Error %@", result.error!)
            return
        case .found:
            NSLog("[AFSDK] Deep link found")
        }
        
        guard let deepLinkObj:DeepLink = result.deepLink else {
            NSLog("[AFSDK] Could not extract deep link object")
            return
        }
        
        if deepLinkObj.clickEvent.keys.contains("deep_link_sub2") {
            let ReferrerId:String = deepLinkObj.clickEvent["deep_link_sub2"] as! String
            NSLog("[AFSDK] AppsFlyer: Referrer ID: \(ReferrerId)")
        } else {
            NSLog("[AFSDK] Could not extract referrerId")
        }        
        
        let deepLinkStr:String = deepLinkObj.toString()
        NSLog("[AFSDK] DeepLink data is: \(deepLinkStr)")
            
        if( deepLinkObj.isDeferred == true) {
            NSLog("[AFSDK] This is a deferred deep link")
        }
        else {
            NSLog("[AFSDK] This is a direct deep link")
        }
        
        fruitNameStr = deepLinkObj.deeplinkValue
        walkToSceneWithParams(fruitName: fruitNameStr!, deepLinkData: deepLinkObj.clickEvent)
    }
}
// User logic
fileprivate func walkToSceneWithParams(deepLinkObj: DeepLink) {
    let storyBoard: UIStoryboard = UIStoryboard(name: "Main", bundle: nil)
    UIApplication.shared.windows.first?.rootViewController?.dismiss(animated: true, completion: nil)
    guard let fruitNameStr = deepLinkObj.clickEvent["deep_link_value"] as? String else {
         print("Could not extract query params from link")
         return
    }
    let destVC = fruitNameStr + "_vc"
    if let newVC = storyBoard.instantiateVC(withIdentifier: destVC) {
       print("AppsFlyer routing to section: \(destVC)")
       newVC.deepLinkData = deepLinkObj
       UIApplication.shared.windows.first?.rootViewController?.present(newVC, animated: true, completion: nil)
    } else {
        print("AppsFlyer: could not find section: \(destVC)")
    }
}
```



⇲ Github links: [Swift][ocds_swift]

[ocds_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/a96399329a369b30263ea4f8cc4558029ea603b3/swift/basic_app/basic_app/AppDelegate.swift#L126