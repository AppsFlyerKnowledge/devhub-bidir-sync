---
title: GETTING STARTED WITH ONELINK
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
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b4b6e9b-4906_step-4_1.gif",
        "4906_step-4 (1).gif",
        200,
        165,
        "#000000"
      ]
    }
  ]
}
[/block]
**  **At a glance**: OneLink is a single link that apps use to provide the optimal user experience to their users regardless of device, operating system, channel, or platform. This article provides an overview of OneLink benefits, as well as an explanation of the steps needed for basic OneLink setup within the app.** 
[block:api-header]
{
  "title": "About Onelink"
}
[/block]
OneLink:

* Helps marketers engage their users by providing links that work across all owned media marketing channels, including banners, social media posts, QR codes, SMS campaigns, emails, etc. 
* Redirects users to the correct app store (e.g. App Store or Google Play), landing page, or app (if users already have the app installed).
* Drastically increases user engagements and conversions by **deep linking** existing users, or **deferred deep linking** new users, to specific content or a specific experience within the app.
[block:api-header]
{
  "title": "Deep linking and deferred deep linking"
}
[/block]
Since users may or may not have the mobile app installed, there are two types of deep linking:
[block:parameters]
{
  "data": {
    "0-0": "**Deep linking**",
    "0-1": "Directly serves personalized content to existing users, who already have the app installed.",
    "1-0": "**Deferred deep linking**",
    "1-1": "Serves personalized content to new or former users, directly after the download, installation and first app launch."
  },
  "cols": 2,
  "rows": 2
}
[/block]
**Using OneLink, AppsFlyer supports both deep linking and deferred deep linking methods.**
[block:api-header]
{
  "title": "OneLink setup"
}
[/block]
Setting up a OneLink requires two different personas within an organization to work together, using their own resources:

* Marketers/product managers: Plan the marketing campaigns and set up the OneLink in the AppsFlyer dashboard.
* Developers: Configure the app to receive data from the link click, and then launch the app with the requisite personalized user experience (with deep linking and deferred deep linking).

The developer's tasks for OneLink setup in the app include: 

1. Initial app setup
2. Direct deep linking
3. Deferred deep linking