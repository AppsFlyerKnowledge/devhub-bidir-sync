---
title: 'Android: Branded domains [Draft]'
excerpt: '**At a glance**: Set a branded domain for the Android SDK'
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

Use [`setOneLinkCustomDomain`](doc:android-sdk-reference-appsflyerlib#setonelinkcustomdomain) to register the branded domain(s) mapped to your OneLink subdomain, so the SDK recognizes clicks from that domain.

## Prerequisites
- Android SDK 4.10.1+.
- Call this method before calling [`init`](doc:android-sdk-reference-appsflyerlib#init).
- The domain must already be mapped to your OneLink subdomain via CNAME. See [Brand OneLink with your domain](https://support.appsflyer.com/hc/en-us/articles/360002329137#h_01HFM51V4PWWX7TBCHFMPR3QVB).

> 🚧 Call this before `init`
> If the SDK initializes first, the domain isn't recognized and deep linking falls back to default resolution.

## Usage

### Usage example

```java
AppsFlyerLib.getInstance().setOneLinkCustomDomain("promotion.greatapp.com", "click.greatapp.com");
AppsFlyerLib.getInstance().init(devKey, null, this);
```

Don't include `https://` as part of the domain.

## See also
- [Branded domains](doc:dl_branded_domains)
- [Android SDK reference: AppsFlyerLib](doc:android-sdk-reference-appsflyerlib)
