---
title: SDK 7 Immigration guide
deprecated: false
hidden: false
metadata:
  robots: index
---
# Migrate Android SDK to V7

This guide walks you through migrating your Android app from AppsFlyer SDK 6 to SDK 7. SDK 7 introduces a new session control model, removes several deprecated APIs, and aligns Android behavior with iOS. Use this guide to identify what you need to change, understand why each change was made, and update your integration in the correct order.

> 📘 **Note**
>
> The vast majority of `AppsFlyerLib` methods remain unchanged between SDK 6 and SDK 7. You don't need to rewrite your integration from scratch. This guide covers only what you need to change.

---

## Before you begin

### SDK 6 support policy

SDK 6 continues to be supported, but only for critical fixes. All new features are planned for SDK 7 only. We recommend migrating as soon as possible to stay current with new capabilities.

### Kotlin version requirement

If your app uses Kotlin 1.9, you may encounter metadata errors when building against the SDK 7 AAR. Update to Kotlin 2.0 or higher before migrating.

### Minimum SDK version

SDK 7 raises the minimum Android API level from 19 to 21. Update your `build.gradle` before proceeding:

```
// SDK 6
minSdk 19

// SDK 7
minSdk 21
```

Most apps are already above API 21, as many Google libraries require API 25 or higher.

### Article scope

This article covers Android only. An iOS migration guide will be published separately.

---

## The SDK 7 session model

The core theme of SDK 7 is giving you control over when to send the first session, and any subsequent session. In SDK 6, the SDK sent the session automatically when the app came to the foreground. In SDK 7, that responsibility moves to you as the developer.

This change reflects a real-world need: many apps must complete steps before sending a launch event to AppsFlyer, for example collecting user consent, retrieving a customer user ID (CUID), or completing ATT authorization. SDK 7 is designed around this requirement.

SDK 7 also addresses long-standing inconsistencies between iOS and Android. The most significant behavioral alignment is setter persistence: in SDK 6, many `AppsFlyerLib` setter values were persisted to disk on Android and survived process restarts. In SDK 7, Android aligns with iOS, meaning all setter values are runtime-only and must be re-applied on every cold start.

---

## Part 1: Breaking changes

The following changes cause compile errors if not addressed. Work through them in the order below, as some steps depend on others.

### 1. Update imports to `com.appsflyer.share.*`

All user-facing public classes have moved from their previous packages to `com.appsflyer.share`. Your code won't compile until you update these imports.

| SDK 6 import | SDK 7 import |
|---|---|
| `com.appsflyer.AppsFlyerConversionListener` | `com.appsflyer.share.AppsFlyerConversionListener` |
| `com.appsflyer.attribution.AppsFlyerRequestListener` | `com.appsflyer.share.attribution.AppsFlyerRequestListener` |
| `com.appsflyer.attribution.RequestError` | `com.appsflyer.share.attribution.RequestError` |
| `com.appsflyer.deeplink.DeepLinkListener` | `com.appsflyer.share.deeplink.DeepLinkListener` |
| `com.appsflyer.deeplink.DeepLinkResult` | `com.appsflyer.share.deeplink.DeepLinkResult` |
| `com.appsflyer.internal.platform_extension.PluginInfo` | `com.appsflyer.share.platform_extension.PluginInfo` |
| `com.appsflyer.AFAdRevenueData`, `AFInAppEventType`, `AFInAppEventParameterName`, `AFPurchaseDetails`, `AdRevenueScheme`, `MediationNetwork`, `AppsFlyerConsent`, and others | `com.appsflyer.share.*` (same class names, new package) |

> 📘 **Note**
>
> Android Studio can resolve these import changes automatically. Remove the old import and let the IDE suggest the correct replacement from `com.appsflyer.share`.

---

### 2. Update the `start()` method

The `start()` overloads that accepted a `Context` or a dev key have been removed. The dev key and context are already provided to `init()` and don't need to be passed again.

SDK 7 supports only two `start()` signatures: one with no arguments, and one with an `AppsFlyerRequestListener`.

Java

```java
// SDK 6
AppsFlyerLib.getInstance().start(this);
AppsFlyerLib.getInstance().start(this, devKey);
AppsFlyerLib.getInstance().start(this, devKey, requestListener);

// SDK 7
AppsFlyerLib.getInstance().start();
AppsFlyerLib.getInstance().start(requestListener);
```

