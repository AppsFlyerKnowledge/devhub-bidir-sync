---
title: iOS SDK [Draft]
excerpt: AppsFlyer iOS SDK guides for developers.
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
  "html": "<iframe class=\"embedly-embed\" src=\"//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Ffast.wistia.net%2Fembed%2Fiframe%2Fqo9qauberm&display_name=Wistia%2C+Inc.&url=https%3A%2F%2Fappsflyer.wistia.com%2Fmedias%2Fqo9qauberm&image=https%3A%2F%2Fembed-ssl.wistia.com%2Fdeliveries%2F479ad78a31932db5e727279e79bbdfe9.jpg%3Fimage_crop_resized%3D960x540&key=f2aa6fc3595946d0afc3d76cbbd25dc3&type=text%2Fhtml&schema=wistia\" width=\"960\" height=\"540\" scrolling=\"no\" title=\"IOS SDK install and integrate\" frameborder=\"0\" allow=\"autoplay; fullscreen\" allowfullscreen=\"true\"></iframe>",
  "url": "https://appsflyer.wistia.com/medias/qo9qauberm",
  "title": "IOS SDK install and integrate",
  "favicon": "https://appsflyer.wistia.com/favicon.ico",
  "image": "https://embed-ssl.wistia.com/deliveries/479ad78a31932db5e727279e79bbdfe9.jpg?image_crop_resized=960x540"
}
[/block]

Which SDK version should I use?
--------------------------------

> 🚧 Important
>
> iOS SDK v7 is current and recommended for all new integrations. It changes how sessions start and includes other breaking changes. SDK 6 continues to receive critical fixes only, all new features ship on v7 only.

## iOS SDK 7 (current)

Already integrated on v6? See [Migrate to iOS SDK v7](doc:migrate-ios-sdk-to-v7).

- [Integrate SDK](doc:integrate-sdk-ios-7)
  - [Install SDK](doc:install-ios-sdk-7)
  - [Integrate SDK](doc:integrate-ios-sdk-7)
  - [Getting the conversion data](doc:conversion-data-ios-7)
  - [Setting the Customer User ID](doc:customer-user-id-ios-7)
  - [Troubleshooting](doc:troubleshooting-ios-7)
- [In-app events](doc:in-app-events-ios-7)
  - [Sending events](doc:sending-events-ios-7)
  - [Purchase and subscription validation](doc:purchase-validation-ios-7)
  - [Ad revenue](doc:ad-revenue-ios-7)
- [Preserve user privacy](doc:preserve-user-privacy-ios-7)
- [Features](doc:features-ios-7)
  - [Push notifications](doc:push-notifications-ios-7)
  - [Uninstall measurement](doc:uninstall-measurement-ios-7)
  - [Send consent for DMA compliance](doc:ios-send-consent-for-dma-compliance-7)

## iOS SDK 6 (previous version)

If you're starting a new integration, use iOS SDK 7 instead.

[block:html]
{
  "html": "<details><summary>iOS SDK 6</summary>\n<div class=\"af__accordion\">\n  <ul>\n    <li><a href=\"https://dev.appsflyer.com/hc/docs/ios-sdk-6\">iOS SDK 6</a></li>\n  </ul>\n</div>\n</details>"
}
[/block]

Reference documents
-------------------

[block:html]
{
  "html": "<details><summary>Reference documents</summary>\n<div class=\"af__accordion\">\n  <ul>\n    <li><a href=\"https://dev.appsflyer.com/hc/docs/ios-sdk-reference\">iOS SDK reference</a></li>\n    <li><a href=\"https://support.appsflyer.com/hc/en-us/articles/115001224823\">iOS SDK release notes</a></li>\n  </ul>\n</div>\n</details>"
}
[/block]

SDK compatibility
-----------------

- iOS 9+ (iPhone, iPod, iPad)
- tvOS 9+ (Apple TV)
- MacOS 10.13 (High Sierra)
- Complies with Apple [IPv6 DNS64/NAT64](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/UnderstandingandPreparingfortheIPv6Transition/UnderstandingandPreparingfortheIPv6Transition.html#//apple_ref/doc/uid/TP40010220-CH213-SW1) networks.

For more information, see [SDK compatibility](https://support.appsflyer.com/hc/en-us/articles/207032126-SDK-integration-overview#sdk-compatibility).
