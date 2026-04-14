---
title: FAQ
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
[block:html]
{
  "html": "<table class=\"table--hover table--striped table--color-header unsortable\">\n  <tbody>\n    <tr>\n      <td>\n        <p>\n          <strong>What if I can't send a device identifier?</strong>\n        </p>\n        <p>\n          You may be unable to send an identifier for a reason out of your\n          control. For example, because the user has limited ad tracking\n          (LAT) or uses iOS 14 and did not give ATT consent.\n        </p>\n        <p>\n          Not sending an advertising ID/device identifier can result in:\n        </p>\n        <ul>\n          <li>\n            <strong>Postback issues</strong>: The media source receives\n            the postback but without a device identifier. Therefore,\n            the media source can't associate it with a user.\n          </li>\n          <li>\n            <strong>Audience segmentation and rule failure: </strong>Audience\n            rulesets require identifiers. The recommended practice is\n            to send a device ID or customer user ID for according to\n            the ID type your ruleset uses for every S2S event. However,\n            if AppsFlyer identifiers have been sent via other means (SDK\n            or a past S2S event), new in-app events are matched to the\n            correct users even if no identifiers are sent with the events.\n          </li>\n        </ul>\n      </td>\n    </tr>\n  </tbody>\n</table>"
}
[/block]