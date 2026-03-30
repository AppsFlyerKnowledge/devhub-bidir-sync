---
title: Initial app setup
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
**At a glance**: The initial app setup enables the marketer to create links that will send existing app users directly into the app. The initial setup is also a prerequisite for direct and deferred deep-linking.
[block:api-header]
{
  "title": "Deep linking methods"
}
[/block]
There are two deep-linking methods that need to be implemented to cover your entire user base. The method used depends on the mobile platform version.

The two methods and instructions for implementation are described in detail in the following sections.
[block:parameters]
{
  "data": {
    "h-0": "Method",
    "h-1": "Description",
    "h-2": "Android Versions",
    "0-0": "**Android App Links**",
    "0-1": "Directly opens the mobile app at the default activity.",
    "0-2": "Android 6 and later",
    "1-0": "**URI Scheme**",
    "1-1": "Directly opens the app based on the activity path specified in the URI scheme.",
    "1-2": "Android all versions",
    "h-3": "Procedure",
    "0-3": "1. [Generate SHA256 fingerprint.](https://afonelink.readme.io/docs/initial-setup-for-deep-linking-and-deferred-deep-linking#generating-a-sha256-fingerprint)\n2. [Add intent-filter to main activity.](https://afonelink.readme.io/docs/initial-setup-for-deep-linking-and-deferred-deep-linking#adding-app-link-intent-filter-to-main-activity)",
    "1-3": "1. [Decide on a URI scheme with the marketer.](https://afonelink.readme.io/docs/initial-setup-for-deep-linking-and-deferred-deep-linking#deciding-on-a-uri-scheme)\n2. [Add intent-filter to main activity.](https://afonelink.readme.io/docs/initial-setup-for-deep-linking-and-deferred-deep-linking#adding-app-link-intent-filter-to-main-activity)\n3. [Testing](https://afonelink.readme.io/docs/initial-setup-for-deep-linking-and-deferred-deep-linking#testing-uri-schemes)"
  },
  "cols": 4,
  "rows": 2
}
[/block]

