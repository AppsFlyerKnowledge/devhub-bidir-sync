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

```java
public class CrossPromotionHelper
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

```java
public static void logAndOpenStore(@NonNull Context context,
                                       String promoted_app_id,
                                       String campaign,
                                       Map<String, String> userParams)
```

**Description**
Attributes the click and opens the promoted app's Play Store listing. Learn more in [Cross-promotion](doc:cross-promotion-android).

**Input arguments**

| Type                  | Name              | Description                       |
| :-------------------- | :---------------- | :-------------------------------- |
| `Context`             | `context`         | Application / Activity Context.   |
| `String`              | `promoted_app_id` |                                   |
| `String`              | `campaign`        | Name of cross-promotion campaign. |
| `Map<String, String>` | `userParams`      | Optional.                         |

**Returns**
`void`.

### logCrossPromoteImpression

**Method signature**

```java
public static void logCrossPromoteImpression(@NonNull Context context,
                                                 String appID,
                                                 String campaign,
                                                 Map<String, String> userParams)
```

**Description**

**Input arguments**

| Type                  | Name         | Description                       |
| :-------------------- | :----------- | :-------------------------------- |
| `Context`             | `context`    | Application / Activity Context.   |
| `String`              | `appID`      |                                   |
| `String`              | `campaign`   | Name of cross-promotion campaign. |
| `Map<String, String>` | `userParams` | Optional.                         |

**Returns**
`void`.

### setUrl

**Method signature**

```java
public static void setUrl(Map<String, String> mapOfURLs)
```

**Description**

**Input arguments**

| Type                  | Name        | Description |
| :-------------------- | :---------- | :---------- |
| `Map<String, String>` | `mapOfURLs` |             |

**Returns**
`void`.
