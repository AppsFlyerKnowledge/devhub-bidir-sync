---
title: AppsFlyerInAppPurchaseValidatorListener
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
## Overview
Implement the `AppsFlyerInAppPurchaseValidatorListener` interface to handle purchase validation success and failure.

Go back to the [SDK reference index](doc:android-sdk-reference).

**Interface declaration**
```java
public interface AppsFlyerInAppPurchaseValidatorListener
```
## Methods
### onValidateInApp
**Method signature**
```java
void onValidateInApp()
```

**Description**
Triggered upon successful purchase validation.

**Returns**
`void`

### onValidateInAppFailure
**Method signature**
```java
void onValidateInAppFailure(java.lang.String error)
```

**Description**
Triggered upon failed purchase validation.

**Callback parameters**

| Type | Name | Description |
|:--------|:-----------------|:--------------|
| `String` | `error` | The type of error that occurred during validation. |

**Returns**
`void`