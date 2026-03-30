---
title: Unified Deep Linking (UDL) API in iOS
description: >-
  Step-by-step guidance for easy integration of Unified Deep Linking in an iOS
  application
hidden: false
recipe:
  color: '#cef2d1'
  icon: 🍏
---
```swift AppDelegate
import UIKit
import AppsFlyerLib

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    
    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        // 1 - Get AppsFlyer preferences from .plist file
        guard let propertiesPath = Bundle.main.path(forResource: "afdevkey_donotpush", ofType: "plist"),
            let properties = NSDictionary(contentsOfFile: propertiesPath) as? [String:String] else {
                fatalError("Cannot find `afdevkey_donotpush`")
        }
        guard let appsFlyerDevKey = properties["appsFlyerDevKey"],
                   let appleAppID = properties["appleAppID"] else {
            fatalError("Cannot find `appsFlyerDevKey` or `appleAppID` key")
        }
        // 2 - Replace 'appsFlyerDevKey', `appleAppID` with your DevKey, Apple App ID
        AppsFlyerLib.shared().appsFlyerDevKey = appsFlyerDevKey
        AppsFlyerLib.shared().appleAppID = appleAppID
        //  Set isDebug to true to see AppsFlyer debug logs
        AppsFlyerLib.shared().isDebug = true
        
        AppsFlyerLib.shared().delegate = self
        AppsFlyerLib.shared().deepLinkDelegate = self
        
        // 3 - Subscribe to didBecomeActiveNotification if you use SceneDelegate or just call
        // -[AppsFlyerTracker trackAppLaunch] from -[AppDelegate applicationDidBecomeActive:]
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
    
    // Open Universal Links
    
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
    
    // Report Push Notification attribution data for re-engagements
    func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
        AppsFlyerLib.shared().handlePushNotification(userInfo)
    }
    
    // User logic
    fileprivate func walkToSceneWithParams(fruitName: String, deepLinkData: [String: Any]?) {
        let storyBoard: UIStoryboard = UIStoryboard(name: "Main", bundle: nil)
        UIApplication.shared.windows.first?.rootViewController?.dismiss(animated: true, completion: nil)
               
        let destVC = fruitName + "_vc"
        if let newVC = storyBoard.instantiateVC(withIdentifier: destVC) {
            
            NSLog("[AFSDK] AppsFlyer routing to section: \(destVC)")
            newVC.deepLinkData = deepLinkData
            
             UIApplication.shared.windows.first?.rootViewController?.present(newVC, animated: true, completion: nil)
        } else {
            NSLog("[AFSDK] AppsFlyer: could not find section: \(destVC)")
        }
    }
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

extension UIStoryboard {
    func instantiateVC(withIdentifier identifier: String) -> DLViewController? {
        // "identifierToNibNameMap" – dont change it. It is a key for searching IDs
        if let identifiersList = self.value(forKey: "identifierToNibNameMap") as? [String: Any] {
            if identifiersList[identifier] != nil {
                return self.instantiateViewController(withIdentifier: identifier) as? DLViewController
            }
        }
        return nil
    }
}
```

```swift SceneDelegate
import UIKit
import AppsFlyerLib


class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?
           
    func scene(_ scene: UIScene, continue userActivity: NSUserActivity) {
        // Universal Link - Background -> foreground
        AppsFlyerLib.shared().continue(userActivity, restorationHandler: nil)
    }
    
    func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        // URI scheme - Background -> foreground
        if let url = URLContexts.first?.url {
            AppsFlyerLib.shared().handleOpen(url, options: nil)
        }
    } 

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        // URI scheme - killed -> foreground
        
        // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
        // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
        // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).

        // Processing Universal Link from the killed state
        if let userActivity = connectionOptions.userActivities.first {
            AppsFlyerLib.shared().continue(userActivity, restorationHandler: nil)
        } else if let url = connectionOptions.urlContexts.first?.url {
            AppsFlyerLib.shared().handleOpen(url, options: nil)
        }
        guard let _ = (scene as? UIWindowScene) else { return }
    }

    func sceneDidDisconnect(_ scene: UIScene) {
        // Called as the scene is being released by the system.
        // This occurs shortly after the scene enters the background, or when its session is discarded.
        // Release any resources associated with this scene that can be re-created the next time the scene connects.
        // The scene may re-connect later, as its session was not neccessarily discarded (see `application:didDiscardSceneSessions` instead).
    }

    func sceneDidBecomeActive(_ scene: UIScene) {
        // Called when the scene has moved from an inactive state to an active state.
        // Use this method to restart any tasks that were paused (or not yet started) when the scene was inactive.
    }

    func sceneWillResignActive(_ scene: UIScene) {
        // Called when the scene will move from an active state to an inactive state.
        // This may occur due to temporary interruptions (ex. an incoming phone call).
    }

    func sceneWillEnterForeground(_ scene: UIScene) {
        // Called as the scene transitions from the background to the foreground.
        // Use this method to undo the changes made on entering the background.
    }

    func sceneDidEnterBackground(_ scene: UIScene) {
        // Called as the scene transitions from the foreground to the background.
        // Use this method to save data, release shared resources, and store enough scene-specific state information
        // to restore the scene back to its current state.
    }


}


```

