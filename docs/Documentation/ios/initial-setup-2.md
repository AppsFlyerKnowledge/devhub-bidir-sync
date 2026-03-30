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
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/18804c2-direct_and_deferred_deep_linking.png",
        "direct and deferred deep linking.png",
        1494,
        663,
        "#d1d2cc"
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Deep linking methods"
}
[/block]
There are two deep linking methods that need to be implemented to cover your entire user base. The method used depends on the mobile platform version and platform.

The two methods and instructions for implementation are described in detail in the following sections.
[block:parameters]
{
  "data": {
    "h-0": "Method",
    "h-1": "Description",
    "h-2": "iOS Versions",
    "0-0": "**Universal Links**",
    "0-1": "Directly opens the mobile app at the default activity.\nUniversal Links take the format of regular web links (e.g. https://yourbrand.onelink.me or https://www.yourbrand.com).",
    "0-2": "iOS 9 and above",
    "1-0": "**URI Scheme**",
    "1-1": "Directly opens the app based on the activity path specified in the URI scheme.",
    "1-2": "iOS all versions",
    "h-3": "Procedures",
    "1-3": "1. [Decide on a URI scheme with the marketer.](https://dev.appsflyer.com/docs/initial-setup-2#deciding-on-a-uri-scheme)\n2. [Add URI scheme.](https://dev.appsflyer.com/docs/initial-setup-2#adding-uri-scheme)\n3. [Testing](https://dev.appsflyer.com/docs/initial-setup-2#testing-uri-schemes)",
    "0-3": "1. [Get the app bundle ID and prefix ID.](https://dev.appsflyer.com/docs/initial-setup-2#getting-the-app-bundle-id-and-prefix-id)\n2. [Enable associated domains.](https://dev.appsflyer.com/docs/initial-setup-2#enabling-associated-domains)"
  },
  "cols": 4,
  "rows": 2
}
[/block]

[block:api-header]
{
  "title": "Procedures for iOS Universal Links"
}
[/block]
### Getting the app bundle ID and prefix ID

1. Log into your Apple Developer Account.
2. On the left-hand menu, select ***Certificates, Identifiers & Profiles***.
3. Under ***Identifiers***, select ***App IDs***.
4. Click the relevant app.
5. Copy the prefix ID and app bundle ID.
6. Give the prefix ID and app bundle ID to your marketer.
The marketer will use it in AppsFlyer's dashboard to register the app.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6b67004-certs_apple_info.png",
        "certs_apple_info.png",
        832,
        689,
        "#f8f7f7"
      ]
    }
  ]
}
[/block]
### Enabling associated domains

**To support associated domains in your app**:

1. Follow [these iOS instructions](https://developer.apple.com/documentation/safariservices/supporting_associated_domains_in_your_app). 

### Configuring mobile apps to register approved domains
Configuring mobile apps to register approved domains takes place inside Xcode. It requires the OneLink subdomain that your marketer generates.

**To configure mobile apps to register approved domains**:

1. Get the OneLink subdomain from your marketer.
2, In Xcode, click on your project.
3. Click on the project target - see screenshot below.
4. Switch to ***Capabilities*** tab.
5. Turn on ***Associated Domain***.
6. Add the subdomain that you got from your marketer.
    The format is **applinks:subdomain.onelink.me**
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ed37397-xcode-associated-domains.png",
        "xcode-associated-domains.png",
        2032,
        1430,
        "#2f3034"
      ]
    }
  ]
}
[/block]
### Universal Link limitations

#### Opening apps from browsers
Universal Links only work when clicked on. For example, when clicking a link in a web page or email. Pasting the link in the browser address bar doesn't deep link into the app.

#### OneLink subdomain
While the OneLink subdomain can be changed at anytime, it causes all existing OneLink URLs using the original subdomain to stop working.

#### OneLink in social network apps
Not all apps, including social networks apps, fully support Universal links. For further details, please see [this guide](https://support.appsflyer.com/hc/en-us/articles/207032246-OneLink-Basic-Setup-Guide#partners-onelink-social-apps).

#### Other limitations and issues
There may also be other limitations with Universal links. Visit the OneLink troubleshooting section for more details.
[block:api-header]
{
  "title": "Procedures for URI scheme"
}
[/block]
A URI scheme is a URL that leads users directly to the mobile app. 

When an app user enters a URI scheme in a browser address bar box, or clicks on a link based on a URI scheme, the app launches and the user is deep-linked.

Whenever an App Link fails to open the app, the URI scheme can be used as a fallback to open the application.

### Deciding on a URI scheme

**To decide on a URI scheme:**
1. Contact the marketer and Android developer. 
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
### Adding URI scheme

**To add the URI scheme:**

1. In Xcode, open app information plist file.
2. Add a **URL types** entry.
3. Expand the **URL type** and **Item 0** rows.
4. Add a unique identifier for the app for URL identifier as a value. 
It is best to select a unique identifier that is unlikely to be used by other apps.
5. Right-click **URL identifier** and select **Add Row** > **URI Schemes**.
6. Set the **Item 0** value to your unique scheme.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/569ae8d-info_list_uri_schemes.png",
        "info_list_uri_schemes.png",
        750,
        431,
        "#2c3035"
      ]
    }
  ]
}
[/block]
### Testing URI schemes

**Prerequisites**:

An iOS device with the app installed. Make sure it is the app source and version where the developer(s) made changes (and implemented Universal Links and URI schemes).

**To test the URI scheme**:

1. Contact the marketer and get the custom link they created.
2. Send the short or long URL the marketer gives you to your phone. You can either:
   * Scan the QR code with your phone camera or QR scanner app.
   * Email or WhatsApp yourself the link, and open it on your phone.
3. Click the link on your mobile device.
The app should open to its home screen.

[1]: https://support.appsflyer.com/hc/en-us/articles/207033836?__hstc=215508872.986091deeadbd815ef04121e1d880589.1586684365062.1591196345127.1591212728952.29&__hssc=215508872.2.1591212728952&__hsfp=3667076369 "Title"

If the link doesn't open the app, add the parameter af_force_deeplink=true to the custom attribution link. For example:
[block:code]
{
  "codes": [
    {
      "code": "https://demo.onelink.me/1aBC/123ab45c?af_force_deeplink=true ",
      "language": "text"
    }
  ]
}
[/block]
### URI scheme limitations
Neither Apple nor Google enforces unique naming for app schemes. **Choose a scheme name unique to your brand to avoid** conflicting schemes across different applications. A good scheme name could be your app bundle ID i.e. **com.company.app**.

To enable OneLink to serve both iOS and Android, it is important that **the same scheme is defined for both platforms**.

When a OneLink that has `af_force_deeplink=true` is opened in iOS 12.3.1, the following logic applies:
* A dialog is shown asking the user if the app installed:
    * If the user chooses OK (app is installed), AppsFlyer attempts to open the app using URI scheme
    * If the user chooses Cancel (app is not installed), AppsFlyer redirects the user to the app store
    * If the user chooses OK but the app is not installed, an error message is shown:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4bdb9ef-885402320830842.XbZXy5YrCSL3FKIBZPjn_height640.png",
        "885402320830842.XbZXy5YrCSL3FKIBZPjn_height640.png",
        353,
        204,
        "#e1e1e1"
      ]
    }
  ]
}
[/block]