Kotlin

```kotlin
// SDK 6
AppsFlyerLib.getInstance().start(this)
AppsFlyerLib.getInstance().start(this, devKey)
AppsFlyerLib.getInstance().start(this, devKey, requestListener)

// SDK 7
AppsFlyerLib.getInstance().start()
AppsFlyerLib.getInstance().start(requestListener)
```

---

### 3. Add `registerSessionReadyListener`

This is the most significant change in SDK 7. You must call `registerSessionReadyListener` before calling `start()`. If you call `start()` without a registered listener, the SDK logs a warning and does not start the session.

The listener fires when the SDK has completed its internal checks and is ready for the session to begin. From that point, it's up to you to decide when to actually call `start()`, based on your app's own requirements.

Two patterns are available depending on your app's needs.

#### Simplified approach

Use this pattern if your app has no pre-start conditions, meaning you don't need to wait for consent, a CUID, or any other gate before sending the first session.

Java

```java
AppsFlyerLib.getInstance().init(devKey, conversionListener, applicationContext);

AppsFlyerLib.getInstance().registerSessionReadyListener(() -> {
    AppsFlyerLib.getInstance().start();
});

// Optional: unregister when tearing down (for example in Activity.onDestroy())
// AppsFlyerLib.getInstance().unregisterSessionReadyListener();

boolean ready = AppsFlyerLib.getInstance().isSessionReady();
```

Kotlin

```kotlin
AppsFlyerLib.getInstance().init(devKey, conversionListener, applicationContext)

AppsFlyerLib.getInstance().registerSessionReadyListener {
    AppsFlyerLib.getInstance().start()
}

// Optional: unregister when tearing down (for example in Activity.onDestroy())
// AppsFlyerLib.getInstance().unregisterSessionReadyListener()

val ready = AppsFlyerLib.getInstance().isSessionReady
```

#### Coordinator pattern for apps with pre-start conditions

Use this pattern if your app must satisfy conditions before sending the first session, for example collecting user consent or waiting for a CUID from your backend. The coordinator class synchronizes the SDK's readiness signal with your app's own readiness, and calls `start()` only when both conditions are met.

Java

```java
package com.yourapp;

import android.util.Log;
import com.appsflyer.AppsFlyerLib;
import com.appsflyer.share.attribution.AppsFlyerRequestListener;

public final class AfSdkStartupManager {
    // Note: the SessionReadyListener callback fires on a background thread.
    // Mark these flags volatile at minimum, or use synchronized access
    // if your consent flow also runs on a background thread.
    private volatile boolean isConsentReady;
    private volatile boolean isSdkReadyToStart;

    public void onConsentReady() {
        isConsentReady = true;
        startAfSdkIfAllConditionsAreMet();
    }

    public void onAfSdkReadyToStart() {
        isSdkReadyToStart = true;
        startAfSdkIfAllConditionsAreMet();
    }

    private void startAfSdkIfAllConditionsAreMet() {
        if (isConsentReady && isSdkReadyToStart) {
            AppsFlyerLib.getInstance().start(new AppsFlyerRequestListener() {
                @Override
                public void onSuccess() {
                    Log.d("AppsFlyer", "Session started successfully");
                }

                @Override
                public void onError(int code, String error) {
                    Log.d("AppsFlyer", "Session start error. Code: " + code + ", error: " + error);
                }
            });
            isSdkReadyToStart = false;
        }
    }

    public void reset() {
        isConsentReady = false;
        isSdkReadyToStart = false;
    }
}
```

Kotlin

```kotlin
package com.yourapp

import android.util.Log
import com.appsflyer.AppsFlyerLib
import com.appsflyer.share.attribution.AppsFlyerRequestListener

class AfSdkStartupManager {
    // Note: the SessionReadyListener callback fires on a background thread.
    // Mark these flags @Volatile at minimum, or use synchronized access
    // if your consent flow also runs on a background thread.
    @Volatile private var isConsentReady = false
    @Volatile private var isSdkReadyToStart = false

    fun onConsentReady() {
        isConsentReady = true
        startAfSdkIfAllConditionsAreMet()
    }

    fun onAfSdkReadyToStart() {
        isSdkReadyToStart = true
        startAfSdkIfAllConditionsAreMet()
    }

    private fun startAfSdkIfAllConditionsAreMet() {
        if (isConsentReady && isSdkReadyToStart) {
            AppsFlyerLib.getInstance().start(object : AppsFlyerRequestListener {
                override fun onSuccess() {
                    Log.d("AppsFlyer", "Session started successfully")
                }

                override fun onError(code: Int, error: String) {
                    Log.d("AppsFlyer", "Session start error. Code: $code, error: $error")
                }
            })
            isSdkReadyToStart = false
        }
    }

    fun reset() {
        isConsentReady = false
        isSdkReadyToStart = false
    }
}
```

