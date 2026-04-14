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
Get your raw data reports in CSV files.

## Base URL

```http
https://hq1.appsflyer.com/api/raw-data/export/app/
```

## Endpoints

| Name                                                                                                                        | Path                                            | API Method | Description                                                                                                                                                                           |
| --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Raw data reports (non-organic)**                                                                                          |                                                 |            |                                                                                                                                                                                       |
| [Installs](https://dev.appsflyer.com/hc/reference/get_app-id-installs-report-v5)                                            | `/{app-id}/installs_report/v5`                  | `GET`      | Records non-organic installs. The record is created when a user opens the app for the first time. Data is updated in real-time.                                                       |
| [In-app events](https://dev.appsflyer.com/hc/reference/get_app-id-in-app-events-report-v5)                                  | `/{app-id}/in_app_events_report/v5`             | `GET`      | Records in-app events performed by users. Data is updated in real-time.                                                                                                               |
| [Uninstalls](https://dev.appsflyer.com/hc/reference/get_app-id-uninstall-events-report-v5)                                  | `/{app-id}/uninstall_events_report/v5`          | `GET`      | Records when a user uninstalls the app. Data is updated daily.                                                                                                                        |
| [Reinstalls](https://dev.appsflyer.com/hc/reference/get_app-id-reinstalls-v5)                                               | `/{app-id}/reinstalls/v5`                       | `GET`      | Records users who reinstall the app after uninstalling and engaging with a User Acquisition (UA) media source during the reattribution window. Data is updated in real-time.          |
| **Raw data reports (organic)**                                                                                              |                                                 |            |                                                                                                                                                                                       |
| [Organic Installs](https://dev.appsflyer.com/hc/reference/get_app-id-organic-installs-report-v5)                            | `/{app-id}/organic_installs_report/v5`          | `GET`      | Records when the app is opened by a user for the first time without attribution to an advertising source. Data is updated continuously.                                               |
| [Organic in-app events](https://dev.appsflyer.com/hc/reference/get_app-id-organic-in-app-events-report-v5)                  | `/{app-id}/organic_in_app_events_report/v5`     | `GET`      | Records details about events performed by users organically. Data is updated continuously.                                                                                            |
| [Organic uninstalls](https://dev.appsflyer.com/hc/reference/get_app-id-organic-uninstall-events-report-v5)                  | `/{app-id}/organic_uninstall_events_report/v5`  | `GET`      | Records users uninstalling the app without prior engagement with non-organic sources. Data is updated daily.                                                                          |
| [Organic reinstalls](https://dev.appsflyer.com/hc/reference/get_app-id-reinstalls-organic-v5)                               | `/{app-id}/reinstalls_organic/v5`               | `GET`      | Records organically attributed reinstalls of the app. Data is updated in real-time.                                                                                                   |
| **Retargeting**                                                                                                             |                                                 |            |                                                                                                                                                                                       |
| [Conversions (re-engagements & re-attributions)](https://dev.appsflyer.com/hc/reference/get_app-id-installs-retarget-v5)    | `/{app-id}/installs-retarget/v5`                | `GET`      | Records conversions (re-engagements & re-attributions) from retargeting campaigns. Data is updated in real-time.                                                                      |
| [In-app events retargeting](https://dev.appsflyer.com/hc/reference/get_app-id-in-app-events-retarget-v5)                    | `/{app-id}/in-app-events-retarget/v5`           | `GET`      | Records in-app events during re-engagement window triggered by retargeting campaigns. Data is updated in real-time.                                                                   |
| **Ad Revenue Raw data**                                                                                                     |                                                 |            |                                                                                                                                                                                       |
| [Attributed ad revenue](https://dev.appsflyer.com/hc/reference/get_app-id-ad-revenue-raw-v5)                                | `/{app-id}/ad_revenue_raw/v5`                   | `GET`      | Records ad revenue for users attributed to a media source. Data is updated daily.                                                                                                     |
| [Organic ad revenue](https://dev.appsflyer.com/hc/reference/get_app-id-ad-revenue-organic-raw-v5)                           | `/{app-id}/ad_revenue_organic_raw/v5`           | `GET`      | Records ad revenue for users not attributed to any media source. Data is updated daily.                                                                                               |
| [Retargeting ad revenue](https://dev.appsflyer.com/hc/reference/get_app-id-ad-revenue-raw-retarget-v5)                      | `/{app-id}/ad-revenue-raw-retarget/v5`          | `GET`      | Records ad revenue for users attributed to retargeting campaigns during the re-engagement window. Data is updated daily.                                                              |
| **Protect360 fraud**                                                                                                        |                                                 |            |                                                                                                                                                                                       |
| [Installs (Protect360 fraud)](https://dev.appsflyer.com/hc/reference/get_app-id-blocked-installs-report-v5)                 | `/{app-id}/blocked_installs_report/v5`          | `GET`      | Records installs identified as fraudulent and therefore not attributed to any media source. Data freshness: Real-time                                                                 |
| [Post-attribution installs](https://dev.appsflyer.com/hc/reference/get_app-id-detection-v5)                                 | `/{app-id}/detection/v5`                        | `GET`      | Reports include installs attributed to a media source but later found to be fraudulent. Data freshness: Real-time                                                                     |
| [In-app events (Protect360 fraud)](https://dev.appsflyer.com/hc/reference/get_app-id-blocked-in-app-events-report-v5)       | `/{app-id}/blocked_in_app_events_report/v5`     | `GET`      | Records in-app events identified as fraudulent by Protect360. Data freshness: Daily                                                                                                   |
| [Post-attribution in-app events](https://dev.appsflyer.com/hc/reference/get_app-id-fraud-post-inapps-v5)                    | `/{app-id}/fraud-post-inapps/v5`                | `GET`      | Records in-app events for installs identified as fraudulent after being attributed to a media source or judged fraudulent without regard to the install itself. Data freshness: Daily |
| [Clicks (Protect360 fraud)](https://dev.appsflyer.com/hc/reference/get_app-id-blocked-clicks-report-v5)                     | `/{app-id}/blocked_clicks_report/v5`            | `GET`      | Records clicks performed by users blocked by Protect360. Data freshness: Daily                                                                                                        |
| [Blocked install postbacks](https://dev.appsflyer.com/hc/reference/get_app-id-blocked-install-postbacks-v5)                 | `/{app-id}/blocked_install_postbacks/v5`        | `GET`      | Records install postbacks that were blocked due to being identified as fraudulent. Data updated in real time.                                                                         |
| **Postbacks**                                                                                                               |                                                 |            |                                                                                                                                                                                       |
| [Install postbacks](https://dev.appsflyer.com/hc/reference/get_app-id-postbacks-v5)                                         | `/{app-id}/postbacks/v5`                        | `GET`      | Records install events generated when a user opens the app for the first time. Data freshness: Daily                                                                                  |
| [In-app event postbacks](https://dev.appsflyer.com/hc/reference/get_app-id-in-app-events-postbacks-v5)                      | `/{app-id}/in-app-events-postbacks/v5`          | `GET`      | Records in-app event postbacks sent to the media source. Data freshness: Daily                                                                                                        |
| [Retargeting in-app event postbacks](https://dev.appsflyer.com/hc/reference/get_app-id-retarget-in-app-events-postbacks-v5) | `/{app-id}/retarget_in_app_events_postbacks/v5` | `GET`      | Records in-app events users performed during the re-engagement window. Data freshness: Real-time                                                                                      |
| [Retargeting conversions postbacks](https://dev.appsflyer.com/hc/reference/get_app-id-retarget-install-postbacks-v5)        | `/{app-id}/retarget_install_postbacks/v5`       | `GET`      | Records retargeting conversion postbacks sent to the media source. Data freshness: Real-time                                                                                          |

## Path parameters

The path parameters are identical for all raw data pull APIs. 

| Parameter | Data Type | Description    | Example    |
| --------- | --------- | -------------- | ---------- |
| `app_id*` | string    | Application ID. For non-mobile apps, use the unified app ID with the required platform prefix as specified in the list below. | `id121244` |

### Supported platform prefixes and examples
- `nativepc-`: Example: `nativepc-com.kurogame.pc.wutheringwaves`
- `steam-`: Example: `steam-3564740`
- `playstation-`: Example: `playstation-10011142`
- `roku-`: Example: `roku-43465`
- `epic-`: Example: `epic-fghi45674unnrAspVxkT5bJgoNo2dPfK`
- `xbox-`: Example: `xbox-9N8PMW7QMD3D`
- `tizen-`: Example: `tizen-G19068012619`
- `smartcast-`: Example: `smartcast-vzfubo`
- `webos-`: Example: `webos-com.fubotv.app`
- `quest-`: Example: `quest-6442996282466138`
- `vidaa-`: Example: `vidaa-07121931`
- `switch-`: Example: `switch-75158121`
- `battlenet-`: Example: `battlenet-75158120`
- `chatgpt-`: Example: `chatgpt-75157577`


## Query parameters