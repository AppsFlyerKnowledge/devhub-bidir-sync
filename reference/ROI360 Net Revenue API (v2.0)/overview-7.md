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
ROI360 automatically calculates net in-app purchase (IAP) and subscription revenue. It does this by deducting store commissions and local taxes from the gross amount.

The default configuration reflects standard App Store and Google Play fees and tax rules. If your store commission differs (for example, Apple’s Small Business Program at 15 percent) or you operate in a country with special tax treatment, use the Net Revenue API to override the defaults.

ROI360 applies the following default store commissions:

- **IAP (one-time purchases):** 30 percent on both iOS and Android.
- **Subscriptions:**
  - iOS: 30 percent during the first 12 months. 15 percent thereafter.
  - Android: 15 percent from day one.

Default tax rates are listed in <a href="https://docs.google.com/document/d/e/2PACX-1vSl3DwlK2Gt2aa5gmDzD3-K3CtnIM85oMNrqx3PCTamwCERWYU48GugNpD31BFjA2PJjZnqaXIVe2Hx/pub">True Revenue - default tax rates</a>

> ⚠️ **Deprecation Notice**
> 
> API v1.0 (`/api/stores-taxes/v1.0/`) is deprecated. Please migrate to v2.0 (`/api/net-revenue/v2.0/`).