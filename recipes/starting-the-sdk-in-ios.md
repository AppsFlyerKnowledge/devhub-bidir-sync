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
//Optional
import AppTrackingTransparency


@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    
    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        
        AppsFlyerLib.shared().appsFlyerDevKey = <YOUR DEV KEY>
        AppsFlyerLib.shared().appleAppID = <APP ID (without id prefix)>
        //  Set isDebug to true to see AppsFlyer debug logs
        AppsFlyerLib.shared().isDebug = true
      
        //Optional
        AppsFlyerLib.shared().waitForATTUserAuthorization(timeoutInterval: 60)
                
        NotificationCenter.default.addObserver(self,
        selector: #selector(didBecomeActiveNotification),
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

```

# Import AppsFlyer SDK lib

<!-- swift@2 -->



# Set dev key and app ID

<!-- swift@15-16 -->

>Happens in `didFinishLaunchingWithOptions`

Replace `appsFlyerDevKey`, `appleAppID` with your DevKey, Apple App ID

Instruction on how to obtain the *dev key* can be found [here](https://support.appsflyer.com/hc/en-us/articles/207032066-iOS-SDK-V6-X-integration-guide-for-developers#integration-31-retrieve-your-appsflyer-dev-key)

# [Optional] Show debug logs

<!-- swift@18 -->

Remember to remove in production

# [Optional] for SceneDelegate subscribe to didBecomeActiveNotification

<!-- swift@23-27 -->

`didBecomeActiveNotification` is the name of the function that holds the SDK `start` function

# Start the SDK

<!-- swift@33 -->

Running `start` inside `didBecomeActiveNotification`
makes sure it runs only when the application is active

# [Optional] Import the AppTrackingTransparency library

<!-- swift@4 -->



# [Optional] Set ATT timeout

<!-- swift@21 -->

The timeout passed `waitForATTUserAuthorization` determines how much time the SDK waits for a user input to the ATT prompt

# [Optional] Present ATT prompt

<!-- swift@34-49 -->

