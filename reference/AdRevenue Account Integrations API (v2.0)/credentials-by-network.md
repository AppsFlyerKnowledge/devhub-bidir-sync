---
title: Credentials by network
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
The `credentials` object of `POST /integrations` is a flat map of string keys to string values. The keys you must send depend on the partner `network`.

## Rules

- Send `credentials` for `aggregate_s2s`, `device_s2s`, and `impression_sdk_and_device_s2s`. Do not send it for `impression_sdk`.
- Send **every** required key listed for the network, whichever of those three integration types you use. A missing key fails the whole request.
- Key names are case-sensitive, lowercase, with underscores. Values are strings.
- Keys that are not listed for the network are ignored. A misspelled key is reported as a missing key.

## Required credential keys

✅ means the network supports that `integration_type`.

| `network`         | `aggregate_s2s` | `device_s2s` | `impression_sdk` | `impression_sdk_and_device_s2s` | Required `credentials` keys                                  |
| :---------------- | :-------------: | :----------: | :--------------: | :-----------------------------: | :----------------------------------------------------------- |
| `ironsource`      |        ✅        |      ✅       |        ✅         |                ✅                | `username`, `secret_key`, `network_app_id`, `refresh_token`  |
| `fyber`           |        ✅        |      ✅       |        ✅         |                ✅                | `network_app_id`, `client_id`, `client_secret`               |
| `topon`           |        ✅        |      ✅       |        ✅         |                ✅                | `publisher_key`, `network_app_id`                            |
| `appodeal`        |        ✅        |      ✅       |        ✅         |                ✅                | `api_key`, `user_id`, `app_key`, `network_app_id`            |
| `applovinmax`     |                 |      ✅       |        ✅         |                ✅                | `package_name`, `report_key`                                 |
| `tradplus`        |                 |      ✅       |        ✅         |                ✅                | `api_key`, `network_app_id`                                  |
| `admost`          |                 |      ✅       |        ✅         |                ✅                | `network_app_id`, `token`                                    |
| `chartboost`      |        ✅        |              |        ✅         |                                 | `network_app_id`, `user_id`, `user_signature`                |
| `tapjoy`          |                 |      ✅       |                  |                                 | `network_app_id`, `api_key`                                  |
| `odeeo`           |                 |      ✅       |                  |                                 | `api_key`                                                    |
| `applovin`        |        ✅        |              |                  |                                 | `filter_package_name`, `api_key`                             |
| `vungle`          |        ✅        |              |                  |                                 | `api_key`, `application_id`                                  |
| `mintegral`       |        ✅        |              |                  |                                 | `skey`, `secret`, `network_app_id`                           |
| `bytedance`       |        ✅        |              |                  |                                 | `network_app_id`, `secure_key`, `account_id`                 |
| `bytedanceglobal` |        ✅        |              |                  |                                 | `network_app_id`, `secure_key`, `account_id`                 |
| `unityads`        |        ✅        |              |                  |                                 | `api_key`, `source_ids`. Optional: `organization_id`         |

### Networks with no credentials

These networks support `impression_sdk` only, so do not send `credentials`: `toponpte`, `yandex`, `custom_mediation`, and `googleadmob`.

### Networks that are not available through this API

`facebook`, `doubleclick`, `vidcoin`, `inmobi`, and `unityadsmediation` cannot be configured through this API. Configure them in the AppsFlyer UI.

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
