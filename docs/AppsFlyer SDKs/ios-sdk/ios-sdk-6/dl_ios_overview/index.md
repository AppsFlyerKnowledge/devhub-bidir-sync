---
title: Deep linking
hidden: false
---

## What is OneLink?

OneLink is a single link that lets your app deliver the right experience to users regardless of device, OS, channel, or platform. It powers:

- **Deep linking** – if the user already has the app installed, OneLink opens it and uses data from the click to redirect them to a specific in-app activity.
- **Deferred deep linking** – if the user doesn't have the app installed, OneLink sends them to the app store, then redirects them to a specific in-app activity once they open the app for the first time.

Setting up OneLink involves two roles: marketers plan campaigns and configure OneLink URLs; developers implement the SDK-side handling below.

> 📘 **Tip**
>
> The marketer and developers need to decide together on the best long-term system for the `deep_link_value` (and any other parameters and values) to minimize additional app updates.
>
> The `deep_link_value` can be based on a SKU, post ID, path, or anything else. We strongly recommend agreeing on a system that lets you enter dynamic values on your chosen parameter, so you can generate many different deep links to different in-app content without further changes to the app code.
>
> In the following examples, the marketer and developer chose a fruit type as the `deep_link_value`, and the developers made the value dynamic so the marketer can enter any fruit without more work from the dev team:
>
> `https://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&deep_link_value=apples`
> `https://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&deep_link_value=bananas`
> `https://onelink-sample-app.onelink.me/H5hv?pid=Email&c=fruit_of_the_month&deep_link_value=peaches`

## iOS setup

1. [Initial app setup](dl_ios_init_setup)
2. [Unified deep linking (UDL)](dl_ios_unified_deep_linking)
3. [Optional] [Extended deferred deep linking](dl_ios_ocds_ddl) using parts of the GCD API.
4. [Optional] [Deferred deep linking with iOS Private Relay](dl_ios_private_relay), for users who enable Private Relay on iOS 15 and later.

**Note:** For new users, UDL only returns the parameters relevant to deferred deep linking: `deep_link_value` and `deep_link_sub1-10`. Other parameters, such as `media_source`, `campaign`, and `af_sub1-5`, return `null`.

**Note:** UDL isn't activated for deferred deep linking when a user clicks a link from a self-reporting network (SRN) like Meta ads, clicks a link that doesn't contain `deep_link_value` or `deep_link_sub1-10`, or installs more than 15 minutes (the UDL lookback window) after the click. Implement extended deferred deep linking to cover these cases. If you implement both UDL and extended deferred deep linking, make sure only one of them handles deferred deep linking.

**Note:** Customers already using OneLink may be using the [legacy methods](dl_ios_gcd_legacy) for deep linking and deferred deep linking, instead of UDL.

## User invites

Let users refer others to the app using OneLink links. See [Creating user invites for iOS](dl_ios_user_invite).
