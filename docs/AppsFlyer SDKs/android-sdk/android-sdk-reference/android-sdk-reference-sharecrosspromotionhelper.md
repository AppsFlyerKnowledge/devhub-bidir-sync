---
title: CrossPromotionHelper
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  title: ''
  description: ''
  robots: noindex
---
## Overview

Android SDK cross-promotion helper class.

Go back to the [SDK reference index](doc:android-sdk-reference).

**Class declaration**

```java Java
public class CrossPromotionHelper
```
```kotlin Kotlin
class CrossPromotionHelper
```

**Import the class**

```java Java
import com.appsflyer.share.CrossPromotionHelper;
```
```kotlin Kotlin
import com.appsflyer.share.CrossPromotionHelper
```

## Methods

### logAndOpenStore

**Method signature**

```java Java
public static void logAndOpenStore(@NonNull Context context,
                                   String promoted_app_id,
                                   String campaign,
                                   Map<String, String> userParams)
```
```kotlin Kotlin
fun logAndOpenStore(context: Context,
                    promoted_app_id: String?,
                    campaign: String?,
                    userParams: Map<String, String>?)
```

**Description**
Sends the click to AppsFlyer and opens the promoted app's Play Store listing. Learn more in [Cross-promotion](doc:cross-promotion-android).

**Input arguments**

| Type                  | Name              | Description                       |
| :-------------------- | :---------------- | :-------------------------------- |
| `Context`             | `context`         | Application / Activity Context.   |
| `String`              | `promoted_app_id` | Promoted App ID, the package name of the promoted app as it appears within the AppsFlyer dashboard. |
| `String`              | `campaign`        | Name of cross-promotion campaign. |
| `Map<String, String>` | `userParams`      | Optional. Custom parameters that are added to the link. |

**Returns**
`void`.

### logCrossPromoteImpression

**Method signature**

```java Java
public static void logCrossPromoteImpression(@NonNull Context context,
                                             String appID,
                                             String campaign,
                                             Map<String, String> userParams)
```
```kotlin Kotlin
fun logCrossPromoteImpression(context: Context,
                              appID: String?,
                              campaign: String?,
                              userParams: Map<String, String>?)
```

**Description**
Logs an impression as part of a cross-promotion campaign. Make sure to use the promoted App ID as it appears within the AppsFlyer dashboard. Learn more in [Cross-promotion](doc:cross-promotion-android).

**Input arguments**

| Type                  | Name         | Description                       |
| :-------------------- | :----------- | :-------------------------------- |
| `Context`             | `context`    | Application / Activity Context.   |
| `String`              | `appID`      | Promoted App ID, the package name of the promoted app as it appears within the AppsFlyer dashboard. |
| `String`              | `campaign`   | Name of cross-promotion campaign. |
| `Map<String, String>` | `userParams` | Optional. Custom parameters that are added to the link. |

**Returns**
`void`.

### setUrl

**Method signature**

```java Java
public static void setUrl(Map<String, String> mapOfURLs)
```
```kotlin Kotlin
fun setUrl(mapOfURLs: Map<String, String>?)
```

**Description**
Sets a map of URLs for `CrossPromotionHelper`.

**Input arguments**

| Type                  | Name        | Description |
| :-------------------- | :---------- | :---------- |
| `Map<String, String>` | `mapOfURLs` | Map of URLs. |

**Returns**
`void`.
