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
In SKAdNetwork (SKAN) attribution advertisers grade user quality by setting a conversion value (CV) based on post-install user activity during an activity window of 12–72 hours. (Default: 24 hours.)

iOS reports the CV via a postback sent to the ad network placing the ad. Starting iOS 15 advertisers can also instruct iOS to send the postback directly to AppsFlyer. 

This API provides ad networks the CV-to-attribution-events-and-values map. Ad networks use it to decode the postbacks that they receive.

The API returns as a nested JSON. The schema for each app is separate. The mappings per CV are contained within the conversion_value_mapping array.

For a list of keys and their description, click 200 in the simulator.

Related reading: [SKAN solution guide](https://support.appsflyer.com/hc/en-us/articles/360011420698) 

* The CV value range is 0-63.
* Advertisers can change the CV mapping at any time.
* You, the ad network, must query this API daily, to ensure that you have the most updated schema.
* Consider that the postback is sent by iOS 24-120 hours after the install itself. The delay depends on the duration of the activity period and user activity in the app.