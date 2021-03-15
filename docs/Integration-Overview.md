# Integration Overview

## Introduction

Welcome to our API reference! We're happy that you've decided to partner with us and explore our REST APIs.

## Steps for a Direct integration

This type of integration will generate a hosted-checkout experience from a server-side API request. You will control the communication flow.

1. Contact Developer Support to receive your secret API key for the staging environment.
2. Initate checkout flow by creating a checkout object.
3. Redirect your customers to the `checkout_url` returned by the `/checkouts/` call so they can commence the checkout process.
4. As soon as the customer confirms to pay for an Ablr transaction, *by default* Ablr would capture the fund from the customer' preferred payment method immediately before redirecting the customer back to your Merchant's `success_url`.
You could opt this out in order to capture the charge yourself on the redirect back to your Merchant site. For example, you might need to verify whether or not the customer's order can be fulfilled at that point (e.g. product is still in stock, and price remains unchanged). Contact Developer Support to opt out.

A webhook URL will need to be configured on your merchant integration.