---
title: 'Android: Set parameters based on the clicked URL domain'
hidden: false
---

## Overview
Organic search attribution can be set from AppsFlyer without updating the SDK. [Learn more](https://support.appsflyer.com/hc/en-us/articles/15123194526353#setup).

Use the `appendParametersToDeepLinkingURL` method to dynamically set the media source and other parameters based on the clicked URL domain name.

## Prerequisites
- Call this method before calling [`start`](#start). 

## Usage

### Input parameters

| Type                  | Name         | Description                                               |
| :-------------------- | :----------- | :-------------------------------------------------------- |
| `String`              | `contains `  | A domain name to identify URLs                  |
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
