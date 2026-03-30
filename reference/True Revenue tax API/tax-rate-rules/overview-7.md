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
True Revenue is a business logic layer built to serve the AppsFlyer in-app purchase and subscription revenue solution. It automatically calculates the net revenue value for each incoming transaction in real time and includes it in reports. True Revenue considers the following factors in the gross-to-net revenue calculation: 

- Store commission:
   - Is calculated and reported automatically. No action on your part is required.
   - For subscriptions are automatically calculated on a per-subscriber basis, taking into account the lifetime of the subscriber, starting at 30% commission, and reducing to 15% after 1 year.
   - Related net revenue parameters are available in raw data reports.

- Tax: Reported after you set up the dedicated tax API which sets the tax rates based on geo to be calculated on incoming transactions.

**Prerequisites**
Get from the marketer:
- The API V2 token to use as the authorization key.
- The parameters and values that contain information on what taxes to calculate.