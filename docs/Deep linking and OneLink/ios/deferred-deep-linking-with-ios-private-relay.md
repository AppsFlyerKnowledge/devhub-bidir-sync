---
title: Deferred deep linking with iOS Private Relay
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
> ❗️ This page has a newer [version](https://dev.appsflyer.com/hc/docs/dl_ios_private_relay)

With the launch of 1OS 15, Apple provides iCloud+ users with a feature called Private Relay, which gives them the option to encrypt their web-browsing traffic and hide their exact location, IP address, and the contents of their browsing traffic. If users opt-in to Private Relay, this could interfere with attribution and deferred deep linking. Meaning, once a new user without the app goes to the App Store, and installs and launches the app, Private Relay could prevent them from being sent to a specific page in the app.

To ensure that deferred deep linking (DDL) continues to work as expected, you need to implement one of the following AppsFlyer solutions:

- **[Recommended] App Clip-based solution**: Create an App Clip that gives you user attribution data, and directs users to a customized App Clip experience similar to the one you want DDL to achieve. The app clip can also include a flow to direct users from your App Clip to your full app.
- **Clipboard-based solution**: Create a web landing page that copies the deferred deep linking data from the URL and correctly redirects the user to the app. Note: This solution does not help with attribution.

## App Clip-based solution

**Prerequisites**: AppsFlyer SDK V6.4.0+

**To set up the App Clip-based DDL solution**:

1. Follow the [Apple instructions](https://developer.apple.com/documentation/app_clips) and develop an App Clip that provides the desired user journey.
2. [Integrate the AppsFlyer SDK for App Clips](https://dev.appsflyer.com/hc/docs/app-clip-sdk-integration), including [App Clip-to-full app attribution](https://dev.appsflyer.com/hc/docs/app-clip-to-full-app-install).
3. In the App Clip `sceneDelegate`:
   - Replace `scene` `continue userActivity` with the following function:

```swift
func scene(_ scene: UIScene, continue userActivity: NSUserActivity) {
  // Must for AppsFlyer attrib
  AppsFlyerLib.shared().continue(userActivity, restorationHandler: nil)

  //Get the invocation URL from the userActivity in order to add it to the shared user default
  guard userActivity.activityType == NSUserActivityTypeBrowsingWeb,
  let invocationURL = userActivity.webpageURL else {
    return
  }
  addDlUrlToSharedUserDefaults(invocationURL)        
}
```



⇲ Github links: [Swift][scene_swift]

[scene_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/0a11ff86ca01e0c279010eebc00baf35bf88da2e/swift/basic_app/basic_app_AppClip/SceneDelegate.swift#L17-L28

- Add the following method: 

```swift
func addDlUrlToSharedUserDefaults(_ url: URL){
  guard let sharedUserDefaults = UserDefaults(suiteName: "group.<your_app>.appClipToFullApp") else {
    return
  }
  //Add invocation URL to the app group
  sharedUserDefaults.set(url, forKey: "dl_url")
  //Enable sending events
  sharedUserDefaults.set(true, forKey: "AppsFlyerReadyToSendEvents")
}
```



⇲ Github links: [Swift][scene_swift]

[scene_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/0a11ff86ca01e0c279010eebc00baf35bf88da2e/swift/basic_app/basic_app_AppClip/SceneDelegate.swift#L70-L78

4. In the full app:
   - In `appDelegate`, add the following method:

```swift
func deepLinkFromAppClip() {
  guard let sharedUserDefaults = UserDefaults(suiteName: "group.<your_app>.appClipToFullApp"),
  let dlUrl = sharedUserDefaults.url(forKey: "dl_url")
  else {
    NSLog("Could not find the App Group or the deep link URL from the app clip")
    return
  }
  AppsFlyerLib.shared().performOnAppAttribution(with: dlUrl)
  sharedUserDefaults.removeObject(forKey: "dl_url")
}
```



⇲ Github links: [Swift][scene_swift]

[scene_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/0a11ff86ca01e0c279010eebc00baf35bf88da2e/swift/basic_app/basic_app/AppDelegate.swift#L123-L134

- At the end of the `application didFinishLaunchingWithOptions launchOptions` method, call `deepLinkFromAppClip`: 

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

  // ...

  deepLinkFromAppClip()

  return true
}
```



⇲ Github links: [Swift][scene_swift]

[scene_swift]: https://github.com/AppsFlyerSDK/appsflyer-onelink-ios-sample-apps/blob/0a11ff86ca01e0c279010eebc00baf35bf88da2e/swift/basic_app/basic_app/AppDelegate.swift#L54

## Clipboard-based solution

**To set up the clipboard-based solution**:

1. Enter the following code in `appDelegate`.

```objectivec
NSString *pasteboardUrl = [[UIPasteboard generalPasteboard] string];
NSString *checkParameter = @"cp_url=true";

if ([pasteboardUrl containsString:checkParameter]) {
  [[AppsFlyerLib shared] performOnAppAttributionWithURL:[NSURL URLWithString:pasteboardUrl]];
}
```
```swift
var pasteboardUrl = UIPasteboard.general.string ?? ""
let checkParameter = "cp_url=true"

if pasteboardUrl.contains(checkParameter) {
    AppsFlyerLib.shared().performOnAppAttribution(with: URL(string: pasteboardUrl))
}
```



2. Implement code that pastes the deferred deep link data in the URL from the clipboard. This is not part of the AppsFlyer SDK.