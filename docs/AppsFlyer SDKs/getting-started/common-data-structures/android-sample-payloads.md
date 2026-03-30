---
title: Android sample payloads
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
See the following sample payloads for App Links, URI schemes, and deferred deep linking. The samples contain a full payload, relevant for when all parameters in the Onelink custom link setup page contain data.

**Note**: Payloads return as a map. However, for clarity, the sample payloads that follow are displayed in JSON form. 

## Deferred deep linking (GCD)

Input to `onConversionDataSuccess(Map<String, Object> conversionData)`

```json Short link
{
	"redirect_response_data": null,
	"adgroup_id": null,
	"engmnt_source": null,
	"retargeting_conversion_type": "none",
	"orig_cost": 6.0,
	"af_cost_currency": "USD",
	"is_first_launch": true,
	"af_click_lookback": "20d",
	"af_cpi": null,
	"iscache": true,
	"click_time": "2020-08-12 16:04:50.605",
	"af_android_url": "https://isitchristmas.com/",
	"fruit_amount": 26,
	"is_branded_link": null,
	"match_type": "fingerprinting",
	"adset": null,
	"af_channel": "my_channel",
	"campaign_id": null,
	"shortlink": "6d66214a",
	"af_dp": "afbasicapp://mainactivity",
	"install_time": "2020-08-12 16:05:33.750",
	"af_ios_url": "https://isitchristmas.com/",
	"fruit_name": "apples",
        "deep_link_sub1": 26,
        "deep_link_value": "apples",  
	"media_source": "Email",
	"agency": null,
	"af_siteid": null,
	"af_status": "Non-organic",
	"af_sub1": "my_sub1",
	"cost_cents_USD": 600,
	"af_sub5": null,
	"af_adset": "my_adset",
	"af_sub4": null,
	"af_sub3": null,
	"af_sub2": "my_sub2",
	"adset_id": null,
	"esp_name": null,
	"af_cost_value": 6,
	"campaign": "fruit_of_the_month",
	"http_referrer": "android-app://com.slack/",
	"af_ad": "my_adname",
	"is_universal_link": null,
	"is_retargeting": true,
	"adgroup": null
}
```

## App Links

Input to `onAppOpenAttribution(Map<String, String> attributionData)`

```json Short link
{
	"af_dp": "afbasicapp://mainactivity",
	"af_ios_url": "https://isitchristmas.com/",
	"fruit_name": "apples",
        "deep_link_sub1": 26,
        "deep_link_value": "apples",  
	"c": "fruit_of_the_month",
	"media_source": "Email",
	"link": "https://onelink-basic-app.onelink.me/H5hv/6d66214a",
	"pid": "Email",
	"af_cost_currency": "USD",
	"af_sub1": "my_sub1",
	"af_click_lookback": "20d",
	"af_adset": "my_adset",
	"af_android_url": "https://isitchristmas.com/",
	"af_sub2": "my_sub2",
	"fruit_amount": 26,
	"af_cost_value": 6,
	"campaign": "fruit_of_the_month",
	"af_channel": "my_channel",
	"af_ad": "my_adname",
	"is_retargeting": "true"
}
```
```json Long link
{
	"af_dp": "afbasicapp://mainactivity",
	"install_time": "2020-08-06 06:56:02",
	"fruit_name": "apples",
	"af_ios_url": "https://my_ios_lp.com",
	"media_source": "Email",
	"scheme": "https",
	"link": "https://onelink-basic-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&af_channel=my_channel&af_adset=my_adset&af_ad=my_adname&af_sub1=my_sub1&af_sub2=my_sub2&fruit_name=apples&fruit_amount=16&af_cost_currency=USD&af_cost_value=6&af_click_lookback=20d&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_android_url=https%3A%2F%2Fmy_android_lp.com",
	"af_cost_currency": "USD",
	"af_sub1": "my_sub1",
	"af_click_lookback": "20d",
	"path": "/H5hv",
	"af_adset": "my_adset",
	"af_android_url": "https://my_android_lp.com",
	"af_sub2": "my_sub2",
	"fruit_amount": 16,
	"af_cost_value": 6,
	"host": "onelink-basic-app.onelink.me",
	"campaign": "fruit_of_the_month",
	"af_channel": "my_channel",
	"af_ad": "my_adname"
}
```

