---
title: Create OneLink attribution link
excerpt: >-
  # Example:

  Feed Me, a grocery delivery service, 

  wants to send a personalized link via SMS to existing customers to

  encourage them to download the Feed Me app and buy bananas. 

  The developer writes a program to scan Feed Me's database that uses each line
  of data

  to create OneLink custom attribution links. The following JSON displays data 

  in the body params that are used to create a OneLink custom attribution link
  for a Canadian user."

  ```

  {
    "af_ad": "yellow_bananas",
    "af_adset": "my_adset",
    "af_android_url": "https://feedme.ca/buybananas",
    "af_channel": "my_channel",
    "af_dp": "afbasicapp://mainactivity",
    "af_ios_url": "https://feedme.ca/buybananas",
    "c": "my_campaign",
    "deep_link_value": "bananas",
    "deep_link_sub1": 10,
    "is_retargeting": true,
    "pid": "my_media_source_SMS"
  }

  ```
api:
  file: onelink-api-2.json
  operationId: create-onelink-attribution-link
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---