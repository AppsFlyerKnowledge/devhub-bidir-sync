---
title: Android SDK
excerpt: AppsFlyer Android SDK guides for developers.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
[block:embed]
{
  "html": "<iframe class=\"embedly-embed\" src=\"//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Ffast.wistia.net%2Fembed%2Fiframe%2Ff2xwfh47j5&display_name=Wistia%2C+Inc.&url=https%3A%2F%2Fappsflyer.wistia.com%2Fmedias%2Ff2xwfh47j5&image=https%3A%2F%2Fembed-ssl.wistia.com%2Fdeliveries%2F7566f6c2d82b5da634c1411a8ea231c1.jpg%3Fimage_crop_resized%3D960x540&key=f2aa6fc3595946d0afc3d76cbbd25dc3&type=text%2Fhtml&schema=wistia\" width=\"960\" height=\"540\" scrolling=\"no\" title=\"Wistia, Inc. embed\" frameborder=\"0\" allow=\"autoplay; fullscreen\" allowfullscreen=\"true\"></iframe>",
  "url": "https://appsflyer.wistia.com/medias/f2xwfh47j5",
  "title": "Android SDK install and integrate",
  "favicon": "https://appsflyer.wistia.com/favicon.ico",
  "image": "https://embed-ssl.wistia.com/deliveries/7566f6c2d82b5da634c1411a8ea231c1.jpg?image_crop_resized=960x540"
}
[/block]

Which SDK version should I use?
--------------------------------

> 🚧 Important
>
> Android SDK v7 is current and recommended for all new integrations. It changes how sessions start and includes other breaking changes. SDK 6 continues to receive critical fixes only, all new features ship on v7 only.

## Android SDK 7 (current)

Already integrated on v6? See [Migrate to Android SDK v7](doc:migrate-android-sdk-to-v7).

- [Integrate SDK](doc:integrate-sdk-android-7)
  - [Install SDK](doc:install-android-sdk-7)
  - [Integrate SDK](doc:integrate-android-sdk-7)
  - [Getting the conversion data](doc:conversion-data-android-7)
  - [Setting the Customer User ID](doc:customer-user-id-android-7)
  - [Troubleshooting](doc:troubleshooting-android-7)
- [In-app events](doc:in-app-events-android-7)
  - [Sending events](doc:sending-events-android-7)
  - [Purchase and subscription validation](doc:purchase-validation-android-7)
  - [Ad revenue](doc:ad-revenue-android-7)
- [Preserve user privacy](doc:preserve-user-privacy-android-7)
- [Features](doc:features-android-7)
  - [Push notifications](doc:push-notifications-android-7)
  - [Uninstall measurement](doc:uninstall-measurement-android-7)
  - [Send consent for DMA compliance](doc:android-send-consent-for-dma-compliance-7)
  - [OAID](doc:oaid-android-7)

## Android SDK 6 (previous version)

If you're starting a new integration, use Android SDK 7 instead.

[block:html]
{
  "html": "<details><summary>Android SDK 6</summary>\n<div class=\"af__accordion\">\n  <ul>\n    <li><a href=\"https://dev.appsflyer.com/hc/docs/android-sdk-6\">Android SDK 6</a></li>\n  </ul>\n</div>\n</details>"
}
[/block]

Reference and release notes
----------------------------

[block:html]
{
  "html": "<details><summary>Reference and release notes</summary>\n<div class=\"af__accordion\">\n  <ul>\n    <li><a href=\"https://dev.appsflyer.com/hc/docs/android-sdk-reference\">Android SDK reference</a></li>\n    <li><a href=\"https://support.appsflyer.com/hc/en-us/articles/115001256006\">Android SDK release notes</a></li>\n  </ul>\n</div>\n</details>"
}
[/block]

SDK compatibility
-----------------

- Starting Android V4.4 (v6). Android SDK v7 raises the minimum to API 21, see the [migration guide](https://dev.appsflyer.com/hc/docs/migrate-android-sdk-to-v7) for details.
- Non-mobile Android-based platforms, such as Smart TVs (including Amazon Fire TV). [See CTV overview](https://support.appsflyer.com/hc/en-us/articles/4404083608849)
- [Out-of-store-markets](https://support.appsflyer.com/hc/en-us/articles/207447023) for Android apps, such as Amazon and Baidu.
