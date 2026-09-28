---
title: Branded domains [Draft]
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
## Overview
A branded (vanity) domain lets you replace a generic `onelink.me` link with your own domain, for example `promotion.greatapp.com`, while keeping OneLink's attribution and deep linking behavior intact.

Setting this up has two parts:
1. A marketer-owned domain configuration in the AppsFlyer dashboard and DNS.
2. An SDK call that tells the app to recognize and resolve links from that domain.

This article covers the concept and links to the platform-specific SDK setup. For creating the branded domain in the dashboard and adding the CNAME record with your DNS provider, see [Brand OneLink with your domain](https://support.appsflyer.com/hc/en-us/articles/360002329137#h_01HFM51V4PWWX7TBCHFMPR3QVB) in the Help Center.

## Prerequisites
- A branded domain already mapped to your OneLink subdomain via CNAME (dashboard and DNS steps, done in the Help Center article above).
- Invite-referral 5.2.0+, if you use branded domains with User Invite links.

## Implementation guides

[block:html]
{
  "html": "<div class=\"button-container\"><a class=\"button android\" href=\"https://dev.appsflyer.com/hc/docs/dl_android_branded_domains\">Android</a><a class=\"button ios\" href=\"https://dev.appsflyer.com/hc/docs/dl_ios_branded_domains\">iOS</a></div><style>.button-container{display:flex;}.button{display:flex;justify-content:center;align-items:center;width:150px;border-radius:6px;padding:8px;margin-right:4px;}.button:before{margin-right:4px;}.button.android{border:solid 2px #3DDC84;}.ios{border-radius:6px;padding:8px;border:solid 2px #7D7D7D;}.ios:before{content:url(\"https://files.readme.io/19fdc72-apple-icon.svg\");}.android:before{content:url(\"https://files.readme.io/d7dc5a3-android-icon.svg\");}</style>"
}
[/block]

## See also
- [Brand OneLink with your domain](https://support.appsflyer.com/hc/en-us/articles/360002329137#h_01HFM51V4PWWX7TBCHFMPR3QVB) — dashboard and DNS/CNAME setup
- [Deep Linking work flow](https://dev.appsflyer.com/hc/docs/dl_work_flow)