Wire the coordinator from your `Application` class:

Java

```java
AfSdkStartupManager startupManager = new AfSdkStartupManager();

AppsFlyerLib.getInstance().init(devKey, conversionListener, this);
AppsFlyerLib.getInstance().registerSessionReadyListener(() -> {
    startupManager.onAfSdkReadyToStart();
});

// Call this when your consent flow completes:
// startupManager.onConsentReady();
```

Kotlin

```kotlin
val startupManager = AfSdkStartupManager()

AppsFlyerLib.getInstance().init(devKey, conversionListener, this)
AppsFlyerLib.getInstance().registerSessionReadyListener {
    startupManager.onAfSdkReadyToStart()
}

// Call this when your consent flow completes:
// startupManager.onConsentReady()
```

> 🚧 **Important**
>
> The `SessionReadyListener` callback fires on a background thread. If your consent flow also runs on a background thread, make sure the flags in your coordinator class are thread-safe. At minimum, mark them `volatile` in Java or `@Volatile` in Kotlin, as shown in the sample above.

---

### 4. Update `registerConversionListener`

Two changes apply to `registerConversionListener` in SDK 7:

- The `Context` parameter has been removed.
- The `onAppOpenAttribution` and `onAttributionFailure` callbacks have been removed. These were deprecated in SDK 6 and are no longer needed because Unified Deep Linking (UDL) is now the required path for handling deep links after app open.

Java

```java
// SDK 6
import com.appsflyer.AppsFlyerConversionListener;

AppsFlyerLib.getInstance().registerConversionListener(context, new AppsFlyerConversionListener() {
    @Override
    public void onConversionDataSuccess(Map<String, Object> conversionData) { }

    @Override
    public void onConversionDataFail(String errorMessage) { }

    @Override
    public void onAppOpenAttribution(Map<String, String> attributionData) { }

    @Override
    public void onAttributionFailure(String errorMessage) { }
});

// SDK 7
import com.appsflyer.share.AppsFlyerConversionListener;

AppsFlyerLib.getInstance().registerConversionListener(new AppsFlyerConversionListener() {
    @Override
    public void onConversionDataSuccess(Map<String, Object> conversionData) { }

    @Override
    public void onConversionDataFail(String errorMessage) { }
});
```

Kotlin

```kotlin
// SDK 6
import com.appsflyer.AppsFlyerConversionListener

AppsFlyerLib.getInstance().registerConversionListener(
    context,
    object : AppsFlyerConversionListener {
        override fun onConversionDataSuccess(conversionData: MutableMap<String, Any>?) { }
        override fun onConversionDataFail(errorMessage: String?) { }
        override fun onAppOpenAttribution(attributionData: MutableMap<String, String>?) { }
        override fun onAttributionFailure(errorMessage: String?) { }
    }
)

// SDK 7
import com.appsflyer.share.AppsFlyerConversionListener

AppsFlyerLib.getInstance().registerConversionListener(
    object : AppsFlyerConversionListener {
        override fun onConversionDataSuccess(conversionData: MutableMap<String, Any>?) { }
        override fun onConversionDataFail(errorMessage: String?) { }
    }
)
```

> 📘 **Note**
>
> If your app uses Self-Reporting Networks (SRNs), you still need `onConversionDataSuccess` for the Extended Deferred Deep Linking (EDDL) flow. Only `onAppOpenAttribution` and `onAttributionFailure` are removed. Move any logic from those two callbacks to your UDL implementation using `subscribeForDeepLink`.

---

### 5. Update deep linking

#### 5a. `performDeepLinking` replaces two removed methods

The following methods have been removed:

- `performOnDeepLinking(Intent, Context)`
- `performOnAppAttribution(Context, URI)`

Replace both with `performDeepLinking(String url, boolean shouldTriggerSession)`. This method is more generic than its predecessors: it accepts the deep link as a plain string, works for both intent-based and non-intent-based sources (for example Firebase Messaging Service), and gives you explicit control over whether a Launch event is sent to AppsFlyer.

| Parameter | Description |
|---|---|
| `url` | The deep link string to resolve: a full URL, a OneLink URL, or the string extracted from an `Intent` using `intent.getDataString()`. |
| `shouldTriggerSession` | Set to `true` to send a Launch event to AppsFlyer after resolving the deep link. This is required to close the re-engagement attribution cycle. Set to `false` to resolve the deep link and deliver it to your `DeepLinkListener` without sending a session, for example when the user hasn't yet provided consent. |

Java

```java
// SDK 6
AppsFlyerLib.getInstance().performOnDeepLinking(intent, context);

// SDK 7 — from an Intent
AppsFlyerLib.getInstance().performDeepLinking(intent.getDataString(), true);

// SDK 7 — from a URI (replaces performOnAppAttribution)
AppsFlyerLib.getInstance().performDeepLinking(uri.toString(), true);
```

Kotlin

```kotlin
// SDK 6
AppsFlyerLib.getInstance().performOnDeepLinking(intent, context)

// SDK 7 — from an Intent
AppsFlyerLib.getInstance().performDeepLinking(intent.dataString, true)

// SDK 7 — from a URI (replaces performOnAppAttribution)
AppsFlyerLib.getInstance().performDeepLinking(uri.toString(), true)
```

#### 5b. Set deep link timeout separately

The `subscribeForDeepLink(DeepLinkListener, long)` overload that accepted a timeout has been removed. Set the timeout separately using `setDeepLinkTimeout`, then call `subscribeForDeepLink` without the timeout parameter. This aligns the Android API with iOS.

Java

```java
// SDK 6
AppsFlyerLib.getInstance().subscribeForDeepLink(listener, 3000L);

// SDK 7
AppsFlyerLib.getInstance().setDeepLinkTimeout(3000L);
AppsFlyerLib.getInstance().subscribeForDeepLink(listener);
```

Kotlin

```kotlin
// SDK 6
AppsFlyerLib.getInstance().subscribeForDeepLink(listener, 3000L)

// SDK 7
AppsFlyerLib.getInstance().setDeepLinkTimeout(3000L)
AppsFlyerLib.getInstance().subscribeForDeepLink(listener)
```

#### 5c. UDL no longer requires `start()` first

In SDK 6, UDL required `start()` to have been called before a deep link could be resolved. In SDK 7, the SDK subscribes to the Android lifecycle from `init()` and can catch the very first activity creation or resume. This means you can register for deep links before calling `start()`, and your `DeepLinkListener` will fire even if `start()` hasn't been called yet.

The recommended initialization sequence in your `Application` class is:

1. Call `init()`.
2. Call `subscribeForDeepLink()`.
3. Call `registerSessionReadyListener()`.
4. Call `start()` inside the listener callback, or later when your app's conditions are met.

Deep-linking a user without starting a session is now also a first-class supported flow. If a user hasn't yet provided consent to send data to AppsFlyer but you still want to route them within the app, call `performDeepLinking` with `shouldTriggerSession` set to `false`. The deep link resolves and reaches your listener with no Launch event sent.

---

### 6. Update push notification handling

The existing `sendPushNotificationData(Activity)` API is unchanged. SDK 7 adds a new overload that lets you provide push data manually using the `AFPushData` object. This is useful when your push payload is resolved outside of an `Intent`, for example directly from Firebase Messaging Service.

Java

```java
import com.appsflyer.share.AFPushData;

// Unchanged from SDK 6
AppsFlyerLib.getInstance().sendPushNotificationData(activity);

// New in SDK 7: provide push data manually
Map<String, Object> extras = new HashMap<>();
extras.put("key1", "value1");

AFPushData pushData = new AFPushData(
    "Campaign1",
    "Firebase",
    true,
    extras
);
AppsFlyerLib.getInstance().sendPushNotificationData(pushData);
```

Kotlin

