---
title: iOS sample payloads
deprecated: false
hidden: false
icon: 😀
link:
  new_tab: false
metadata:
  title: ''
  description: ''
  robots: index
---
See the following sample payloads for Universal Links, URI schemes, and deferred deep linking. The samples contain a full payload, relevant for when all parameters in the Onelink custom link setup page  contain data.

**Note**: Payloads return as a map. However, for clarity, the sample payloads that follow are displayed in JSON form.

## Universal Links

Input to `onAppOpenAttribution(_ attributionData: [AnyHashable: Any])`

```json Short link
{
   "af_ad": "my_adname",
   "af_adset": "my_adset",
   "af_android_url": "https://isitchristmas.com/",
   "af_channel": "my_channel",
   "af_click_lookback": "20d",
   "af_cost_currency": "USD",
   "af_cost_value": 6,
   "af_dp": "afbasicapp://mainactivity",
   "af_ios_url": "https://isitchristmas.com/",
   "af_sub1": "my_sub1",
   "af_sub2": "my_sub2",
   "c": "fruit_of_the_month",
   "campaign": "fruit_of_the_month",
   "fruit_amount": 26,
   "fruit_name": "apples",
   "deep_link_sub1": 26,
   "deep_link_value": "apples",
   "is_retargeting": true,
   "link": "https://onelink-basic-app.onelink.me/H5hv/6d66214a",
   "media_source": "Email",
   "pid": "Email"
}
```
```json Long link
{
   "path": "/H5hv",
   "af_android_url": "https://my_android_lp.com",
   "af_channel": "my_channel",
   "host": "onelink-basic-app.onelink.me",
   "af_adset": "my_adset",
   "pid": "Email",
   "scheme": "https",
   "af_dp": "afbasicapp://mainactivity",
   "af_sub1": "my_sub1",
   "fruit_name": "apples",
   "af_ad": "my_adname",
   "af_click_lookback": "20d",
   "fruit_amount": 16,
   "af_sub2": "my_sub2",
   "link": "https://onelink-basic-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&af_channel=my_channel&af_adset=my_adset&af_ad=my_adname&af_sub1=my_sub1&af_sub2=my_sub2&fruit_name=apples&fruit_amount=16&af_cost_currency=USD&af_cost_value=6&af_click_lookback=20d&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_android_url=https%3A%2F%2Fmy_android_lp.com",
   "af_cost_currency": "USD",
   "c": "fruit_of_the_month",
   "af_ios_url": "https://my_ios_lp.com",
   "af_cost_value": 6
}
```

## URI scheme

Input to `onAppOpenAttribution(_ attributionData: [AnyHashable: Any])`

