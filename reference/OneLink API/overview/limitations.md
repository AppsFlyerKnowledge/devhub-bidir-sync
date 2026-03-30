---
title: Limitations
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
OneLink API has the following limitations:

  * Links created via the API do not appear in the list of OneLink custom links in the AppsFlyer dashboard.
  * Each OneLink attribution link created by the API has a default Time to Live (TTL) of 31 days. After 31 days this attribution link record is removed from our systems. Clicking on such an attribution Link once the TTL expires still defaults to the behavior defined in OneLink base Configuration. Maximum TTL is 31 days.
  * Any TTL value larger than 31 is overridden with the default TTL of 31.
  * TTL value can be specified in days (default), minutes or hours (e.g., 10m, 20h, 14d).
  * It is not possible to use the following special characters in the OneLink API data payload: “&” 
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "  * Use the OneLink API only with links created by it.\n  * Don't use OneLink API to update or delete shortened OneLink URLs created using the Link management page. It may break live URLs. "
}
[/block]