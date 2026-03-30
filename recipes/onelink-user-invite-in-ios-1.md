---
title: OneLink user invite in iOS
description: >-
  Create OneLink custom link for user-invite and copy to clipboard when link is
  ready
hidden: false
recipe:
  color: '#01f44a'
  icon: ✍️
---
```swift AppDelegate
//
//  AppDelegate.swift
//  basic_app
//
//  Created by Liaz Kamper on 11/05/2020.
//  Copyright © 2020 OneLink. All rights reserved.
//

import UIKit
import AppsFlyerLib
import AppTrackingTransparency

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    var ConversionData: [AnyHashable: Any]? = nil
    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        // 1 - Get AppsFlyer preferences from .plist file
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
        AppsFlyerLib.shared().deepLinkDelegate = self
        
        //set the OneLink template id for share invite links
        AppsFlyerLib.shared().appInviteOneLinkID = "H5hv"
        
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
    fileprivate func walkToSceneWithParams(fruitName: String, deepLinkObj: DeepLink) {
        let storyBoard: UIStoryboard = UIStoryboard(name: "Main", bundle: nil)
        UIApplication.shared.windows.first?.rootViewController?.dismiss(animated: true, completion: nil)
               
        let destVC = fruitName + "_vc"
        if let newVC = storyBoard.instantiateVC(withIdentifier: destVC) {
            
            NSLog("[AFSDK] AppsFlyer routing to section: \(destVC)")
            newVC.deepLinkData = deepLinkObj
            
             UIApplication.shared.windows.first?.rootViewController?.present(newVC, animated: true, completion: nil)
        } else {
            NSLog("[AFSDK] AppsFlyer: could not find section: \(destVC)")
        }
    }
}

extension AppDelegate: DeepLinkDelegate {
     
    func didResolveDeepLink(_ result: DeepLinkResult) {
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
        } else {
            NSLog("[AFSDK] This is a direct deep link")
        }
        
        guard let fruitNameStr = deepLinkObj.deeplinkValue else {
            print("Could not extract deep_link_value from deep link object")
            return
        }      
        
        walkToSceneWithParams(fruitName: fruitNameStr, deepLinkObj: deepLinkObj)
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

```swift ViewController
import UIKit
import AppsFlyerLib

class DLViewController: UIViewController {
    
    var deepLinkData: DeepLink? = nil

    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    func attributionDataToString(data : [String: Any]) -> NSMutableAttributedString {
        let newString = NSMutableAttributedString()
        let boldAttribute = [
           NSAttributedString.Key.font: UIFont(name: "Avenir Next Bold", size: 18.0)!
        ]
        let regularAttribute = [
           NSAttributedString.Key.font: UIFont(name: "Avenir Next", size: 18.0)!
        ]
        let sortedKeys = Array(data.keys).sorted(by: <)
        for key in sortedKeys {
            print("ViewController", key, ":",data[key] ?? "null")
            let keyStr = key
            let boldKeyStr = NSAttributedString(string: keyStr, attributes: boldAttribute)
            newString.append(boldKeyStr)
            
            var valueStr: String
            switch data[key] {
            case let s as String:
                valueStr = s
            case let b as Bool:
                valueStr = b.description
            default:
                valueStr = "null"
            }
            
            let normalValueStr = NSAttributedString(string: ": \(valueStr)\n", attributes: regularAttribute)
            newString.append(normalValueStr)
        }
        return newString
    }
    
    func showToast(message : String, font: UIFont) {

        let toastLabel = UILabel(frame: CGRect(x: self.view.frame.size.width/2 - 75, y: 20, width: 150, height: 35))
        toastLabel.backgroundColor = UIColor.black.withAlphaComponent(0.6)
        toastLabel.textColor = UIColor.white
        toastLabel.font = font
        toastLabel.textAlignment = .center;
        toastLabel.text = message
        toastLabel.alpha = 1.0
        toastLabel.layer.cornerRadius = 10;
        toastLabel.clipsToBounds  =  true
        self.view.addSubview(toastLabel)
        UIView.animate(withDuration: 4.0, delay: 0.1, options: .curveEaseOut, animations: {
             toastLabel.alpha = 0.0
        }, completion: {(isCompleted) in
            toastLabel.removeFromSuperview()
        })
    }
    
    func copyShareInviteLink(fruitName: String){
        AppsFlyerShareInviteHelper.generateInviteUrl(linkGenerator:
         {(_ generator: AppsFlyerLinkGenerator) -> AppsFlyerLinkGenerator in
            generator.addParameterValue(fruitName, forKey: "deep_link_value")
            generator.addParameterValue(self.fruitAmountStr, forKey: "deep_link_sub1")
            generator.addParameterValue("THIS_USER_ID", forKey: "deep_link_sub2")
            generator.setCampaign("summer_fruits")
            generator.setChannel("mobile_share")
          return generator },
        completionHandler: {(_ url: URL?) -> Void in
            if url != nil{
               NSLog("[AFSDK] AppsFlyer share-invite link: \(url)")
            }
            else{
                NSLog("url is nil")
            }
        })
    }
}

```

```json Response Example
{"success":true}
```

# Set OneLink template

<!-- swift@43 -->

>  In the AppDelegate tab

Set the OneLink template for the OneLink user invite.
The template is provided by the marketer

# Import AppsFlyer library

<!-- swift@2 -->

> In the ViewController tab

# Create LinkGenerator object

<!-- swift@63 -->

> In the ViewController tab 

Using [`ShareInviteHelper`] create a  [`LinkGenerator`] instance and register a completion handler

[`ShareInviteHelper`]: https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyershareinvitehelper
[`LinkGenerator`]: https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyerlinkgenerator

# Set OneLink custom parameters like `deep_link_value`

<!-- swift@65-67 -->

> Goto ViewController tab

Use [`addParameterValue()`]

[`addParameterValue()`]: 
https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyerlinkgenerator#addparametervalue

# Set OneLink attribution parameters

<!-- swift@68-69 -->

> In the ViewController tab

Use native functions (e.g. [`setCampaign`] and [`setChannel`]) to set OneLink parameters (.e.g `campaign` and `channel`)

[`setCampaign`]: 
https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyerlinkgenerator#setcampaign
[`setChannel`]: 
https://dev.appsflyer.com/hc/docs/ios-sdk-reference-appsflyerlinkgenerator#setchannel

# Create a completion handler to receive the user-invite link async

<!-- swift@71-79 -->

> In the ViewController tab

Use `completionHandler` to register the async responses for successful or failed link creation

# Successful link creation async response

<!-- swift@72 -->

> In the ViewController tab

`url` is not `nil` when the user-invite is created successfully.
In this example the link is copied to the clipboard

# Failed link creation async response

<!-- swift@76 -->

> In the ViewController tab

`url` is `nil` when the user-invite failed to create