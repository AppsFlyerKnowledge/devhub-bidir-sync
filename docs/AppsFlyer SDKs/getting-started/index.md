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
## Getting started with AppsFlyer SDKs

AppsFlyer empowers marketers and helps them make better decisions.

This is done by providing marketers with powerful tools that solve real pain points. These include cross-platform attribution, mobile and web analytics, deep linking, fraud detection, privacy management and preservation, and much more.

## Native SDKs and plugins

The AppsFlyer Software Development Kits (SDKs) and plugins are your primary way to tap into our ever-growing ecosystem of digital marketing services. By [integrating our SDKs](https://dev.appsflyer.com/hc/docs/getting-started#integration-guides), you get [attribution](https://support.appsflyer.com/hc/en-us/articles/207447053) out-of-the-box. You also gain access to many more [SDK features](https://dev.appsflyer.com/hc/docs/getting-started#sdk-features).

<Callout icon="📘" theme="info">
  ### Note

  AppsFlyer supports only the HTTPS protocol for all incoming SDK traffic
</Callout>

### Native SDKs

The native SDKs utilize OS-specific APIs and optimizations to enable a better user experience. They contain the core functionality that also powers our [plugins](https://dev.appsflyer.com/hc/docs/getting-started#plugins), which are thin wrappers around the native SDKs.

<HTMLBlock>{`
<div class="button-container">
  <a class="button android" href="https://dev.appsflyer.com/hc/docs/android-sdk">Android SDK</a>
  <a class="button ios" href="https://dev.appsflyer.com/hc/docs/ios-sdk">iOS SDK</a>
</div>

<style>
  .button-container {
  	display: flex;
  }
  .button {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 150px;
	  border-radius: 6px;
    padding: 8px;
    margin-right: 4px;
	}
  
  .button:before {
  	margin-right: 4px;
  }

  .button.android {
    border: solid 2px #3DDC84;
  }

  .button.ios {
  	border-radius: 6px;
    padding: 8px;
    border: solid 2px #7D7D7D;
  }

  .ios:before {
        content: url("https://files.readme.io/19fdc72-apple-icon.svg");
  }

  .android:before {
        content: url("https://files.readme.io/d7dc5a3-android-icon.svg");
  }



.unity:before {
    content: url("https://files.readme.io/59acdf6-unity-icon.svg");
}

.reactnative:before {
   content: url("https://files.readme.io/3e1288d-reactnative-icon.svg");
}

.flutter:before {
    content: url("https://files.readme.io/1f70175-flutter-icon.svg");
}
</style>
`}</HTMLBlock>

### Plugins

Plugins are SDKs for cross-platform frameworks. They wrap around the native SDKs and use them under the hood, exposing their functionality to cross-platform applications.

<div class="button-container">
  <a class="button unity" href="https://dev.appsflyer.com/hc/docs/unity-plugin">Unity </a>
  <a class="button reactnative" href="https://github.com/AppsFlyerSDK/appsflyer-react-native-plugin">React Native</a>
  <a class="button flutter" href="https://github.com/af-dbram/appsflyer-flutter-plugin">Flutter </a>
</div>

## Integration guides

<HTMLBlock>{`
<div class="button-container">
  <a class="button install" href="https://dev.appsflyer.com/hc/docs/sdk-installation">SDK installation</a>
  <a class="button initialize" href="https://dev.appsflyer.com/hc/docs/sdk-integration">SDK integration</a>
</div>

<style>
  .button-container {
  	display: flex;
  }
  .button {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 150px;
	  border-radius: 6px;
    border: solid 2px;
    border-color: var(--project-primary-color);
    padding: 16px;
    margin-right: 4px;
	}
</style>
`}</HTMLBlock>

## SDK features

<HTMLBlock>{`
<div class="button-container">
  <a class="button inappevents" href="https://dev.appsflyer.com/hc/docs/in-app-events-sdk">In-app events</a>
  <a class="button conversiondata" href="https://dev.appsflyer.com/hc/docs/conversion-data">Conversion data</a>
</div>

<style>
  .button-container {
  	display: flex;
  }
  .button {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 150px;
	  border-radius: 6px;
    border: solid 2px;
    border-color: var(--project-primary-color);
    padding: 16px;
    margin-right: 4px;
	}
</style>
`}</HTMLBlock>

## SDK references

<HTMLBlock>{`
<div class="button-container">
  <a class="button android" href="https://dev.appsflyer.com/hc/docs/android-sdk-reference">Android SDK reference</a>
  <a class="button ios" href="https://dev.appsflyer.com/hc/docs/ios-sdk-reference">iOS SDK reference</a>
</div>

<style>
  .button-container {
  	display: flex;
  }
  .button {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 150px;
	  border-radius: 6px;
    border: solid 2px;
    border-color: var(--project-primary-color);
    padding: 16px;
    margin-right: 4px;
	}
  
</style>
`}</HTMLBlock>

<br />
