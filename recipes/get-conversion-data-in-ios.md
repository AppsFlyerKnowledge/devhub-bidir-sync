---
title: Get Conversion Data in iOS
description: >-
  For developers who wish to learn step-by-step how to get conversion data using
  the GetConversionData interface
hidden: false
recipe:
  color: '#01f476'
  icon: 👟
---
```swift Swift
import UIKit
import AppsFlyerLib
import AppTrackingTransparency

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    var ConversionData: [AnyHashable: Any]? = nil
    var window: UIWindow?
    var dlHappened = false

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        

        // Get AppsFlyer preferences from .plist file
        guard let propertiesPath = Bundle.main.path(forResource: "afdevkey", ofType: "plist"),
            let properties = NSDictionary(contentsOfFile: propertiesPath) as? [String:String] else {
                fatalError("Cannot find `afdevkey`")
        }
        guard let appsFlyerDevKey = properties["appsFlyerDevKey"],
                   let appleAppID = properties["appleAppID"] else {
            fatalError("Cannot find `appsFlyerDevKey` or `appleAppID` key")
        }
        
        //  Set isDebug to true to see AppsFlyer debug logs
        AppsFlyerLib.shared().isDebug = true
        
        // Replace 'appsFlyerDevKey', `appleAppID` with your DevKey, Apple App ID
        AppsFlyerLib.shared().appsFlyerDevKey = appsFlyerDevKey
        AppsFlyerLib.shared().appleAppID = appleAppID
        
        AppsFlyerLib.shared().waitForATTUserAuthorization(timeoutInterval: 60)
        
        AppsFlyerLib.shared().delegate = self
        
        // Subscribe to didBecomeActiveNotification if you use SceneDelegate or just call
        // -[AppsFlyerLib start] from -[AppDelegate applicationDidBecomeActive:]
        NotificationCenter.default.addObserver(self, selector: #selector(didBecomeActiveNotification),
        // For Swift version < 4.2 replace name argument with the commented out code
        name: UIApplication.didBecomeActiveNotification, //.UIApplicationDidBecomeActive for Swift < 4.2
        object: nil)
        
        return true
    }
    
    @objc func didBecomeActiveNotification() {
        AppsFlyerLib.shared().start()
        
        if #available(iOS 14, *) {
          ATTrackingManager.requestTrackingAuthorization { (status) in
            switch status {
            case .denied:
                print("AuthorizationSatus is denied")
            case .notDetermined:
                print("AuthorizationSatus is notDetermined")
            case .restricted:
                print("AuthorizationSatus is restricted")
            case .authorized:
                print("AuthorizationSatus is authorized")
            @unknown default:
                fatalError("Invalid authorization status")
            }
          }
        }
    }
}

extension AppDelegate: AppsFlyerLibDelegate {
     
    // Handle Organic/Non-organic installation
    func onConversionDataSuccess(_ data: [AnyHashable: Any]) {
        ConversionData = data
        print("onConversionDataSuccess data:")
        for (key, value) in data {
            print(key, ":", value)
        }
        
        if let status = data["af_status"] as? String {
            if (status == "Non-organic") {
                if let sourceID = data["media_source"],
                    let campaign = data["campaign"] {
                    NSLog("[AFSDK] This is a Non-Organic install. Media source: \(sourceID)  Campaign: \(campaign)")
                }
            } else {
                NSLog("[AFSDK] This is an organic install.")
            }
            if let is_first_launch = data["is_first_launch"] as? Bool,
                is_first_launch {
                NSLog("[AFSDK] First Launch")
            } else {
                NSLog("[AFSDK] Not First Launch")
            }
        }
    }
    
    func onConversionDataFail(_ error: Error) {
        NSLog("[AFSDK] \(error)")
    }
}

```

# Create AppsFlyerLibDelegate

<!-- swift@67 -->

[`AppsFlyerLibDelegate`] is an extension of `AppDelegate`

[`AppsFlyerLibDelegate`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate

# Register AppsFlyerLibDelegate in the SDK

<!-- swift@33 -->

>Happens in `didFinishLaunchingWithOptions`

The `delegate` parameter holds the delegate class that contains the GCD call back functions.
We used `self` since [`AppsFlyerLibDelegate`] is an extension of `AppDelegate` 

[`AppsFlyerLibDelegate`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate

# Implement onConversionDataSuccess()

<!-- swift@70-93 -->

[`onConversionDataSuccess()`] is called  on **every** app launch. 

The attribution data is passed as a parameter from type `data: [AnyHashable: Any]`.
The input parameters are listed [here](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#input-parameters).

> 

[`onConversionDataSuccess()`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate#onconversiondatasuccess

# Detect successful attribution

<!-- swift@77 -->

If `af_status` field in [`data`] equals `Non-organic`, this install is **successfully attributed** by AppsFlyer, and other fields are valid.  


[`data`]: https://dev.appsflyer.com/hc/docs/ios-legacy-apis#input-parameters

# Detect first launch

<!-- swift@86-88 -->

[`onConversionDataSuccess()`] is called on **every** app launch.
If `is_first_launch` field in [`conversionData`] equals `true`, this is the very first launch.

[`conversionData`]: https://dev.appsflyer.com/hc/docs/ios-legacy-apis#input-parameters
[`onConversionDataSuccess()`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate#onconversiondatasuccess

# Handle fail to get conversion data

<!-- swift@95-97 -->

If the attribution operations fails (*from the SDK side*), [`onConversionDataFail()`] is called. Implement here error handling and reporting 

[`onConversionDataFail()`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate#onconversiondatafail