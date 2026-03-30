---
title: Measure sessions
excerpt: >-
  > ℹ️ This is  API is used to to report events directly from your application's
  client. If you need Server-to-Server please see [this
  section](https://dev.appsflyer.com/hc/reference/post_session-app-platform-app-id)         


  This AppsFlyer API integration provide advertisers with the ability

  to measure post-install sessions and attribute it to a media source and
  campaign


  > 📘 Note

  >

  > ### Generate the authentication signature

  >

  > To authenticate the API request, generate a Hash-Based Message
  Authentication Code (HMAC) signature by hashing the entire raw message using
  SHA-256 and the Devkey as the secret key. Place the resulting signature in the
  request authentication header.

  >

  > For example, if the raw message is:

  > `{"timestamp": 1604503583337, "ip": "127.0.0.1", "user_agent":
  "Roku/DVP-9.21 (AE9.21E04090A)", "device_os_version": "9.3.0", "device_model":
  "3920X", "device_ids": [{"type": "rida", "value":
  "fa73d67d-f55c-5af3-883a-726253dc7d0e"}], "limit_ad_tracking": false}`

  >

  > The Devkey is:

  > `w8sZzy8EaPaxFKfaoTqU16`

  >

  > The signature is:

  > 715b133d5a327b77beb898688a3ad438da0f78071cd3df70e8ec5ecf60751972
api:
  file: pcconsolectv-client-app-events-api.json
  operationId: ctv-c2s-events-sessions-post
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---