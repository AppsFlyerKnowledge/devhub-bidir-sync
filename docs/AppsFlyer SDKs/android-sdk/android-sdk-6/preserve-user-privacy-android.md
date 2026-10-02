---
title: Preserve user privacy
excerpt: Learn how to preserve user privacy in the Android SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---

# Preserve user privacy

## Privacy-preserving methods: When, Who, and What to send

The SDK lets you control three things about the data you send to AppsFlyer:

- **When** you send user-level data
- **Who** receives user-level data
- **What** user-level data you send

### When to send user-level data

The SDK sends information about app installs and in-app events to AppsFlyer as soon as `start` is called. Privacy-preserving methods affect the availability of certain user-level event data depending on whether they're invoked before or after the `start` call.

When `start` is called for the first time, the SDK sends the install event to AppsFlyer, along with the parameters required to attribute the installation to the correct media source. Invoking privacy-preserving methods before calling `start` can prevent AppsFlyer from attributing the install event properly.

So, if you want attribution to occur, avoid calling privacy-preserving methods before `start`. Instead, call them only after the install event has been sent to AppsFlyer. This approach ensures that the user's in-app events remain private while install attribution can still take place.

### Who receives user-level data

Your ad network and Self-Reporting Network (SRN) partners receive user-level data for attribution and optimization. Use a partner-sharing filter method, below, to limit which partners receive this data based on end-user preference.

### What data to send

Some methods below anonymize data by deleting or hashing all user-level identifiers. Others remove only specific identifiers.

## Use start to share only the install event

If you prefer to send only the install event and no additional information, you can invoke `start` with a request callback. Upon receiving a success message confirming that the install event has been logged, you should then call `stop` or `anonymizeUser` from within the callback function.

- [`anonymizeUser`](https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerlib#anonymizeuser) sends data to AppsFlyer; however, upon its arrival to AppsFlyer servers, all identifiers (including IP address) are either deleted or hashed.
- The `stop` method reverts the `start` call, which means that the SDK stops sending any data to AppsFlyer.

```java
appsflyer.start(getApplicationContext(), null, new AppsFlyerRequestListener() {
    @Override
    public void onSuccess() {
        Log.d(LOG_TAG, "Launch sent successfully, got 200 response code from server");
        appsflyer.stop(true, getApplicationContext());
    }

    @Override
    public void onError(int i, @NonNull String s) {
        Log.d(LOG_TAG, "Launch failed to be sent:\n" +
                "Error code: " + i + "\n"
                + "Error description: " + s);
    }
});
```

## Prevent sharing data with third parties

If you want to prevent sharing install and in-app event information with third parties such as SRNs and ad networks, use the `setSharingFilterForPartners` method before calling `start`. Partners that are excluded with this method will not receive data through postbacks, APIs, raw data reports, or any other means.  

**Note:** You can call [`setSharingFilterForPartners`](https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerlib#setsharingfilterforpartners) again if the user changes the app sharing settings (adding or removing partners) later in the session.  
For a code example please refer to [`setSharingFilterForPartners`](https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerlib#setsharingfilterforpartners).

## Anonymize user information

You can configure the SDK to instruct AppsFlyer to remove all user-identifying information by using the [`anonymizeUser`](https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerlib#anonymizeuser) method. In this case, the SDK sends install and in-app events to AppsFlyer, where all identifying information is then deleted or hashed:

- **Deleted:** personal identifiers (GAID, IDFA, IDFV, and CUID)
- **Hashed:** AppsFlyer ID and IP address.

To learn how to implement the method without anonymizing install events, see: [Share only the install event](#use-start-to-share-only-the-install-event).

## Disable IDs

The SDK is capable of sending several specific identifiers to AppsFlyer. You can choose to exclude them in accordance to your needs. 

**Note:** Disabling the advertiser ID before calling start will prevent SRN attribution.

| Disable identifier                                                                                                                                | Description                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| [`setDisableAdvertisingIdentifiers`](https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerlib#setdisableadvertisingidentifiers)(true) | Disables collection of various Advertising IDs by the SDK. This includes Google Advertising ID (GAID), OAID, and Amazon Advertising ID (AAID). |
| [`setCollectOaid`](https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerlib#setcollectoaid)(false)                                    | Disables the collection of OAID by the SDK.                                                                                                    |

## Send data only after the user opts-in

In cases where you would like to not send any data to AppsFlyer until the user gives consent, defer calling [`start`](https://dev.appsflyer.com/hc/docs/android-sdk-reference-appsflyerlib#start) until after the consent is given.
