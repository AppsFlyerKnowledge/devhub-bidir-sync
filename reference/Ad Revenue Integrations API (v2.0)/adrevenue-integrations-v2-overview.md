---
title: Overview
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
The Ad Revenue Integrations API lets you create, update, get, and delete ad revenue partner integrations for one or more apps in a single call. Ad revenue data flows into AppsFlyer, so LTV, ROAS, and cohort metrics include ad revenue.

## Integration types

For each app and partner, choose one integration type:

| `integration_type`              | Description                                                  | `credentials` | `source_event` |
| :------------------------------ | :----------------------------------------------------------- | :------------ | :------------- |
| `aggregate_s2s`                 | Aggregate level via S2S API                                  | Required      | Required       |
| `device_s2s`                    | Device-level via S2S API                                     | Required      | Not allowed    |
| `impression_sdk`                | Impression-level via SDK                                     | Not allowed   | Not allowed    |
| `impression_sdk_and_device_s2s` | Impression-level (via SDK) with device-level (via S2S API)   | Required      | Not allowed    |

To learn more about each integration type, see [Integration types for ad revenue attribution](https://support.appsflyer.com/hc/en-us/articles/217490046-ROI360-guide-ad-revenue-attribution).

The `credentials` keys you must send depend on the partner. See [Partner credentials](doc:adrevenue-integrations-v2-credentials).

Aggregate level (`aggregate_s2s`) integrations for Meta ads, Google AdMob, and Google Ad Manager use OAuth, so you can set them up only in the AppsFlyer platform.

## Source event

`aggregate_s2s` requires a `source_event`: the name of an in-app event of the app, for example `af_app_opened`. The ad revenue is recorded as a new event named `<source_event>_monetized`, for example `af_app_opened_monetized`.

- Use the name of an in-app event that your app already sends to AppsFlyer. There is no fixed list of values.
- Do not add the `_monetized` suffix yourself. A `source_event` that ends with `_monetized` is rejected.
- Do not send `source_event` for the other integration types.

## Authentication

Send the [API V2 token](https://support.appsflyer.com/hc/en-us/articles/360004562377) as a bearer token in the `Authorization` header. Only the account admin can retrieve the token.

## When changes take effect

| Action                                                              | Takes effect                    |
| :------------------------------------------------------------------ | :------------------------------ |
| Create a new integration                                            | Immediately                     |
| Update an existing integration (including a change of integration type) | At 00:00 UTC the following day |
| Delete an integration                                               | Immediately                     |

## Requests and responses

- `POST /integrations` creates an integration, or fully replaces an existing one for the same app and network (integration type, credentials, and `source_event`).
- Create, update, and delete requests are **atomic**: either every integration in the request is applied, or none is.
- Send up to 1000 integrations in a single create or update request. For larger sets, split them into several requests.
- Every response includes a `request_id`. Include it when you contact support.
- A failed request returns an `error` object with a `code` (`invalid_request`, `unauthorized`, `not_found`, or `internal_error`) and a `message`.

## Migrating from API v1.0

API v1.0 is no longer available. If you used v1.0, use this table to move to v2.0.

| v1.0                                                  | v2.0                                                           |
| :---------------------------------------------------- | :------------------------------------------------------------- |
| `POST /api/adrevenue/v1.0/integrations`               | `POST /api/adrevenue/v2.0/integrations`                        |
| `GET /api/adrevenue/v1.0/integrations`                | `GET /api/adrevenue/v2.0/integrations/app/{app_ids}`           |
| Not available                                         | `DELETE /api/adrevenue/v2.0/integrations/app/{app_ids}`        |
| `network`: values with `_int` (for example `ironsource_int`, `sponsorpay_int`) | `network`: new values (for example `ironsource`, `fyber`). See [Partner credentials](doc:adrevenue-integrations-v2-credentials). |
| `products[]` (list per integration)                   | One `integration_type` per app and network                     |
| `products[].name`: `attribution_app_level`            | `integration_type`: `aggregate_s2s`                            |
| `products[].name`: `attribution_user_level`           | `integration_type`: `device_s2s`                               |
| `products[].name`: `attribution_sdk_level`            | `integration_type`: `impression_sdk`                           |
| `products[].name`: `attribution_sdk_and_user_level`   | `integration_type`: `impression_sdk_and_device_s2s`            |
| `products[].enabled`                                  | Not supported. To disable an integration, delete it.           |
| `event_name`                                          | Not supported.                                                 |
