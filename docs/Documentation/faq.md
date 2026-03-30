---
title: FAQ
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
### OneLink opens a blank page on Chrome

If OneLink URL is placed inside an html a tag with target="_blank" attribute, it opens a blank page in Chrome on Android and iOS. This affects OneLink functionality. Make sure that the a tag doesn't include the target attribute.

### iOS app launches but onAppOpenAttribution callback is not called

This means that the SDK integration for deep linking was not fully completed. The `continueUserActivity` method on the `AppDelegate` is missing and should be called.

### iOS app launches but immediately redirects to the app store

This usually happens as a result of code in the app that performs a redirection to a browser when an https link launches the app.

### iOS: Tapping the link always redirects to the app store

On iOS 9 & 10 (removed from iOS 11) there is an option to ignore deep linking. When an app is opened by Universal Links, iOS shows a **deep link bypass link** in the top right corner which sets that link to be opened without deep linking.

This is a per-app setting that is saved on each unique device. The setting is preserved even if you delete the app and reinstall it. The only way to reverse this is by re-enabling Universal Link behavior for that app on your device. There are a couple of ways to do this:

To reset this setting, paste the OneLink into Notes or iMessage (or some other app that supports Universal Links) and long-press on it. You'll see an 'Open in [App]' option. Select it, and after that all Universal Links for that app will work again.

Select ***Open in your app*** (e.g. open in LoginBox).
Deep linking is now restored.

**I performed a long tap, but I don't see the "Open in your app" option!**

In this case, the app was not configured properly for Universal Links. Check the following:

* iOS apps are built with a provisioning file (similar to a license). This file is generated on Apple's developer console, and should be enabled for Universal Links.
* On Xcode - make sure ***Associate Domains*** is enabled on the ***Capabilities*** tab and includes the OneLink domain for the app.
* Make sure the team ID of the app matches the one entered on the OneLink templates page.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/343d41e-xcode.png",
        "xcode.png",
        985,
        282,
        "#f8f9f9"
      ]
    }
  ]
}
[/block]
* Make sure the team ID of the app matches the one entered on the OneLink templates page.

### iOS Universal links: Safari opens instead of the app

Delete the app, reinstall and run it again. If the problem persists, try to add another row on the Associate Domains (so it is changed). Then, delete the app and repeat the process. Also make sure your OneLink has the same domain as set on the app.

### iOS Universal links: I tried it all but it still doesn't work. Help!

Hey, we hear you. If all of the above did not resolve the issue, try to generate your provisioning files again for your app. Download them one more time and double-click to install them on Xcode.

You can also try to delete the app and re-install it with an advanced app version.

### iOS Universal links: iOS 11.2 problem

**iOS 11.2, and all its subsequent patches, have a bug that sometimes prevents deep linking through Universal links.**

Apparently, the ***apple-app-site-association*** file, that is used to open apps with Universal links and is automatically downloaded with any iOS app install, sometimes fails to download with app installs of devices running iOS 11.2 and above. Without this file on the iOS device Universal links fail to directly open apps.

If you experience this problem while testing deep linking, uninstall the app, restart your (whitelisted) iOS device and reinstall the app. Didn't help? Try again.

It is still unclear how widespread this sporadic problem really is, although it has been reproduced on multiple iPhone devices running iOS 11.2. Hopefully Apple would soon solve this bug that may affect deep linking for more than 2/3 of its iOS users.

**Note**: You can click the ***Follow*** button at the top of the screen to get updated when this issue is solved, or whenever this article is updated.

### iOS Universal links: iOS 13 problem

**iOS 13 has a bug that sometimes prevents deep linking through Universal links.**

Apparently, a process that runs in iOS 13 to verify installations sometimes fails. This failure prevents iOS from verifying associated domains. As a result, deep linking in iOS 13 might not work. This doesn't always happen but the rate at which it does is unknown at the moment. See [here](https://developer.apple.com/forums/thread/123554) for more information.

As a workaround, you can use URI-scheme deep linking mechanism. By default, when Universal Links fail for any reason, OneLink redirects the user to the store. You can add `af_force_deeplink=true` parameter to the link. Doing so causes OneLink to try and do deep linking using the scheme specified in the `af_dp` parameter.

You can read more about force deep linking [here](https://support.appsflyer.com/hc/en-us/articles/360000724598#forced-deep-linking-solution).

**Note**: You can click the ***Follow*** button at the top of the screen to get updated when this issue is solved, or whenever this article is updated.
[block:image]
{
  "images": [
    {
      "image": []
    }
  ]
}
[/block]