```kotlin
import com.appsflyer.share.AFPushData

// Unchanged from SDK 6
AppsFlyerLib.getInstance().sendPushNotificationData(activity)

// New in SDK 7: provide push data manually
AppsFlyerLib.getInstance().sendPushNotificationData(
    AFPushData(
        campaign = "Campaign1",
        pid = "Firebase",
        isRetargeting = true,
        additionalParameters = mapOf("key1" to "value1")
    )
)
```

---

### 7. Update `setUserEmails`

Two changes apply to `setUserEmails`:

- SHA1 and MD5 encryption types have been removed. Only `NONE` and `SHA256` are supported in SDK 7. If you used MD5 or SHA1, update your code to use `SHA256` or `NONE`.
- `EmailsCryptType` has moved from `AppsFlyerProperties` to `com.appsflyer.share`. Update the import.

Java

```java
// SDK 6
AppsFlyerLib.getInstance().setUserEmails(
    AppsFlyerProperties.EmailsCryptType.SHA256,
    "user@example.com"
);

// SDK 7
import com.appsflyer.share.EmailsCryptType;

AppsFlyerLib.getInstance().setUserEmails(
    EmailsCryptType.SHA256,
    "user@example.com"
);
```

Kotlin

```kotlin
// SDK 6
AppsFlyerLib.getInstance().setUserEmails(
    AppsFlyerProperties.EmailsCryptType.SHA256,
    "user@example.com"
)

// SDK 7
import com.appsflyer.share.EmailsCryptType

AppsFlyerLib.getInstance().setUserEmails(
    EmailsCryptType.SHA256,
    "user@example.com"
)
```

---

### 8. Remove legacy broadcast receivers

`SingleInstallBroadcastReceiver` and `MultipleInstallBroadcastReceiver` have been removed. These were the legacy path for tracking the `com.android.vending.INSTALL_REFERRER` broadcast, which was replaced by the Google Play Install Referrer API years ago.

To migrate:

1. Remove any `<receiver>` entries in your `AndroidManifest.xml` whose `android:name` is `com.appsflyer.SingleInstallBroadcastReceiver` or `com.appsflyer.MultipleInstallBroadcastReceiver`, together with their `INSTALL_REFERRER` intent filter blocks.
2. Add the Google Play Install Referrer library as an `implementation` dependency in your app module's `build.gradle`. The AppsFlyer SDK declares this as `compileOnly` internally, so you must add it explicitly to your app.

```
implementation 'com.android.installreferrer:installreferrer:2.2'
```

> 🚧 **Important**
>
> Leaving old `<receiver>` entries in your manifest causes a build-time error, not a runtime error. Your app won't build until you remove them.

---

### 9. Remove or replace other removed APIs

The following APIs have been removed in SDK 7.

| Removed API | Replacement |
|---|---|
| `waitForCustomerUserId(boolean)` / `setCustomerIdAndLogSession()` | No replacement needed. See the note below. |
| `setCollectIMEI(boolean)` | Use `setImeiData(String)` to provide IMEI manually when needed. |
| `setCollectOaid(boolean)` | Use `setDisableAdvertisingIdentifiers` and other supported identifier APIs. |
| `setExtension(String)` | Use `PluginInfo` (from `com.appsflyer.share.platform_extension`). |
| `registerValidatorListener` / `validateAndLogInAppPurchase` V1 | Use `validateAndLogInAppPurchase(AFPurchaseDetails, Map, AppsFlyerInAppPurchaseValidationCallback)` with `com.appsflyer.share.AFPurchaseDetails`. The V2 API has the listener built in. |
| `setSharingFilter(String...)` / `setSharingFilterForAllPartners()` | Use `setSharingFilterForPartners(String...)`. Pass `"all"` to block all partners. |

> 📘 **Customer user ID flow in SDK 7**
>
> In SDK 6, because the session started automatically, you had to call `waitForCustomerUserId(true)` to tell the SDK to hold off until the CUID was available, then call `setCustomerIdAndLogSession()` to release it. Forgetting the second call caused the SDK to wait indefinitely, which was a common source of integration issues.
>
> In SDK 7, you control when `start()` is called. If you need to include a CUID in the first session, call `setCustomerUserId()` before calling `start()`. No waiting mechanism is needed.

