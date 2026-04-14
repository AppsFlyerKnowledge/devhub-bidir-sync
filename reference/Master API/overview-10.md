---
title: Overview
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Get selected LTV, activity, retention, cohort, and Protect360 campaign performance KPIs in CSV or JSON format.

- Lets you get selected LTV, activity, retention, cohort, and Protect360 campaign performance KPIs. The KPIs available are the equivalent KPIs of those found in the Overview, Activity, Retention, Cohort, and Protect360 dashboards.
- Is calculated daily. The updated data is available to you within 24-48 hours, depending on your app-specific timezone.

## Authentication

A Pull API authentication token (key) is required to use Pull API. See [API token instructions](https://support.appsflyer.com/hc/en-us/articles/360004562377).

## Endpoint

```http
GET https://hq1.appsflyer.com/api/master-agg-data/v4/app/all?groupings=c&kpis=clicks,impressions
```

## Path parameters

The following path parameters are included:

| Parameter   | Data Type | Description                       | Example     |
|-------------|-----------|-----------------------------------|-------------|
| `app-id*`   | string    | Selected or all Application IDs   | id1234567   |

## Query parameters

The following query parameters are included:

| Name               | Type           | Description                                                                | Example                                 |
|--------------------|----------------|----------------------------------------------------------------------------|-----------------------------------------|
| `app-id`           | string or array| Single app id or multiple app ids that are passed in the path. "All" or Array of specific app IDs are supported.| "all" or ["com.example.testapp"]        |
| `from`             | string         | Lower bound of the LTV attribution date range. Format: `yyyy-mm-dd` | "2022-04-22"                             |
| `to`               | string         | Higher bound of the LTV attribution date range. Number of the days in the range: 1-31 days. For a single day: `from` and `to` values are identical. Format: `yyyy-mm-dd`| "2022-04-22" |
| `groupings`        | array          | Group by the [dimensions below](#group_dimenstions).                      | -                                       |
| `kpis`             | array          | Include [the KPIs below](#kpis).                                           | -                                       |
| `calculated_kpis`  | object         | Custom calculated KPIs that can be included in reports.For example<br> **First three days combined retention**<br>`calculated_kpi_3days_retention=retention_day_1%2Bretention_day_2%2Bretention_day_3`<br>**Average revenue per impression**<br>`calculated_kpi_rev_per_impression=revenue%2Fimpression`<br>**Cohort day 7 ROI**<br>`calculated_kpi_roi_day_7=(cohort_day_7_total_revenue_per_user-average_ecpi)%2Faverage_ecpi`                   |  |
## Group dimensions

The dimensions in the table below are used for collecting the data into groups to allow an easier and more accurate examination of the information. You can find descriptions of these fields [here](https://support.appsflyer.com/hc/en-us/articles/208387843).

| Group by API name       | Group by display name   | LTV KPIs | Retention KPIs | Activity KPIs | Protect360 | Cohort |
|-------------------------|-------------------------|----------|----------------|---------------|------------|--------|
| `app_id`                | App ID                  | Yes      | Yes            | Yes           | Yes        | Yes    |
| `pid`                   | Media source            | Yes      | Yes            | Yes           | Yes        | Yes    |
| `af_prt`                | Agency                  | Yes      | Yes            | Yes           | Yes        | No     |
| `c`                     | Campaign                | Yes      | Yes            | Yes           | Yes        | Yes    |
| `af_adset`              | Adset                   | Yes      | Yes            | Yes           | No         | No     |
| `af_ad`                 | Ad                      | Yes      | Yes            | Yes           | No         | No     |
| `af_channel`            | Channel                 | Yes      | Yes            | Yes           | Yes        | No     |
| `af_siteid`             | Publisher ID            | Yes      | Yes            | Yes           | Yes        | Yes    |
| `af_keywords`           | Keywords                | Yes      | Yes            | Yes           | No         | No     |
| `is_primary`            | Is primary attribution  | Yes      | No             | Yes           | Yes        | No     |
| `af_c_id`               | Campaign ID             | Yes      | No             | Yes           | Yes        | No     |
| `af_adset_id`           | Adset ID                | Yes      | No             | Yes           | No         | No     |
| `af_ad_id`              | Ad ID                   | Yes      | No             | Yes           | No         | No     |
| `install_time`          | Install Time            | Yes      | Yes            | Yes*          | Yes        | Yes    |
| `attributed_touch_type` | Touch Type              | Yes      | Yes            | Yes           | Yes        | No     |
| `geo`                   | Geo                     | Yes      | Yes            | Yes           | Yes        | Yes    |

* In the context of Activity KPIs, regard install time as event time.

## KPIs

### LTV KPIs

| KPI                                 | Description                                                                                      |
|-------------------------------------|--------------------------------------------------------------------------------------------------|
| `impressions`                       | Number of impressions within the selected time frame                                             |
| `clicks`                            | Number of clicks within the selected time frame                                                   |
| `installs`                          | Number of installs within the selected time frame                                                 |
| `cr`                                | Conversion Rate                                                                                   |
| `sessions`                          | Number of sessions created by the users who installed within the selected time frame              |
| `loyal_users`                       | Number of loyal users who installed within the selected time frame                                |
| `loyal_users_rate`                  | Loyal users/installs                                                                              |
| `cost`                              | [Total cost in the selected time frame. See limitations.](https://support.appsflyer.com/hc/en-us/articles/213223166#limitations) |
| `revenue`                           | Lifetime revenue generated by the users who installed in the selected time frame                  |
| `roi`                               | Return on Investment over a certain time frame                                                    |
| `arpu_ltv`                          | Average revenue per user, for the users who installed in the selected time frame                  |
| `average_ecpi`                      | Effective Cost per Installation (eCPI) over a certain time frame. Available only if cost and installs are included in the call. |
| `uninstalls`                        | Uninstalling users, who installed in the selected time frame                                      |
| `uninstalls_rate`                   | Uninstallation rate                                                                               |
| `event_counter_[event_name]`        | Number of event occurrences                                                                       |
| `unique_users_[event_name]`         | Number of unique users who performed the event                                                    |
| `sales_in_usd_[event_name]`         | Revenue reported as part of the reported events                                                   |

### Retention KPIs

| Description                                     | KPI                  |
|-------------------------------------------------|----------------------|
| Number of retained users at day X               | `retention_day_[x]`  |
| Number of retained users at day X out of installing users | `retention_rate_day_[x]` |

### Activity KPIs

| KPI                                        | Description                                                                                               |
|--------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| `activity_average_dau`                     | Average daily active users (DAU) within the selected time frame                                           |
| `activity_average_mau`                     | Average monthly active users within the selected time frame (one MAU day represents the unique users in the preceding 30 days) |
| `activity_average_dau_mau_rate`            | Average DAU/MAU rate                                                                                      |
| `activity_average_arpdau`                  | Average revenue per daily active user - the average revenue of a given day out of all unique users        |
| `activity_sessions`                        | Number of sessions performed within the selected time frame                                               |
| `activity_revenue`                         | Revenue reported within the selected time frame                                                           |
| `activity_event_counter_[event_name]`      | Number of events generated by users within the selected time frame                                        |
| `activity_sales_in_usd_[event_name]`       | Revenue reported as part of the reported events within the selected time frame                            |
| `activity_average_unique_users_[event_name]` | Average unique users performing a given event within the selected time frame                              |
### Cohort KPIs

| KPI                                                   | Description                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `cohort_day_[x]_total_sessions_per_user`                 | Cohort Day x - Cumulative sessions per user up to day x (including day x)                                                                                                                                                                                                                     |
| `cohort_day_[x]_sessions_per_user`                       | Cohort Day x - Sessions on day x only from the cohort                                                                                                                                                                                                                                         |
| `cohort_[x]_days_total_sessions_per_user`                | Replaces specifying KPIs Cohort_day_1_total_sessions_per_user to Cohort_day_x_total_sessions_per_user on the URL. For example: cohort_3_days_total_sessions_per_user on the URL produces 3 report columns: Cohort_day_1_total_sessions_per_user+ Cohort_day_2_total_sessions_per_user+Cohort_day_3_total_sessions_per_user |
| `REVENUE`                                             |                                                                                                                                                                                                                                                                                              |
| `cohort_day_[x]_total_revenue_per_user`                  | Cohort Day x - Cumulative revenue per user up to day x (including day x)                                                                                                                                                                                                                      |
| `cohort_day_[x]_revenue_per_user`                        | Cohort Day x - ARPU received on day x only from the cohort                                                                                                                                                                                                                                    |
| `cohort_[x]_days_total_revenue_per_user`                 | Replaces specifying KPIs Cohort_day_1_total_revenue_per_user to Cohort_day_x_total_revenue_per_user. For example: cohort_3_days_total_revenue_per_user on the URL produces 3 report columns: Cohort_day_1_total_revenue_per_user+ Cohort_day_2_total_revenue_per_user+ Cohort_day_3_total_revenue_per_user                |
| `cohort_day_[x]_total_event_[eventname]_revenue_per_user` | Cohort Day x accumulative revenue per user according to specific in-app event                                                                                                                                                                                                                  |
| `cohort_day_[x]_event_[eventname]_revenue_per_user`      | Cohort Day x revenue per user according to specific in-app event                                                                                                                                                                                                                              |
| `EVENTS`                                              |                                                                                                                                                                                                                                                                                              |
| `cohort_day_[x]_total_event_[eventname]_per_user`        | Cohort Day x - Cumulative events per user up to day x (including day x)                                                                                                                                                                                                                       |
| `cohort_day_[x]_event_[eventname]_per_user`              | Cohort Day x - Events received on day x only from the cohort                                                                                                                                                                                                                                  |
| `cohort_[x]_days_total_event_[eventname]_per_user`       | Replaces specifying KPIs events to Cohort_day_x_total_events_per_user. For example: cohort_3_days_total_events_per_user on the URL produces 3 report columns: Cohort_day_1_total_events_per_user+ Cohort_day_2_total_events_per_user+Cohort_day_3_total_events_per_user                     |

### Protect360 KPIs

| Description                                           | KPI                                                       |
| ----------------------------------------------------- | --------------------------------------------------------- |
| **Installs**                                          |                                                           |
| Total                                                 | `protect360_total_installs`                               |
| Blocked                                               | `blocked_installs`                                        |
| Blocked %                                             | `blocked_installs_rate`                                   |
| Post-attribution                                      | `post_attribution_installs`                               |
| Post-attribution %                                    | `post_attribution_installs_rate`                          |
| Total fraudulent installs                             | `total_fraudulent_installs`                               |
| Fraudulent installs %                                 | `fraudulent_installs_rate`                                |
| **Fake installs**                                     |                                                           |
| Real-time block                                       | `real_time_fake_installs`                                 |
| Post-attribution fraud                                | `post_attribution_fake_installs`                          |
| **Hijacked installs**                                 |                                                           |
| Real-time block                                       | `real_time_hijacked_installs`                             |
| Post-attribution fraud                                | `post_attribution_installs_hijacked_installs`             |
| **Validation rules**                                  |                                                           |
| Blocked installs                                      | `validation_rules_blocked_installs`                       |
| Blocked attribution                                   | `validation_rules_blocked_attribution`                    |
| **Fake installs block breakdown**                     |                                                           |
| Blocked site ID blacklist                             | `blocked_installs_siteid_blacklist`                       |
| Post-attribution site ID blacklist                    | `post_attribution_installs_siteid_blacklist`              |
| Blocked bots                                          | `blocked_installs_bots`                                   |
| Post-attribution bots                                 | `post_attribution_installs_bots`                          |
| Blocked behavioral anomalies                          | `blocked_installs_behavioral_anomalies`                   |
| Post-attribution behavioral anomalies                 | `post_attribution_installs_behavioral_anomalies`          |
| Blocked install validation                            | `blocked_installs_install_validation`                     |
| **Hijacked installs block breakdown**                 |                                                           |
| Blocked install hijacking                             | `blocked_installs_install_hijacking`                      |
| Post-attribution install hijacking                    | `post_attribution_installs_installs_hijacking`            |
| Blocked CTIT anomalies                                | `blocked_installs_ctit_anomalies`                         |
| Post-attribution CTIT anomalies                       | `post_attribution_installs_ctit_anomalies`                |
| Blocked click flooding                                | `blocked_installs_click_flood`                            |
| Post-attribution click flooding                       | `post_attribution_installs_click_flood`                    |
| **Clicks**                                            |                                                           |
| Total                                                 | `protect360_total_clicks`                                 |
| Blocked                                               | `blocked_clicks`                                          |
| %                                                     | `blocked_clicks_rate`                                     |
| **In-App Events**                                     |                                                           |
| Total                                                 | `protect360_total_in_apps`                                |
| Blocked                                               | `blocked_in-app-events`                                   |
| %                                                     | `blocked_in-app-events_rate`                              |
| **Device farm indicators - new devices**             |                                                      |
| Installs                                              | `device_farm_new_devices_installs`                   |
| Blocked installs                                      | `blocked_device_farm_new_devices`                    |
| Post-attribution installs                             | `post_attribution_device_farm_new_devices`           |
| **Device farm indicators - click flooding**          |                                                      |
| Clicks                                                | `device_farm_click_flooding_clicks`                  |
| Blocked clicks                                        | `blocked_device_farm_click_flooding`                 |
| Post-attribution clicks                               | `post_attribution_device_farm_click_flooding`        |
| **Device farm indicators - in-app events**           |                                                      |
| In-app events                                         | `device_farm_in_app_events`                          |
| Blocked in-app events                                 | `blocked_device_farm_in_app_events`                  |
| Post-attribution in-app events                        | `post_attribution_device_farm_in_app_events`         |