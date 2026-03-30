---
title: Integrate SDK
excerpt: Learn how to initialize and start the Android SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: link
      title: In-app events
      url: https://dev.appsflyer.com/hc/docs/in-app-events-android
    - type: link
      title: Deep linking
      url: https://dev.appsflyer.com/hc/docs/android
    - type: link
      title: Conversion data
      url: https://dev.appsflyer.com/hc/docs/conversion-data-android
---
Before you begin
----------------

- You must [install the Android SDK](doc:install-android-sdk).
- Ensure that in your app `build.gradle` file, `applicationId`'s value (in the `defaultConfig` block) matches the app's app ID in AppsFlyer.
- Get the [AppsFlyer dev key](https://support.appsflyer.com/hc/en-us/articles/207032126#integration-2-integrating-the-sdk). It is required to successfully initialize the SDK.
- The codes in this document are example implementations. Make sure to change the `<AF_DEV_KEY>` and other placeholders as needed.
- All the steps in this document are mandatory unless stated otherwise.

Initializing the Android SDK
----------------------------


[block:tutorial-tile]
{
  "backgroundColor": "#0197f4",
  "emoji": "🥇",
  "id": "63d046543a8a2b003c3360b1",
  "link": "https://dev.appsflyer.com/v0.1/recipes/starting-the-sdk-in-android",
  "slug": "starting-the-sdk-in-android",
  "title": "Starting the SDK in Android"
}
[/block]


It's recommended to initialize the SDK in the [global Application class/subclass]. That is to ensure the SDK can start in any scenario (for example, deep linking).

**Step 1: Import AppsFlyerLib**  
In your global Application class, import [`AppsFlyerLib`](doc:android-sdk-reference-appsflyerlib):

```java Java
import com.appsflyer.AppsFlyerLib;
```
```kotlin Kotlin
import com.appsflyer.AppsFlyerLib
```

**Step 2: Initialize the SDK**  
In the global Application `onCreate`, call [`init`](doc:android-sdk-reference-appsflyerlib#init) with the following arguments:

```java
AppsFlyerLib.getInstance().init(<AF_DEV_KEY>, null, this);
```
```kotlin
AppsFlyerLib.getInstance().init(<AF_DEV_KEY>, null, this)
```

1. The first argument is your AppsFlyer dev key.
2. The second argument is a Nullable [`AppsFlyerConversionListener`](doc:android-sdk-reference-appsflyerconversionlistener). If you don't need conversion data, we recommend passing a `null` as the second argument. For more information, see [Conversion data](doc:conversion-data-android).
3. The third argument is the Application Context.

Starting the Android SDK
------------------------

In the Application's `onCreate` method, after calling [`init`](doc:android-sdk-reference-appsflyerlib#init), call [`start`](doc:android-sdk-reference-appsflyerlib#start) and pass it the Application's Context as the first argument:

```java
AppsFlyerLib.getInstance().start(this);
```
```kotlin
AppsFlyerLib.getInstance().start(this)
```

### Deferring SDK start

<span class="annotation-optional">Optional</span>  
You can defer the SDK initialization by calling [`start`](doc:android-sdk-reference-appsflyerlib#start) from an Activity class, instead of calling it in the Application class. [`init`](doc:android-sdk-reference-appsflyerlib#init) should still be called in the Application class.

Typical usage of deferred SDK start is when an app would like to request consent from the user to collect data in the Main Activity, and call [`start`](doc:android-sdk-reference-appsflyerlib#start) after getting the user's consent.

**Note**: If the app calls `start` from an Activity, it should pass the Activity Context to the SDK.



### Starting with a response listener

To receive confirmation that the SDK was started successfully, create an `AppsFlyerRequestListener` object and pass it as the third argument of `start`:

```java
AppsFlyerLib.getInstance().start(getApplicationContext(), <YOUR_DEV_KEY>, new AppsFlyerRequestListener() {
  @Override
  public void onSuccess() {
    Log.d(LOG_TAG, "Launch sent successfully, got 200 response code from server");
  }
  
  @Override
  public void onError(int i, @NonNull String s) {
    Log.d(LOG_TAG, "Launch failed to be sent:\n" +
          "Error code: " + i + "\n"
          + "Error description: " + s);
  }
});
```
```kotlin
AppsFlyerLib.getInstance().start(this, <YOUR_DEV_KEY>, object : AppsFlyerRequestListener {
  override fun onSuccess() {
    Log.d(LOG_TAG, "Launch sent successfully")
    }
  
  override fun onError(errorCode: Int, errorDesc: String) {
    Log.d(LOG_TAG, "Launch failed to be sent:\n" +
          "Error code: " + errorCode + "\n"
          + "Error description: " + errorDesc)
    }
})
```

- The `onSuccess()` callback method is invoked for every `200` response to an attribution request made by the SDK.
- The `onError(String error)` callback method is invoked for any other response and returns the response as the error string.

Full example
------------

The following example demonstrates how to initialize and start the SDK from the Application class.

```java
import android.app.Application;
import com.appsflyer.AppsFlyerLib;
// ...
public class AFApplication extends Application {
    // ...
    @Override
    public void onCreate() {
        super.onCreate();
        // ...
        AppsFlyerLib.getInstance().init(<AF_DEV_KEY>, null, this);
        AppsFlyerLib.getInstance().start(this);
        // ...
    }
    // ...
}
```
```kotlin
import android.app.Application
import com.appsflyer.AppsFlyerLib
// ...
class AFApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        // ...
        AppsFlyerLib.getInstance().init(<AF_DEV_KEY>, null, this)
        AppsFlyerLib.getInstance().start(this)
        // ...
    }
    // ...
}
```

[Github link](https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/80763ef8c93c49b1f0226455ae35d089f7968ede/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/AppsflyerBasicApp.java#L144-L145)

Enabling debug mode
-------------------

<span class="annotation-optional">Optional</span>  
You can enable debug logs by calling [`setDebugLog`](doc:android-sdk-reference-appsflyerlib#setdebuglog):

```java
AppsFlyerLib.getInstance().setDebugLog(true);
```
```kotlin
AppsFlyerLib.getInstance().setDebugLog(true)
```

> 📘 Note
> 
> To see full debug logs, make sure to call `setDebugLog` before invoking other SDK methods.
> 
> See [example](https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/d3d0d9dcf1c1dcb2f873f5b50708fc4fa24a7868/java/basic_app/app/src/main/java/com/appsflyer/onelink/appsflyeronelinkbasicapp/AppsflyerBasicApp.java#L28).

> 🚧 Warning
> 
> To avoid leaking sensitive information, make sure debug logs are disabled before distributing the app.

Testing the integration
-----------------------

<span class="annotation-optional">Optional</span>  
For detailed integration testing instructions, see the [Android SDK integration testing guide](doc:testing-android).

[global Application class/subclass]: https://developer.android.com/reference/android/app/Application