> 📘 **`AppsFlyerProperties` removed**
>
> `AppsFlyerProperties` is no longer available in SDK 7. If your app used `AppsFlyerProperties.getInstance().set()` to configure SDK behavior, contact AppsFlyer Support for guidance on your specific configuration.

> 🚧 **Important**
>
> `setSharingFilter`, `setSharingFilterForAllPartners`, and `validateAndLogInAppPurchase` V1 were deprecated in SDK 6 but some apps continued to use them. Upgrading to SDK 7 causes compile errors on any of these APIs, giving you a clear signal of what to update.

---

## Part 2: Behavioral and additive changes

The following changes don't cause compile errors, but they affect runtime behavior. Review each one carefully, as some may cause silent data loss if not addressed.

### 1. Setter values are no longer persisted between sessions

In SDK 6, many `AppsFlyerLib` setter values were written to disk on Android and survived process restarts. In SDK 7, all setter values are runtime-only. After a cold start, any value you set via an `AppsFlyerLib` setter is gone.

Re-apply any setter values you rely on after every cold start, typically right after `init()`. If you relied on persistence without realizing it, your integration may silently send incomplete data after upgrading.

| API method | Setting | Status in SDK 7 |
|---|---|---|
| `anonymizeUser(boolean)` | User anonymization | Runtime only, re-apply each cold start |
| `enableTCFDataCollection(boolean)` | TCF data collection flag | Runtime only, re-apply each cold start |
| `setDisableNetworkData(boolean)` | Disable outbound network payloads | Runtime only, re-apply each cold start |
| `setCustomerUserId(String)` | Customer user ID | Runtime only, re-apply each cold start |
| `setOutOfStore(String)` | Out-of-store / store override | Runtime only, re-apply each cold start |
| `setAppInviteOneLink(String)` | User-invite OneLink ID | Runtime only, re-apply each cold start |
| `setAdditionalData(Map)` | Custom event and launch map | Runtime only, re-apply each cold start |
| `setUserEmails(...)` | Masked emails | Runtime only, re-apply each cold start |
| `setCollectAndroidID(boolean)` | Collect Android ID | Runtime only, re-apply each cold start |
| `setImeiData(String)` | Manual IMEI | Runtime only, re-apply each cold start |
| `setOaidData(String)` | Manual OAID | Runtime only, re-apply each cold start |
| `setAppId(String)` | App ID override | Runtime only, re-apply each cold start |
| `setIsUpdate(boolean)` | Fresh install vs. update flag | Runtime only, re-apply each cold start |
| `setCurrencyCode(String)` | In-app currency | Runtime only. Alternatively, set `currency_code` in `af_init_config.json` (see below). |
| `setPreinstallAttribution(String...)` | OEM / preinstall override | Runtime only, re-apply each cold start |
| `setLogLevel(AFLogger.LogLevel)` | Log level | Runtime only, re-apply each cold start |
| `setDebugLog(boolean)` | Debug logging shortcut | Runtime only. Alternatively, set `debug_mode` in `af_init_config.json` (see below). |
| `waitForCustomerUserId` / `setCustomerIdAndLogSession` | CUID wait flow | Removed. See Part 1, step 9. |
| `setCollectIMEI` / `setCollectOaid` / `setExtension` | Various | Removed. See Part 1, step 9. |

---

### 2. Use `af_init_config.json` for constant configuration values

SDK 7 introduces a JSON-based initialization helper. If you place a file named `af_init_config.json` in your `src/main/assets/` folder, the SDK reads it during `init()` and applies the supported keys as if the corresponding setters had been called.

This is the recommended approach for any configuration value that is constant and known at build time. Instead of calling the setter on every cold start, put the value in the file once.

| JSON key | Type | Equivalent setter | Example value |
|---|---|---|---|
| `debug_mode` | boolean | Debug logging | `true` |
| `disable_advertising_identifiers` | boolean | `setDisableAdvertisingIdentifiers` | `true` |
| `currency_code` | string | `setCurrencyCode` | `"USD"` |
| `host` | object `{ "prefix": string, "host": string }` | `setHost` | `{ "prefix": "", "host": "af-sdk.net" }` |
| `min_time_between_sessions` | number (int) | `setMinTimeBetweenSessions` | `1` |
| `ddlTimeout` | number (int, ms) | `setDeepLinkTimeout` | `3000` |

Example file at `src/main/assets/af_init_config.json`:

