---
title: Authentication key
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
OneLink API requests must be authenticated using a OneLink API Key. 

**Prerequisite**:

  * The account admin needs to retrieve the API key. Team members do not have access to the API Key. 

**To retrieve the OneLink API key**:

  * Go to Integration > API Access, scroll down to the OneLink API section.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b39bf80-onelink_api_key.png",
        "onelink api key.png",
        939,
        322,
        "#fcfcfd"
      ]
    }
  ]
}
[/block]
  * API calls must be made over https.
  * API rate limit: The rate limit of creating OneLink attribution links via API is 250K per day, per account.
  * Requirements: Retargeting attribution data via `onAppOpenAttribution` is available using Appslfyer SDKs, iOS and Android, version 4.8.0 and later.