[block:api-header]
{
  "title": "Procedures for Android App Links"
}
[/block]
Android App Links work with Android 6.0 and above. [Learn more](https://support.appsflyer.com/hc/en-us/articles/115005314223).
### Generating a SHA256 fingerprint

**To generate the SHA256 fingerprint:**

1. Locate your [app's keystore][33].
If the app is in still in development, locate the `debug.keystore`
   * For Windows User: `C:\Users\USERNAME\.android\debug.keystore`
   * For Linux or Mac OS User: `~/.android/debug.keystore`
2. Open the command line and navigate to the folder where the keystore file is located.
3. Run the command:
    
[33]: https://developer.android.com/training/articles/keystore "Title"
[block:code]
{
  "codes": [
    {
      "code": "keytool -list -v -keystore [APK-KEY].keystore",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "The password for the debug.keystore is usually \"android\"."
}
[/block]
The output should look like this:
[block:code]
{
  "codes": [
    {
      "code": "Alias name: test\nCreation date: Sep 27, 2017\nEntry type: PrivateKeyEntry\nCertificate chain length: 1\nCertificate[1]:\nOwner: CN=myname\nIssuer: CN=myname\nSerial number: 365ead6d\nValid from: Wed Sep 27 17:53:32 IDT 2017 until: Sun Sep 21 17:53:32 IDT 2042\nCertificate fingerprints:\nMD5: DB:71:C3:FC:1A:42:ED:06:AC:45:2B:6D:23:F9:F1:24\nSHA1: AE:4F:5F:24:AC:F9:49:07:8D:56:54:F0:33:56:48:F7:FE:3C:E1:60\nSHA256: A9:EA:2F:A7:F1:12:AC:02:31:C3:7A:90:7C:CA:4B:CF:C3:21:6E:A7:F0:0D:60:64:4F:4B:5B:2A:D3:E1:86:C9\nSignature algorithm name: SHA256withRSA\nVersion: 3\nExtensions:\n#1: ObjectId: 2.5.29.14 Criticality=false\nSubjectKeyIdentifier [\n  KeyIdentifier [\n   0000: 34 58 91 8C 02 7F 1A 0F  0D 3B 9F 65 66 D8 E8 65 \n   0010: 74 42 2D 44                    \n ]\n]",
      "language": "text"
    }
  ]
}
[/block]
4. Send the SHA256 back to the marketer. 

### Adding App Link intent-filter to main activity
[block:callout]
{
  "type": "info",
  "title": "The marketer will give you an auto-generated intent-filter code.",
  "body": "The intent-filter code is used in the AndroidManifest.XML."
}
[/block]
**To add the intent-filter to the main activity:**

1. Open the app's `AndroidManifest.xml` file.
2. Add the intent-filter to the **main activity**.
If there already is an intent-filter for the Android App Link in the main activity, overwrite it. 
[block:callout]
{
  "type": "info",
  "title": "Example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "<intent-filter android:autoVerify=\"true\">\n    <action android:name=\"android.intent.action.VIEW\" />\n\n    <category android:name=\"android.intent.category.DEFAULT\" />\n    <category android:name=\"android.intent.category.BROWSABLE\" />\n    <data\n        android:host=\"onelink-basic-app.onelink.me\"\n        android:pathPrefix=\"/H5hv\"\n        android:scheme=\"https\" />\n</intent-filter>",
      "language": "xml",
      "gist": "42247018de4a3f87513fcefef00710ef"
    }
  ]
}
[/block]
Github link: [XML][aal_intent-filter]

[aal_intent-filter]: https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/5b202b983b33d62bd5d80102ab27f17e2b1cb25f/java/basic_app/app/src/main/AndroidManifest.xml#L39-L49

3. Tell the marketer that the App Link configuration is completed.
When the marketer tests the link, it should direct the user to the app's main page.
[block:api-header]
{
  "title": "Procedures for URI Scheme"
}
[/block]
A URI scheme is a URL that leads users directly to the mobile app. 

When an app user enters a URI scheme in a browser address bar box, or clicks on a link based on a URI scheme, the app launches and the user is deep-linked.

Whenever an App Link fails to open the app, the URI scheme can be used as a fallback to open the application.

### Deciding on a URI scheme

**To decide on a URI scheme:**
1. Contact the marketer and iOS developer. 
2. Choose a URI scheme. For example: `yourappname://`
[block:callout]
{
  "type": "info",
  "body": "* Use a URI scheme that is as unique as possible to your app and brand to avoid coincidental overlap with other apps in the ecosystem. Overlap with other apps is an inherent issue in the nature of URI scheme protocol.\n* The URI scheme should not start with *http* or *https*.\n* The URI scheme should be similarly defined on Android and iOS."
}
[/block]

[block:callout]
{
  "type": "success",
  "title": "URI scheme should be supplied to the marketer",
  "body": "Send the URI scheme to the marketer, e.g. ``afshopapp://mainactivity``"
}
[/block]
### Adding URI scheme intent-filter to main activity

**To add the intent-filter to the main activity:**

1. Open the app's `AndroidManifest.xml` file.
2. Add the following intent-filter to the **main activity**.
In the `data` section, replace `host` and `scheme` with the URI scheme you chose. In the intent-filter code below, `host="mainactivity"` and `scheme="afshopapp"`, matching the URI scheme `afshopapp://mainactivity`.
If there already is an intent-filter for the URI scheme in the main activity, overwrite it.
[block:code]
{
  "codes": [
    {
      "code": "<intent-filter>\n    <action android:name=\"android.intent.action.VIEW\" />\n    <category android:name=\"android.intent.category.DEFAULT\" />\n    <category android:name=\"android.intent.category.BROWSABLE\" />\n\n    <data\n        android:host=\"mainactivity\"\n        android:scheme=\"afshopapp\" />\n</intent-filter>",
      "language": "xml",
      "gist": "7ce7fdc26fff3e9aecf0ba54d9ec63e6"
    }
  ]
}
[/block]
⇲ Github link: [XML][uri_intent_filter]

3. Give the URI scheme to the marketer.

[uri_intent_filter]: https://github.com/AppsFlyerSDK/appsflyer-onelink-android-sample-apps/blob/5b202b983b33d62bd5d80102ab27f17e2b1cb25f/java/basic_app/app/src/main/AndroidManifest.xml#L29-L38

### Testing URI schemes

**Prerequisites**:

An Android device with the app installed. Make sure it is the app source and version where the developer(s) made changes (and implemented App Links and URI schemes).

**To test the URI scheme**:

1. Contact the marketer and get the custom link they created.
2. Send the short or long URL the marketer gives you to your phone. You can either:
   * Scan the QR code with your phone camera or QR scanner app.
   * Email or WhatsApp yourself the link, and open it on your phone.
3. Click the link on your mobile device.
The app should open to its home screen.

[1]: https://support.appsflyer.com/hc/en-us/articles/207033836?__hstc=215508872.986091deeadbd815ef04121e1d880589.1586684365062.1591196345127.1591212728952.29&__hssc=215508872.2.1591212728952&__hsfp=3667076369 "Title"