## URI scheme

Input to `onAppOpenAttribution(Map<String, String> attributionData)`

```json Short link
{
	"scheme": "afbasicapp",
	"link": "afbasicapp://mainactivity?af_ad=my_adname&af_adset=my_adset&af_android_url=https%3A%2F%2Fmy_android_lp.com&af_channel=my_channel&af_click_lookback=25d&af_cost_currency=NZD&af_cost_value=5&af_deeplink=true&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_force_deeplink=true&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_sub1=my_sub1&af_sub2=my_sub2&af_web_id=367f81fb-59a4-446a-ac6c-a68d2ee9447c-p&campaign=my_campaign&fruit_amount=15&fruit_name=apples&is_retargeting=true&media_source=Email&shortlink=9270d092",
	"af_cost_currency": "NZD",
	"af_click_lookback": "25d",
	"af_deeplink": true,
	"path": "",
	"af_android_url": "https://my_android_lp.com",
	"af_force_deeplink": true,
	"fruit_amount": 15,
        "deep_link_sub1": 26,
        "deep_link_value": "apples",  
	"host": "mainactivity",
	"af_channel": "my_channel",
	"shortlink": "9270d092",
	"af_dp": "afbasicapp://mainactivity",
	"install_time": "2020-08-06 06:56:02",
	"af_ios_url": "https://my_ios_lp.com",
	"fruit_name": "apples",
	"af_web_id": "367f81fb-59a4-446a-ac6c-a68d2ee9447c-p",
	"media_source": "Email",
	"af_status": "Non-organic",
	"af_sub1": "my_sub1",
	"af_adset": "my_adset",
	"af_sub2": "my_sub2",
	"af_cost_value": 5,
	"campaign": "my_campaign",
	"af_ad": "my_adname",
	"is_retargeting": true
}
```
```json Long link
{
	"af_dp": "afbasicapp://mainactivity",
	"install_time": "2020-08-06 06:56:02",
	"af_ios_url": "https://my_ios_lp.com",
	"fruit_name": "apples",
	"af_web_id": "367f81fb-59a4-446a-ac6c-a68d2ee9447c-p",
	"scheme": "afbasicapp",
	"media_source": "Email",
	"link": "afbasicapp://mainactivity?af_ad=my_adname&af_adset=my_adset&af_android_url=https%3A%2F%2Fmy_android_lp.com&af_channel=my_channel&af_click_lookback=25d&af_cost_currency=NZD&af_cost_value=5&af_deeplink=true&af_dp=afbasicapp%3A%2F%2Fmainactivity&af_ios_url=https%3A%2F%2Fmy_ios_lp.com&af_sub1=my_sub1&af_sub2=my_sub2&af_web_id=367f81fb-59a4-446a-ac6c-a68d2ee9447c-p&campaign=my_campaign&fruit_amount=15&fruit_name=apples&is_retargeting=true&media_source=Email",
	"af_cost_currency": "NZD",
	"af_status": "Non-organic",
	"af_click_lookback": "25d",
	"af_sub1": "my_sub1",
	"af_deeplink": true,
	"path": "",
	"af_android_url": "https://my_android_lp.com",
	"af_adset": "my_adset",
	"fruit_amount": 15,
	"af_sub2": "my_sub2",
	"host": "mainactivity",
	"af_cost_value": 5,
	"campaign": "my_campaign",
	"af_channel": "my_channel",
	"af_ad": "my_adname",
	"is_retargeting": true
}
```