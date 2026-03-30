---
title: Starting the SDK in iOS
description: >-
  For developers who wish to learn step-by-step how to start the SDK and receive
  attribution data using the GetConversionData interface
hidden: false
recipe:
  color: '#def7e8'
  icon: 🏆
---
```swift AppDelegate
import UIKit
import AppsFlyerLib

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    
    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        guard let propertiesPath = Bundle.main.path(forResource: "afdevkey_donotpush", ofType: "plist"),
            let properties = NSDictionary(contentsOfFile: propertiesPath) as? [String:String] else {
                fatalError("Cannot find `afdevkey_donotpush`")
        }
        guard let appsFlyerDevKey = properties["appsFlyerDevKey"],
                   let appleAppID = properties["appleAppID"] else {
            fatalError("Cannot find `appsFlyerDevKey` or `appleAppID` key")
        }
        AppsFlyerLib.shared().appsFlyerDevKey = appsFlyerDevKey
        AppsFlyerLib.shared().appleAppID = appleAppID
        //  Set isDebug to true to see AppsFlyer debug logs
        AppsFlyerLib.shared().isDebug = true
        
        AppsFlyerLib.shared().delegate = self
        
        NotificationCenter.default.addObserver(self,
        selector: #selector(didBecomeActiveNotification),
        // For Swift version < 4.2 replace name argument with the commented out code
        name: UIApplication.didBecomeActiveNotification, //.UIApplicationDidBecomeActive for Swift < 4.2
        object: nil)
        
        return true
    }
    
    @objc func didBecomeActiveNotification() {
        AppsFlyerLib.shared().start()
    }    
   
}

extension AppDelegate: AppsFlyerLibDelegate {
     
    // Handle Organic/Non-organic installation
    func onConversionDataSuccess(_ data: [AnyHashable: Any]) {
        
        print("onConversionDataSuccess data:")
        for (key, value) in data {
            print(key, ":", value)
        }
        
        if let status = data["af_status"] as? String {
            if (status == "Non-organic") {
                if let sourceID = data["media_source"],
                    let campaign = data["campaign"] {
                    print("This is a Non-Organic install. Media source: \(sourceID)  Campaign: \(campaign)")
                }
            } else {
                print("This is an organic install.")
            }
            if let is_first_launch = data["is_first_launch"] as? Bool,
                is_first_launch {
                print("First Launch")
            } else {
                print("Not First Launch")
            }
        }
    }
    
    func onConversionDataFail(_ error: Error) {
        print("\(error)")
    }
}

```

# Import AppsFlyer SDK lib

<!-- swift@2 -->



# Set dev key and app ID

<!-- swift@19-20 -->

>Happens in `didFinishLaunchingWithOptions`

Replace `appsFlyerDevKey`, `appleAppID` with your DevKey, Apple App ID

Instruction on how to obtain the *dev key* can be found [here](https://support.appsflyer.com/hc/en-us/articles/207032066-iOS-SDK-V6-X-integration-guide-for-developers#integration-31-retrieve-your-appsflyer-dev-key)

# Register AppsFlyerLibDelegate in the SDK

<!-- swift@24 -->

>Happens in `didFinishLaunchingWithOptions`

The `delegate` parameter holds the delegate class that contains the GCD call back functions.
We used `self` since [`AppsFlyerLibDelegate`] is an extension of `AppDelegate` 

[`AppsFlyerLibDelegate`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate

# [Optional] for SceneDelegate subscribe to didBecomeActiveNotification

<!-- swift@26-30 -->

`didBecomeActiveNotification` is the name of the function that holds the SDK `start` function

# Start the SDK

<!-- swift@35-37 -->



# Create AppsFlyerLibDelegate

<!-- swift@41-72 -->

[`AppsFlyerLibDelegate`] is an extension of `AppDelegate`

[`AppsFlyerLibDelegate`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate

# Implement onConversionDataSuccess()

<!-- swift@63,86 -->

[`onConversionDataSuccess()`] is called  on **every** app launch. 

The attribution data is passed as a parameter from type `data: [AnyHashable: Any]`.
The input parameters are listed [here](https://dev.appsflyer.com/hc/docs/ios-legacy-apis#input-parameters).

> 

[`onConversionDataSuccess()`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate#onconversiondatasuccess

# Detect successful attribution

<!-- swift@70-78,85 -->

If `af_status` field in [`data`] equals `Non-organic`, this install is **successfully attributed** by AppsFlyer, and other fields are valid.  


[`data`]: https://dev.appsflyer.com/hc/docs/ios-legacy-apis#input-parameters

# Detect first launch

<!-- swift@79-84 -->

[`onConversionDataSuccess()`] is called on **every** app launch.
If `is_first_launch` field in [`conversionData`] equals `true`, this is the very first launch.

[`conversionData`]: https://dev.appsflyer.com/hc/docs/ios-legacy-apis#input-parameters
[`onConversionDataSuccess()`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate#onconversiondatasuccess

# Handle attribution failures

<!-- swift@88-90 -->

If the attribution operations fails (*from the SDK side*), [`onConversionDataFail()`] is called. Implement here error handling and reporting 

[`onConversionDataFail()`]: https://dev.appsflyer.com/hc/docs/appsflyerlibdelegate#onconversiondatafail