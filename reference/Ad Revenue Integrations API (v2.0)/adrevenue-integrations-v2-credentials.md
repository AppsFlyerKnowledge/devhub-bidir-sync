---
title: Partner credentials
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
The `credentials` object of `POST /integrations` is a flat map of string keys to string values. The keys you must send depend on the partner.

For the list of supported partners and the integration types each one supports, see [List of ad revenue integrated partners](https://support.appsflyer.com/hc/en-us/articles/217490046-ROI360-guide-ad-revenue-attribution#af-accordion-13).

## Rules

- Send `credentials` for `aggregate_s2s`, `device_s2s`, and `impression_sdk_and_device_s2s`. Do not send it for `impression_sdk`.
- Send **every** required key listed for the network, whichever of those three integration types you use. A missing key fails the whole request.
- Key names are case-sensitive, lowercase, with underscores. Values are strings.
- Keys that are not listed for the network are ignored. A misspelled key is reported as a missing key.

## Required credential keys

| Partner                       | `network`         | Required `credentials` keys                                  |
| :---------------------------- | :---------------- | :----------------------------------------------------------- |
| Admost                        | `admost`          | `network_app_id`, `token`                                    |
| AppLovin (network)            | `applovin`        | `filter_package_name`, `api_key`                             |
| AppLovin MAX                  | `applovinmax`     | `package_name`, `report_key`                                 |
| Appodeal                      | `appodeal`        | `api_key`, `user_id`, `app_key`, `network_app_id`            |
| Bytedance Ads (China traffic) | `bytedance`       | `network_app_id`, `secure_key`, `account_id`                 |
| Chartboost                    | `chartboost`      | `network_app_id`, `user_id`, `user_signature`                |
| Fyber                         | `fyber`           | `network_app_id`, `client_id`, `client_secret`               |
| IronSource (Unity LevelPlay)  | `ironsource`      | `username`, `secret_key`, `network_app_id`, `refresh_token`  |
| Mintegral                     | `mintegral`       | `skey`, `secret`, `network_app_id`                           |
| Odeeo                         | `odeeo`           | `api_key`                                                    |
| Pangle (TikTok for Business)  | `bytedanceglobal` | `network_app_id`, `secure_key`, `account_id`                 |
| Tapjoy                        | `tapjoy`          | `network_app_id`, `api_key`                                  |
| Topon                         | `topon`           | `publisher_key`, `network_app_id`                            |
| Tradplus                      | `tradplus`        | `api_key`, `network_app_id`                                  |
| Unity Ads                     | `unityads`        | `api_key`, `source_ids`. Optional: `organization_id`         |
| Vungle                        | `vungle`          | `api_key`, `application_id`                                  |

Aggregate level (`aggregate_s2s`) integrations for Meta ads, Google AdMob, and Google Ad Manager use OAuth, so you can set them up only in the AppsFlyer platform.

## Example

```json
{
  "integrations": [
    {
      "app_id": "com.example.myapp",
      "network": "ironsource",
      "integration_type": "device_s2s",
      "credentials": {
        "username": "YOUR_USERNAME",
        "secret_key": "YOUR_SECRET_KEY",
        "network_app_id": "YOUR_NETWORK_APP_ID",
        "refresh_token": "YOUR_REFRESH_TOKEN"
      }
    }
  ]
}
```

If a key is missing, the request returns `400` with a message such as:

```text
integration 0, appID='com.example.myapp', network='ironsource': invalid credentials - missing field: refresh_token
```
