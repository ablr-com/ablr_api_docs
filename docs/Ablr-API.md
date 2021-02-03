# Ablr API

## Introduction

Welcome to our API reference! We're happy that you've decided to partner with us and explore our REST APIs. To get started, you'll first want to log into the Merchant Dashboard, set up your development environment, and get your API keys for the sandbox and live environments.


## Basics

### High level concepts

All Ablr transactions require at least 2 API calls.

1. A call to our `/checkouts` endpoint to initiate the customer flow, then a call to our `/charges` endpoint to 'capture' the funds.

2. Before the `/charges` API call can be made, the customer must complete their Ablr sign-up or login (by following the `checkout_url` returned from the `/checkouts` call)

Only if a customer's checkout is Approved, should the `/charges` call be made.

### Capture Method


Once a customer's Checkout has been Approved, you can utilise either the 'Immediate Capture' or 'Authorise and Capture' methods when completing Ablr transactions.

#### Immediate Capture

A single API call to the `/charges` endpoint is required to finalise a transaction.

The funds for that transaction will be disbursed to the merchant the next business day.


#### Authorise and Capture

First an API call including the flag `capture: false` is made to the `/charges` endpoint to perform an 'Authorisation' which will hold the funds from the customer's default payment method on Ablr platform.

After the authorisation is successful, you must send a [Capture] API request in order to capture or settle the funds **within 7 days**.



