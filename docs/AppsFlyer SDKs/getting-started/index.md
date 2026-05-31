---
title: Getting started
excerpt: Getting started with the AppsFlyer SDKs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Getting started with AppsFlyer SDKs
-----------------------------------

AppsFlyer empowers marketers and helps them make better decisions.

This is done by providing marketers with powerful tools that solve real pain points. These include cross-platform attribution, mobile and web analytics, deep linking, fraud detection, privacy management and preservation, and much more.

Native SDKs and plugins
-----------------------

The AppsFlyer Software Development Kits (SDKs) and plugins are your primary way to tap into our ever-growing ecosystem of digital marketing services. By [integrating our SDKs](https://dev.appsflyer.com/hc/docs/getting-started#integration-guides), you get [attribution](https://support.appsflyer.com/hc/en-us/articles/207447053) out-of-the-box. You also gain access to many more [SDK features](https://dev.appsflyer.com/hc/docs/getting-started#sdk-features).

> 📘 Note
> 
> AppsFlyer supports only the HTTPS protocol for all incoming SDK traffic

### Native SDKs

The native SDKs utilize OS-specific APIs and optimizations to enable a better user experience. They contain the core functionality that also powers our [plugins](https://dev.appsflyer.com/hc/docs/getting-started#plugins), which are thin wrappers around the native SDKs.

<style> .button-container { display: flex; max-width:800px; } .button { display: flex; justify-content: center; align-items: center; min-width: 200px; border-radius: 6px; padding: 8px; margin-right: 4px; } .button:before { margin-right: 4px; } .button { border-radius: 6px; padding: 8px; border: solid 2px #434446; } /** .button.android { border: solid 2px #3DDC84; } .button.reactnative { border: solid 2px #FF8C00; } .button.ios { border-radius: 6px; padding: 8px; border: solid 2px #7D7D7D; } .button.unity { border: solid 2px #3DDC84; border-color: var(--project-primary-color); }**/ .ios:before { content: url("https://files.readme.io/19fdc72-apple-icon.svg"); } .android:before { content: url("https://files.readme.io/d7dc5a3-android-icon.svg"); } .unity:before { content: url("https://files.readme.io/59acdf6-unity-icon.svg"); } .flutter:before { content: url("https://files.readme.io/1f70175-flutter-icon.svg"); } .cordova:before { content: url("https://files.readme.io/5f757d6-apache_cordova-icon.svg"); } .capacitor:before { content: url("https://files.readme.io/ad0d405-capacitor-icon.svg"); } .reactnative:before { content: url("https://files.readme.io/3e1288d-reactnative-icon.svg"); } a[href*=http]:not([href*="dev.appsflyer.com"]):not(.landing-page__social):after { display:none !important; } </style>
Android SDK iOS SDK Unity SDK React Native SDK
Flutter Cordova Capacitor Cocos2dx

### Plugins

Plugins are SDKs for cross-platform frameworks. They wrap around the native SDKs and use them under the hood, exposing their functionality to cross-platform applications.

<div class="button-container">
  <a class="button unity" href="https://dev.appsflyer.com/hc/docs/unity-plugin">Unity</a>
  <a class="button reactnative" href="https://github.com/AppsFlyerSDK/appsflyer-react-native-plugin">React Native</a>
  <a class="button flutter" href="https://github.com/af-dbram/appsflyer-flutter-plugin">Flutter</a>
</div>

Integration guides
------------------

[block:html]
{
  "html": "<div class=\"button-container\">\n  <a class=\"button install\" href=\"https://dev.appsflyer.com/hc/docs/sdk-installation\">SDK installation</a>\n  <a class=\"button initialize\" href=\"https://dev.appsflyer.com/hc/docs/sdk-integration\">SDK integration</a>\n</div>\n\n<style>\n  .button-container {\n  \tdisplay: flex;\n  }\n  .button {\n    display: flex;\n    justify-content: center;\n    align-items: center;\n    width: 150px;\n\t  border-radius: 6px;\n    border: solid 2px;\n    border-color: var(--project-primary-color);\n    padding: 16px;\n    margin-right: 4px;\n\t}\n</style>"
}
[/block]

SDK features
------------

[block:html]
{
  "html": "<div class=\"button-container\">\n  <a class=\"button inappevents\" href=\"https://dev.appsflyer.com/hc/docs/in-app-events-sdk\">In-app events</a>\n  <a class=\"button conversiondata\" href=\"https://dev.appsflyer.com/hc/docs/conversion-data\">Conversion data</a>\n</div>\n\n<style>\n  .button-container {\n  \tdisplay: flex;\n  }\n  .button {\n    display: flex;\n    justify-content: center;\n    align-items: center;\n    width: 150px;\n\t  border-radius: 6px;\n    border: solid 2px;\n    border-color: var(--project-primary-color);\n    padding: 16px;\n    margin-right: 4px;\n\t}\n</style>"
}
[/block]

SDK references
--------------

[block:html]
{
  "html": "<div class=\"button-container\">\n  <a class=\"button android\" href=\"https://dev.appsflyer.com/hc/docs/android-sdk-reference\">Android SDK reference</a>\n  <a class=\"button ios\" href=\"https://dev.appsflyer.com/hc/docs/ios-sdk-reference\">iOS SDK reference</a>\n</div>\n\n<style>\n  .button-container {\n  \tdisplay: flex;\n  }\n  .button {\n    display: flex;\n    justify-content: center;\n    align-items: center;\n    width: 150px;\n\t  border-radius: 6px;\n    border: solid 2px;\n    border-color: var(--project-primary-color);\n    padding: 16px;\n    margin-right: 4px;\n\t}\n  \n</style>"
}
[/block]