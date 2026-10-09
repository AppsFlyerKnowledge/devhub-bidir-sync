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
The ad revenue configuration API lets you create, update, list, and delete ROI360 ad revenue partner integrations for your apps. Ad revenue data flows into AppsFlyer, so LTV, ROAS, and cohort metrics include ad revenue.

For each app and partner, choose one **integration type**:

| `integration_type`              | Description                                          | `credentials` | `source_event` |
| :------------------------------ | :--------------------------------------------------- | :------------ | :------------- |
| `aggregate_s2s`                 | Aggregate-level (S2S API)                            | Required      | Required       |
| `device_s2s`                    | Device-level (S2S API)                               | Required      | Not allowed    |
| `impression_sdk`                | Impression-level (SDK)                               | Not allowed   | Not allowed    |
| `impression_sdk_and_device_s2s` | Impression-level (SDK) with Device-level (S2S API)   | Required      | Not allowed    |

The `credentials` keys you must send depend on the partner network. See [Credentials by network](doc:credentials-by-network).

Partners connected through OAuth (for example Meta, Google Ad Manager, and Google AdMob aggregate) are configured in the AppsFlyer UI, not through this API.

## Authentication

Send the API V2 token as a bearer token in the `Authorization` header. Your AppsFlyer admin can retrieve the token from the AppsFlyer platform (HQ).

## Requests and responses

- Create, update, and delete requests are **atomic**: either every integration in the request is applied, or none is.
- Every response includes a `request_id`. Include it when you contact support.
- A failed request returns an `error` object with a `code` (`invalid_request`, `unauthorized`, `not_found`, or `internal_error`) and a `message`.

## Migrating from API v1.0

| v1.0                                                  | v2.0                                                           |
| :---------------------------------------------------- | :------------------------------------------------------------- |
| `POST /api/adrevenue/v1.0/integrations`               | `POST /api/adrevenue/v2.0/integrations`                        |
| `GET /api/adrevenue/v1.0/integrations`                | `GET /api/adrevenue/v2.0/integrations/app/{app_ids}`           |
| Not available                                         | `DELETE /api/adrevenue/v2.0/integrations/app/{app_ids}`        |
| `products[].name`: `attribution_app_level`            | `integration_type`: `aggregate_s2s`                            |
| `products[].name`: `attribution_user_level`           | `integration_type`: `device_s2s`                               |
| `products[].name`: `attribution_sdk_level`            | `integration_type`: `impression_sdk`                           |
| `products[].name`: `attribution_sdk_and_user_level`   | `integration_type`: `impression_sdk_and_device_s2s`            |
| `products[].enabled`                                  | Not supported. To disable an integration, delete it.           |

<Callout icon="⚠️" theme="warn">
  ### **Deprecation Notice**

  API v1.0 (`/api/adrevenue/v1.0/integrations`) is deprecated. Please migrate to v2.0 (`/api/adrevenue/v2.0/integrations`).
</Callout>
