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
> 🚧 This is a server to server API. If you need to report events directly from your application's client please see [this section](doc:c2s-integrations-overview) or contact your CSM

**At a glance**: The AppsFlyer CTV API allows advertisers to measure CTV first app opens (installs), in-app events, and sessions via server-to-server API. All calls are made via secure HTTP.

The AppsFlyer CTV API supports the following platforms:

- Roku
- Samsung Tizen
- Vizio SmartCast
- LG Webos
- PlayStation
- Vidaa
- Steam
- Meta Quest
- Battlenet
- Epic (Closed beta)
- Switch (Closed beta)
- Xbox (Closed beta)

## Integrate AppsFlyer CTV API

**To integrate the API:**

1. Follow the procedures for the following API commands:
   - [Measure first app opens](https://dev.appsflyer.com/hc/reference/post_first-open-app-platform-app-id)  
     **Note**: This API call must be made for every first app open. And first app opens must be reported before in-app events and sessions.
   - [Measure in-app events](https://dev.appsflyer.com/hc/reference/post_inapp-app-platform-app-id)
   - [Measure sessions](https://dev.appsflyer.com/hc/reference/post_session-app-platform-app-id)  
     **Note**: First app opens must be reported before in-app events and sessions.
2. Set up the CTV events for AppsFlyer.
   - Find out from your marketer which events they want to be sent to AppsFlyer.

## Test the integration

**To test the integration**: 

Use each of the API commands to send several events as follows:

1. In the API, set the **Base URL** to the sandbox option. 
2. Send an event.
3. Verify that you get a 200 OK return code.

If you're testing the production environment (not using the sandbox), ask the marketer to confirm the data is available in AppsFlyer.