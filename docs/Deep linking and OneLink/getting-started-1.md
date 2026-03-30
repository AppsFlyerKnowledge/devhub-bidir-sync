---
title: Getting started
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
[block:callout]
{
  "type": "danger",
  "title": "This page has a newer [version](https://dev.appsflyer.com/hc/docs/dl_getting_started)",
  "body": ""
}
[/block]
![](https://files.readme.io/ab7e812-GS_OneLink_.gif "GS_OneLink .gif")

**At a glance:** OneLink is a single link that apps use to provide the optimal user experience to their users regardless of device, operating system, channel, or platform. This guide provides an overview of OneLink benefits, and the scope of work for basic OneLink setup within the app, deep linking, and deferred deep linking.

About Onelink
-------------

OneLink is a single link that marketers use to redirect their users regardless of device, operating system, channel, platform, and whether or not they have the app installed.

#### Deep linking

If users have the app installed already, OneLink: 

- Opens the app.
- Can retrieve data from the click, and use the OneLink data to redirect the user to a specific in-app activity. 

#### Deferred deep linking

If users do not have the app installed, OneLink:

- Redirects users to the correct app store (for example, App Store or Google Play), or landing page.
- Can be used to match the data from the click to this installation, and use the OneLink data to automatically redirect the user to a specific in-app activity when they open the app. 

Setup
-----

Setting up a OneLink requires two different personas within an organization to work together, using their own resources: Marketers and developers.

Marketers plan the marketing campaigns and set up the OneLink URLs. The OneLink URLs are set up to carry parameters and data that are used to give users a personalized experience when deep linking and deferred deep linking. 

Developers perform the OneLink setup in the app:

1. Initial app setup for [Android](https://dev.appsflyer.com/hc/docs/initial-setup-for-deep-linking-and-deferred-deep-linking) and [iOS](https://dev.appsflyer.com/hc/docs/initial-setup-2): Opens the app (using Android App Links, Universal Links, or URI schemes)
2. Unified deep linking (UDL) setup for [Android](https://dev.appsflyer.com/hc/docs/unified-deep-linking-udl) and [iOS](https://dev.appsflyer.com/hc/docs/unified-deep-linking-udl-1): Method to retrieve data from the click and use that data to redirect users for a personalized experience to a specific in-app activity (deep linking or deferred deep linking).  
   **Note:** Customers already using OneLink may be using the legacy methods for [Android](https://dev.appsflyer.com/hc/docs/android-legacy-apis) and [iOS](https://dev.appsflyer.com/hc/docs/ios-legacy-apis) deep linking and deferred deep linking, instead of UDL.