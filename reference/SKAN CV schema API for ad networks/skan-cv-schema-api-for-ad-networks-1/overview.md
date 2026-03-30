---
title: Overview
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
With SKAdNetwork (SKAN) attribution, advertisers grade user quality by setting conversion values (CV) based on post-install user activity during specific time periods (windows). [Learn more about SKAN](https://support.appsflyer.com/hc/en-us/articles/360011420698)

iOS reports the CV via postbacks sent to the ad network placing the ad. Starting iOS 15 advertisers can also instruct iOS to send the postbacks directly to AppsFlyer.

This API provides ad networks the CV-to-attribution-events-and-values map. Ad networks use it to decode the postbacks that they receive.

The API returns as a nested JSON. The schema for each app is separate. The mappings per CV are contained within the conversion_value_mapping array.

For a list of keys and their description, click 200 in the simulator.

**Note**:

- The CV value range is:
   - SKAN 4 window 1 fine value: 0-63.
   - SKAN 4 windows 1, 2, and 3 coarse value: low, medium, or high.
   - SKAN 3 and below: 0-63
- Advertisers can change the CV mapping at any time.
- You, the ad network, must query this API daily, to ensure that you have the most updated schema.