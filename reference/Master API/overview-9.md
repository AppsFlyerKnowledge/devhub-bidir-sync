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
Master API:

 - Lets you get selected LTV, activity, retention, cohort, and Protect360 campaign performance KPIs. The KPIs available are the equivalent KPIs of those found in the Overview, Activity, Retention, Cohort, and Protect360 dashboards.
 - Is calculated daily. The updated data is available to you within 24-48 hours, depending on your app-specific timezone.
 - Is the infrastructure upon which the AppsFlyer pivot table is built. 
 - Isn't available for agencies and partners.

To use Master API, you define the data that you want to view (similar to the implementation of the Pull API). The result returns as a CSV or JSON file.

**Prerequisite**: Get the [API token](https://support.appsflyer.com/hc/en-us/articles/360004562377) from your marketer, to use in the API authorization header.