```json Short link
{
	"af_click_lookback ": "25d",
	"af_sub1 ": "my_sub1",
	"shortlink ": "9270d092",
	"af_deeplink ": true,
	"media_source ": "Email",
	"campaign ": "my_campaign",
	"af_cost_currency ": "NZD",
	"host ": "mainactivity",
	"af_ios_url ": "https://my_ios_lp.com",
	"scheme ": "afbasicapp",
	"path ": "",
	"af_cost_value ": 5,
	"af_adset ": "my_adset",
	"af_ad ": "my_adname",
	"af_android_url ": "https://my_android_lp.com",
	"af_sub2 ": "my_sub2",
	"af_force_deeplink ": true,
	"fruit_amount ": 15,
	"af_dp ": "afbasicapp://mainactivity",
	"link ": "afbasicapp://mainactivity?af_ad=my_adname&af_adset=my_adset&af_android_url=https%3A%2F%2Fmy_android_lp.com&af_channel=my_channel&af_click_lookback=25d&af_cost_currency=NZD&af_cost_value=5&af_deeplink=true&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_force_deeplink=true&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_sub1=my_sub1&af_sub2=my_sub2&af_web_id=56441f02-377b-47c6-9648-7a7f88268130-o&campaign=my_campaign&fruit_amount=15&fruit_name=apples&is_retargeting=true&media_source=Email&shortlink=9270d092",
	"af_channel ": "my_channel",
	"is_retargeting ": true,
	"af_web_id ": "56441f02-377b-47c6-9648-7a7f88268130-o",
	"fruit_name ": "apples"
  "deep_link_sub1": 26,
  "deep_link_value": "apples",
}
```
```json Long link
{
	"af_ad ": "my_adname",
	"fruit_name ": "apples",
	"host ": "mainactivity",
	"af_channel ": "my_channel",
	"link ": "afbasicapp://mainactivity?af_ad=my_adname&af_adset=my_adset&af_android_url=https%3A%2F%2Fmy_android_lp.com&af_channel=my_channel&af_click_lookback=25d&af_cost_currency=NZD&af_cost_value=5&af_deeplink=true&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_force_deeplink=true&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_sub1=my_sub1&af_sub2=my_sub2&af_web_id=56441f02-377b-47c6-9648-7a7f88268130-o&campaign=my_campaign&fruit_amount=15&fruit_name=apples&is_retargeting=true&media_source=Email",
	"af_deeplink ": true,
	"campaign ": "my_campaign",
	"af_sub1 ": "my_sub1",
	"af_click_lookback ": "25d",
	"af_web_id ": "56441f02-377b-47c6-9648-7a7f88268130-o",
	"path ": "",
	"af_sub2 ": "my_sub2",
	"af_ios_url ": "https://my_ios_lp.com",
	"af_cost_value ": 5,
	"fruit_amount ": 15,
	"is_retargeting ": true,
	"scheme ": "afbasicapp",
	"af_force_deeplink ": true,
	"af_adset ": "my_adset",
	"media_source ": "Email",
	"af_cost_currency ": "NZD",
	"af_dp ": "afbasicapp://mainactivity",
	"af_android_url ": "https://my_android_lp.com"
}
```

## Deferred deep linking

Input to `onConversionDataSuccess(_ data: [AnyHashable: Any])`

```json Short link
{
	"adgroup": null,
	"adgroup_id": null,
	"adset": null,
	"adset_id": null,
	"af_ad": "my_adname",
	"af_adset": "my_adset",
	"af_android_url": "https://isitchristmas.com/",
	"af_channel": "my_channel",
	"af_click_lookback": "20d",
	"af_cost_currency": "USD",
	"af_cost_value": 6,
	"af_cpi": null,
	"af_dp": "afbasicapp://mainactivity",
	"af_ios_url": "https://isitchristmas.com/",
	"af_siteid": null,
	"af_status": "Non-organic",
	"af_sub1": "my_sub1",
	"af_sub2": "my_sub2",
	"af_sub3": null,
	"af_sub4": null,
	"af_sub5": null,
	"agency": null,
	"campaign": "fruit_of_the_month ",
	"campaign_id": null,
	"click_time": "2020-08-12 15:08:00.770",
	"cost_cents_USD": 600,
	"engmnt_source": null,
	"esp_name": null,
	"fruit_amount": 26,
	"fruit_name": "apples",
  "deep_link_sub1": 26,
  "deep_link_value": "apples",  
	"http_referrer": null,
	"install_time": "2020-08-12 15:08:33.335",
	"is_branded_link": null,
	"is_first_launch": 1,
	"is_retargeting": true,
	"is_universal_link": null,
	"iscache": 1,
	"match_type": "probabilistic",
	"media_source": "Email",
	"orig_cost": "6.0",
	"redirect_response_data": null,
	"retargeting_conversion_type": "none",
	"shortlink": "6d66214a"
}
```

<Accordion title="Accordion one" icon="fa-info-circle">
  Lorem ipsum
</Accordion>

<Accordion title="Accordion two" icon="fa-info-circle">
  Lorem ipsum.<br />

  ```text
  some code or other
  ```
</Accordion>

<Accordion title="Some title" icon="fa-info-circle">
  wdythiing is going on here?
</Accordion>

<Accordion title="" icon="fa-info-circle">

</Accordion>

<Accordion />

This is a title
