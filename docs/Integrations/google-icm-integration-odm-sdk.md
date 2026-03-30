---
title: Google ICM integration (ODM SDK)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Starting from AppsFlyer iOS SDK v6.17.7, the SDK can automatically fetch Google’s ICM (Integrated Conversion Measurement) data from Google’s On Device Measurement (ODM) SDK. 

#### To enable the Google ICM integration via ODM:

1. **Integrate Google’s ODM event data SDK**  
   Integrate the On-Device Measurement (ODM) event data SDK into your app, either as a standalone SDK or via the Firebase iOS SDK.

   > 📘 Note
   > 
   > For integration instructions, see the [Google documentation](https://developers.google.com/app-conversion-tracking/api/integrated-conversion-measurement#integrate_the_standalone_sdk). 
   > 
   > - Include the ODM library via Firebase 11.14.0 or later, or as a standalone SDK. 
   > - **Important:** Do not implement the **Use the On Device Measurement (ODM): Event Data SDK** section.

2. **Load the GoogleAdsOnDeviceConversion library before AppsFlyer initialization**  
   Before initializing the AppsFlyer SDK, import `GoogleAdsOnDeviceConversion` and invoke `ConversionManager.sharedInstance`.

   AppsFlyer dynamically loads Google’s ODM method implementations at runtime. Accessing `ConversionManager.sharedInstance` ensures the **GoogleAdsOnDeviceConversion** library is loaded into memory so the AppsFlyer SDK can use it.

3. **Initialize and start the AppsFlyer iOS SDK**  
   After ensuring the ODM SDK is loaded, initialize and start the AppsFlyer SDK as described here:  
   <https://dev.appsflyer.com/hc/docs/integrate-ios-sdk>

#### Implementation example (Swift)

```swift
import AppsFlyerLib
import GoogleAdsOnDeviceConversion

@available(iOS 13.0, *)
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {

        // Ensure GoogleAdsOnDeviceConversion is loaded into memory
        let _ = ConversionManager.sharedInstance

        // AppsFlyer SDK configuration
        let afLib = AppsFlyerLib.shared()
        afLib.appsFlyerDevKey = "<YOUR_DEVKEY_HERE>"
        afLib.appleAppID = "<YOUR_APP_ID_HERE>"
        afLib.deepLinkDelegate = self
        afLib.delegate = self

        return true
    }

    func applicationDidBecomeActive(_ application: UIApplication) {
        AppsFlyerLib.shared().start()
    }
}

 
```