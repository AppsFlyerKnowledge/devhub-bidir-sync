---
title: 'iOS: Branded domains [Draft]'
excerpt: '**At a glance**: Set a branded domain for the iOS SDK'
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
## Overview
For an introduction, see [Branded domains](doc:dl_branded_domains).

Use [`oneLinkCustomDomains`](doc:ios-sdk-reference-appsflyerlib#onelinkcustomdomains) to register the branded domain(s) mapped to your OneLink subdomain, so the SDK recognizes clicks from that domain.

## Prerequisites
- iOS SDK 4.10.1+.
- Set this property before calling [`start`](doc:ios-sdk-reference-appsflyerlib#start).
- The domain must already be mapped to your OneLink subdomain via CNAME. See [Brand OneLink with your domain](https://support.appsflyer.com/hc/en-us/articles/360002329137#h_01HFM51V4PWWX7TBCHFMPR3QVB).

> 🚧 Set this before `start`
> If the SDK initializes first, the domain isn't recognized and deep linking falls back to default resolution.

## Usage

### Usage example

```swift
AppsFlyerLib.shared().oneLinkCustomDomains = ["promotion.greatapp.com", "click.greatapp.com"]
AppsFlyerLib.shared().start()
```
```Obj-c
[AppsFlyerLib shared].oneLinkCustomDomains = @[@"promotion.greatapp.com", @"click.greatapp.com"];
[[AppsFlyerLib shared] start];
```

Don't include `https://` as part of the domain.

## See also
- [Branded domains](doc:dl_branded_domains)
- [iOS SDK reference: AppsFlyerLib](doc:ios-sdk-reference-appsflyerlib)
