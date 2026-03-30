---
title: Organic search attribution
excerpt: >-
  **At a glance**: Attribute existing users who re-engage with your app after an
  organic web search.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
> ❗️ This page has a newer [version](https://dev.appsflyer.com/hc/docs/dl_android_organic_search_attr)

## Overview

App owners using Android App Links for deep linking (without OneLink), who have a domain associated with their app can attribute sessions initiated via this domain using the `appendParametersToDeepLinkingURL` method.

## Prerequisites

- Android SDK 6.0.1+.
- Call this method before calling [`start`](#start). 

## Usage

### Input parameters

| Type                  | Name         | Description                                                         |
| :-------------------- | :----------- | :------------------------------------------------------------------ |
| `String`              | `contains `  | A domain name to identify URLs                                      |
| `Map<String, String>` | `parameters` | Parameters to append to the deeplink URL after it passed validation |

Provide the following parameters in the `parameters` `Map`:

- `pid`
- `is_retargeting=true`

### Usage example

```java
HashMap<String, String> urlParameters = new HashMap<>();
parameters.put("pid", "exampleDomain"); // Required
parameters.put("is_retargeting", "true"); // Required
AppsFlyerLib.getInstance().appendParametersToDeepLinkingURL("example.com", parameters);
```
```kotlin
AppsFlyerLib.getInstance().appendParametersToDeepLinkingURL("example.com",
mapOf("pid" to "exampleDomain", "is_retargeting" to "true")) // Required
```



In the example above, the attribution URL sent to AppsFlyer servers is:

```
example.com?pid=exampleDomain&is_retargeting=true
```