```json
{
  "debug_mode": true,
  "disable_advertising_identifiers": false,
  "currency_code": "USD",
  "host": {
    "prefix": "",
    "host": "af-sdk.net"
  },
  "min_time_between_sessions": 1,
  "ddlTimeout": 3000
}
```

If the file is missing, initialization continues normally. Unknown keys are ignored with a log line. Type mismatches are caught and logged.

---

### 3. Opt in to app-open referrer and web referrer collection

In SDK 6, the SDK automatically collected app-open referrer and web referrer data for all apps as part of the startup flow. In SDK 7, this collection is opt-in.

If you want the SDK to collect this data, call `collectDataFromLauncherActivity(Activity)` from your launcher activity's `onCreate` method, before `start()` runs for that cold start.

Java

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    AppsFlyerLib.getInstance().collectDataFromLauncherActivity(this);
}
```

Kotlin

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    AppsFlyerLib.getInstance().collectDataFromLauncherActivity(this)
}
```

> 🚧 **Important**
>
> Referrer data is only available on the activity that received the original launch intent, which is typically your main or splash activity. If you call this method on a secondary activity, or if your app uses a trampoline or navigation activity that creates other activities, the referrer data will already be gone and nothing will be collected. Call this method once, from your launcher activity, before any other activity starts.

---

## Part 3: Integration checklist

Use this checklist to verify you've addressed all migration steps.

1. Update to Kotlin 2.0 or higher if you use Kotlin 1.9.
2. Set `minSdkVersion` to at least 21 in your `build.gradle`.
3. Update all imports from their previous packages to `com.appsflyer.share.*`. Use Android Studio's auto-import to resolve them.
4. Remove all `start(Context, ...)` overloads. Use `start()` or `start(AppsFlyerRequestListener)`.
5. Add `registerSessionReadyListener` after `init()`. Call `start()` inside the callback (simplified approach) or use the coordinator pattern if your app has pre-start conditions.
6. Update `registerConversionListener`: remove the `Context` parameter, remove `onAppOpenAttribution` and `onAttributionFailure`, and update the import to `com.appsflyer.share.AppsFlyerConversionListener`.
7. Replace `performOnDeepLinking` and `performOnAppAttribution` with `performDeepLinking(String, boolean)`. Replace `subscribeForDeepLink(listener, timeout)` with `setDeepLinkTimeout(long)` followed by `subscribeForDeepLink(listener)`.
8. Remove old `<receiver>` entries for `SingleInstallBroadcastReceiver` and `MultipleInstallBroadcastReceiver` from your manifest. Add `implementation 'com.android.installreferrer:installreferrer:2.2'` to your app's `build.gradle`.
9. Update `setUserEmails`: replace MD5 or SHA1 with `SHA256` or `NONE`, and update the import to `com.appsflyer.share.EmailsCryptType`.
10. Remove or replace other removed APIs: `waitForCustomerUserId`, `setCustomerIdAndLogSession`, `setCollectIMEI`, `setCollectOaid`, `setExtension`, `registerValidatorListener`, `validateAndLogInAppPurchase` V1, `setSharingFilter`, and `setSharingFilterForAllPartners`.
11. Re-apply all `AppsFlyerLib` setter values after every cold start, or move constant values to `af_init_config.json`.
12. Optionally, add `collectDataFromLauncherActivity(this)` to your launcher activity's `onCreate` if you need app-open referrer and web referrer data.

---

## Troubleshooting

The following log messages indicate common integration issues. Search your logcat output for these substrings.

| Symptom | Log message to look for |
|---|---|
| `start()` called without `registerSessionReadyListener` | `SessionReadyListener is not registered! — You must call registerSessionReadyListener(SessionReadyListener) before start().` |
| An API was called before `init()` | `AppsFlyer SDK is not initialized! The API call '...' must be called after the 'init(String, AppsFlyerConversionListener)'` |
| Dev key missing from `init()` | `You must provide AppsFlyer Dev-Key in the 'init' API method` |
| `init()` called with a null context | `AppsFlyer SDK requires a valid Context!` |
| `start()` called twice in the same session | `AppsFlyer SDK session already started. Skipping duplicate start call.` |
| Session not started when finishing | `AppsFlyer SDK session not started. Skipping session finish.` |