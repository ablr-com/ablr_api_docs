# Integration Overview

## Introduction

Welcome to our API reference! We're happy that you've decided to partner with us and explore our APIs. Use the Demo site https://demo.ablr.com to experience our checkout flow. You'd find our [Test Credentials](https://docs.ablr.com/docs/ablr/docs/Test-Credentials.md) handy.

## Steps for a Direct integration

This type of integration will generate a hosted-checkout experience from a server-side API request. You will control the communication flow.

1. Contact our Merchant Integration Engineer or email [Developer Support](mailto:developer@ablr.com) to receive your secret API key for the staging environment, and access to our [Merchant Admin Dashboard (Staging)](http://merchant-uat.ablr.com/).
2. Initate checkout flow by [creating a checkout object](https://docs.ablr.com/docs/ablr/reference/Merchant.v1.yaml/paths/~1checkouts~1/post).
3. Redirect your customers to the `checkout_url` returned by the `/checkouts/` call so they can commence the checkout process.
4. As soon as the customer confirms to pay for an Ablr transaction, *by default* Ablr would capture the fund from the customer' preferred payment method immediately before redirecting the customer back to your Merchant's `success_url`.
You could opt out this behavior in order to [capture a confirmed order](https://docs.ablr.com/docs/ablr/reference/Merchant.v1.yaml/paths/~1charges~1/post) yourself on the redirect back to your Merchant's `confirm_url`. For example, you might need to verify whether or not the customer's order can be fulfilled at that point (e.g. product is still in stock, and price remains unchanged). Contact our Merchant Integration Engineer or email [Developer Support](mailto:developer@ablr.com) to opt out.

You could also sign in our [Merchant Admin Dashboard (Staging)](http://merchant-uat.ablr.com/m/settings/) to configure the following redirect URLs
- Redirect URLs:
  - `success_url` (*optional*): The URL to which the consumer is redirected after completing a successful order
  - `confirm_url` (**required** if "Merchant to Capture" is ON): The URL to which the consumer is redirected after confirming an order (pending a charge from the merchant)
  - `cancel_url` (*optional*): The URL to which the consumer is redirected after cancelling an order.