# Set up your app for deep linking



View the [guide](https://dev.appsflyer.com/hc/docs/initial-setup-2) for app-opening and deep linking setup.

# Import AppsFlyer SDK library

<!-- swift@2 -->



# Init the SDK

<!-- swift@21-22 -->

Initialize the SDK with AppsFlyer dev key and the app's Apple ID.

Instruction on how to obtain the *dev key* can be found [here](https://support.appsflyer.com/hc/en-us/articles/207032066-iOS-SDK-V6-X-integration-guide-for-developers#integration-31-retrieve-your-appsflyer-dev-key).

# Register the DeepLinkDelegate

<!-- swift@27 -->

[`DeepLinkDelegate`] holds the call back function for UDL API.

`AppsFlyerLib.shared().deepLinkDelegate` is initialized with `self` since [`DeepLinkDelegate`] is created as an extension of `AppDelegate`.

 [`DeepLinkDelegate`]: https://dev.appsflyer.com/hc/docs/deeplinkdelegate

# Create application function for Universal Link

<!-- swift@48-51 -->

When app opens via Universal Link this `application()` will be called, and the `userActivity` should be passed to AppsFlyer SDK with a call to ```AppsFlyerLib.shared().continue(userActivity, restorationHandler: nil)```

# Create application function for URI scheme

<!-- swift@54-57 -->

When app opens via URI scheme this application() will be called, and the `URL` and   `launch options` should be passed to AppsFlyer SDK with a call to:
```
AppsFlyerLib.shared().handleOpen(url, options: options)
```

# [Optional] Add deep linking support in SceneDelegate

<!-- swift@9-36 -->

> Go to *SceneDelegate* tab

Universal Link is supported in this `scene` function 
```
func scene(_ scene: UIScene, continue userActivity: NSUserActivity)
```

URI scheme is supported in this `scene` function 
```
scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>)
```

In this `scene` function implement logic to support processing deep linking from killed state

# Create DeepLinkDelegate

<!-- swift@82 -->

> Go back to *AppDelegate* tab

[`DeepLinkDelegate`] created as an extension for `AppDelegate`.
[`DeepLinkDelegate`] holds the call back function for UDL API

[`DeepLinkResult.status`]: https://dev.appsflyer.com/hc/docs/deeplinkresult-1

 [`DeepLinkDelegate`]: https://dev.appsflyer.com/hc/docs/deeplinkdelegate

# Implement didResolveDeepLink()

<!-- swift@84 -->

Override [`onDeepLinking()`] in the [`DeepLinkListener`] object.

[`didResolveDeepLink()`] is called when a UDL API gets a result from AppsFlyer servers.
The result is summarized in a [`DeepLinkResult`] object that is passed back to the [`didResolveDeepLink()`].

[`didResolveDeepLink()`]: https://dev.appsflyer.com/hc/docs/deeplinkdelegate#didresolvedeeplink
[`DeepLinkResult`]: https://dev.appsflyer.com/hc/docs/deeplinkresult-1

# Verify UDL status

<!-- swift@85-95 -->

Verify [`status`] of the UDL API query: 
- `Found` allows you to continue the deep linking flow.
- `Not Found` ends the deep linking flow
- `Error` should trigger your error reporting.

[`Status`]: https://dev.appsflyer.com/hc/docs/deeplinkresult-1#status

# Get the deep linking data

<!-- swift@96-99 -->

Use [`deepLink`] field in [`DeepLinkResult`] to retrieve the [`DeepLink`] object. This object contains the deep link data, accessible using predefined methods in the object.

[`deepLink`]: https://dev.appsflyer.com/hc/docs/deeplinkresult-1#deeplink
[`DeepLinkResult`]: https://dev.appsflyer.com/hc/docs/deeplinkresult-1
[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink-1

# Check for deferred deep linking flow

<!-- swift@112-117 -->

[`isDeferred`] field in [`DeepLink`] signals if this flow is a deferred deep linking flow.

Deferred deep linking is when the app isn't installed when the user clicks the OneLink URL, and deep linking data is retrieved after the user installs and opens the app.

[`isDeferred`]: https://dev.appsflyer.com/hc/docs/deeplink-1#isdeferred
[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink-1

# Use deep_link_value for in-app routing

<!-- swift@119 -->

A user can be immediately routed to an activity inside the app based on the `deep_link_value` associated with the link the user clicked.

[`deeplinkValue`] field in [`DeepLink`] holds  the `deep_link_value`

[`deeplinkValue`]: https://dev.appsflyer.com/hc/docs/deeplink-1#deeplinkvalue
[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink-1

# Implement in-app routing

<!-- swift@120,65-79 -->

In this example we implemented in-app routing in the method `walkToSceneWithParams`.

This in-app router receives the fruit names in the `deep_link_value` and routes the user to the correct fruit page (*apples*, *bananas*, or *peaches*).

Together with the fruit name, we pass the [`DeepLink`] object to the fruit page just in case we need more data for a better user experience.

[`DeepLink`]: https://dev.appsflyer.com/hc/docs/deeplink-1