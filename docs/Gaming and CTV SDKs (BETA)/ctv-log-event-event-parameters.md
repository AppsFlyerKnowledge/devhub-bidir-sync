---
title: Log Event - Event Parameters
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
In each In-app event payload, the event parameters are sent as a JSON object which can contain multiple predefined event parameters and custom parameters.

## Predefined Parameter options ("event_parameters")

| Parameter Name | Type                   | Example                |
| :------------- | :--------------------- | :--------------------- |
| af_revenue     | Float                  | {“af_revenue” : 1.99}  |
| af_currency    | String - ISO 4217 code | {“af_currency”: “USD”} |

### Usage example

1. Predefined parameters ([examples of AF event name](https://dev.appsflyer.com/hc/docs/in-app-events-android#predefined-event-names)):

```json
{
  //...
  "event_name":"af_purchase",
  "event_parameters": {
    "af_currency":"USD",
    "af_revenue":12.12
  }
}
```

2. Predefined parameters and custom parameters ([examples of AF event parameters](https://dev.appsflyer.com/hc/docs/in-app-events-android#predefined-event-parameters)):

```json
{
  //...
  "event_name":"af_purchase",
  "event_parameters": {
    "af_currency":"USD",
    "af_revenue":12.12,
  },
  "event_custom_parameters": {
    "test_param": "test_data",
    "af_price":6.66,
    "player_level": 55
  }
}
```