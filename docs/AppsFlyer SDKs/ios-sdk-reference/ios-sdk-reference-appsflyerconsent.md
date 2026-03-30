---
title: AppsFlyerConsent
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Overview

`AppsFlyerConsent` encapsulates the properties for acquiring consent data required by the Digital Marketing Act (DMA) under the GDPR regulation.

## Initializers

### initForGDPRUser

**Input arguments**

| Type    | Name                                  | Description                                                                   |
| ------- | ------------------------------------- | ----------------------------------------------------------------------------- |
| Boolean | forGDPRUserWithHasConsentForDataUsage | Indicates whether the user consented to share for advertising purposes.       |
| Boolean | hasConsentForAdsPersonalization       | Indicates whether the user consented to share their data for ad optimization. |

**Usage examples**

```swift
// If the user is subject to GDPR - collect the consent data
// or retrieve it from the storage

// Set the consent data to the SDK:
var gdprConsent = AppsFlyerConsent(forGDPRUserWithHasConsentForDataUsage: true, hasConsentForAdsPersonalization: true) 
AppsFlyerLib.shared().setConsentData(gdprConsent)
```

### initForNonGDPRUser

**Input arguments**

None

**Usage examples**

```swift
// If the user is not subject to GDPR:
var nonGdprUser = AppsFlyerConsent(nonGDPRUser: ()) 
AppsFlyerLib.shared().setConsentData(nonGdprUser)
```