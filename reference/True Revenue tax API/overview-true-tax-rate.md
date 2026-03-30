---
title: Overview
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
True Revenue is a business logic layer built to serve the AppsFlyer in-app purchase and subscription revenue solution. It automatically calculates the net revenue value for each incoming transaction in real time and includes it in reports. 

The net revenue amount is calculated in part based on the geo-based tax rates defined using the True Revenue tax API.

You can use the API to:

- [Create a tax rate rule](https://dev.appsflyer.com/hc/reference/create-tax-rate-rule).
- [Get a list of all your existing tax rate rules](https://dev.appsflyer.com/hc/reference/get-tax-rate-rule).

To create a tax rate rule, you need the following parameters and values as instructed by your marketer. 
[block:html]
{
  "html": "<table class=\"table--hover table--striped table--color-header unsortable\">\n  <thead>\n    <tr>\n      <th>Parameter</th>\n      <th>Mandatory</th>\n      <th>Remarks</th>\n      <th>Value (as recorded by the marketer)</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <td>tax_name</td>\n      <td>Yes</td>\n      <td>\n        <p>\n          Name appearing in the customer invoice that describes the specific\n          type of tax.&nbsp;\n        </p>\n      </td>\n      <td>\n        <p>Example: Sales, VAT, GST</p>\n      </td>\n    </tr>\n    <tr>\n      <td>tax_rate</td>\n      <td>Yes</td>\n      <td>\n        <p>\n          <span style=\"font-weight: 400;\">Number up to 4 decimal places representing the tax </span>percentage<span style=\"font-weight: 400;\"> to be collected.</span>\n        </p>\n      </td>\n      <td>\n        <p>\n          <span style=\"font-weight: 400;\">Example: 7.25</span>\n        </p>\n      </td>\n    </tr>\n    <tr>\n      <td>tax_exclusive</td>\n      <td>No</td>\n      <td>\n        <ul>\n          <li>\n            <span style=\"font-weight: 400;\">Boolean parameter, either true or false.</span>\n          </li>\n          <li>\n            <span style=\"font-weight: 400;\">False means tax is included in the overall revenue.</span>\n          </li>\n          <li>\n            <span style=\"font-weight: 400;\">True means tax is in addition to the overall stated revenue. For example, in the USA or Canada, where the sticker price doesn't include sales tax.</span>\n          </li>\n          <li>\n            <span style=\"font-weight: 400;\">Default is false.</span>\n          </li>\n        </ul>\n      </td>\n      <td>TRUE or FALSE</td>\n    </tr>\n    <tr>\n      <td>country</td>\n      <td>No</td>\n      <td>\n        <p>\n          <span style=\"font-weight: 400;\"><a href=\"https://www.nationsonline.org/oneworld/country_code_list.htm\">Two-letter ISO country code</a> for which the tax is applied.</span>\n        </p>\n      </td>\n      <td>\n        <p>\n          <span style=\"font-weight: 400;\">Example: GB</span>\n        </p>\n      </td>\n    </tr>\n    <tr>\n      <td>subdivision</td>\n      <td>No</td>\n      <td>\n        <ul>\n          <li>\n            <span style=\"font-weight: 400;\">For some countries, there can be an additional state/subdivision.</span>\n          </li>\n          <li>\n            <span style=\"font-weight: 400;\">Handled according to </span><span style=\"font-weight: 400;\"><a href=\"https://service.unece.org/trade/locode/2022-2%20SubdivisionCodes.htm\" target=\"_blank\" rel=\"noopener\">ISO 3166-2 subdivision codes</a>. </span>\n          </li>\n          <li>\n            <span style=\"font-weight: 400;\">Must include the country code and subdivision code.</span>\n          </li>\n        </ul>\n      </td>\n      <td>\n        <p>\n          <span style=\"font-weight: 400;\">Example: US-CA</span>\n        </p>\n      </td>\n    </tr>\n    <tr>\n      <td>postal_code</td>\n      <td>No</td>\n      <td>\n        <ul>\n          <li>\n            <span style=\"font-weight: 400;\">String of letters and/or numbers</span>\n          </li>\n        </ul>\n      </td>\n      <td>\n        <p>\n          <span style=\"font-weight: 400;\">Example: L4J8E3</span>\n        </p>\n      </td>\n    </tr>\n    <tr>\n      <td>deduction_order</td>\n      <td>No</td>\n      <td>\n        <ul>\n          <li>\n            <span style=\"font-weight: 400;\">Enum, either 0, 1, or 2:</span>\n            <ul>\n              <li>\n                <span style=\"font-weight: 400;\">0 means store commission is deducted first from the gross revenue and tax is deducted from the remaining amount.</span>\n              </li>\n              <li>\n                <span style=\"font-weight: 400;\">1 means tax is deducted first from the gross revenue and store commission is deducted remaining amount.</span>\n              </li>\n              <li>\n                <span style=\"font-weight: 400;\">2 means that both </span>tax\n                and store commission are deducted from the total\n                revenue.\n              </li>\n            </ul>\n          </li>\n        </ul>\n      </td>\n      <td>0, 1, or 2</td>\n    </tr>\n  </tbody>\n</table>"
}
[/block]