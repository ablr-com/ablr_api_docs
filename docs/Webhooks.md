# Webhooks

Ablr uses webhooks to notify your application any time a noteworthy event happens, for instance, a successful checkout.

You could sign in our [Merchant Admin Dashboard (Staging)](http://merchant-uat.ablr.com/m/settings/) to set up a **webhook endpoint** (e.g. https://test-merchant.com/a-secret-path/​) that is hosted on your server, and to retrieve a unique webhook's **signing secret** so you could [verify](#check-your-webhook-signatures) whether a webhook event genuinely comes from us.


## About Ablr's Webhooks

- HTTPS only
- Support TLS 1.2 and above (self-signed or expired certificates are not accepted)
- Receive POST method
- To acknowledge receipt of an event, your endpoint ​**must return a 2xx HTTP status code**​ as soon possible.
- If Ablr does not receive a 2xx HTTP status code, the notification attempt will be ​**retried up to 10 times​** with exponential backoff. That means, if your endpoint doesn't acknowledge a webhook event, we'll resend it after 1 minute. If failing to notify the second time, we'll try again in 2 minutes. If failing to notify the third time, we'll try again in 4 minutes, etc.

Example of an "order.success" event payload

```
{
    "id": "stag_evt_MKsWK4hfTtyxgVEVfHKtDPa0JPkblDz7",
    "created": 1600303087,
    "type": "order.success",
    "data": {
      "order": {
        "checkout": "aMXzJPEJjY66Y4McjgSfewd18eUrjNkevAvODzG8HgLuhktknjza1lcuK3MXU8km",
        "buyer": {
          "email": "john.doe@example.com",
          "full_name": "John Doe",
          "mobile_number": "91112222"
        },
        "state": "APPROVED",
        "paid_at": "2020-09-17T08:38:07+08:00",
        "pay_today": "233.00",
        "order_code": "SG-O-HHYFYGQK4P",
        "grand_total": "699.00",
        "merchant_ref": "john.doe@example.com",
        "currency_code": "SGD",
        "monthly_payment": "233.00",
        "instalment_period": 3
      }
   }
}
```

## Check the webhook signatures

Verify the events that Ablr sends to your webhook endpoint.

Ablr will sign all webhook events that it sends to your endpoint by including a signature in each event’s ​`x-ablr-sig`​ header so you can confirm that the events were indeed sent by Ablr, not by a third party.

For example:

```
"x-ablr-sig"​: "t=1598435819,h=6d0a232d23a201b77eff3a2eb5de8268c9f33f36e58fad18d0c30f37548432ef"
```

ABLR generates signatures using a hash-based message authentication code (​[HMAC](https://en.wikipedia.org/wiki/HMAC)) with [SHA-256​](https://en.wikipedia.org/wiki/SHA-2).

### Prevent replay attacks

A [replay attack](https://en.wikipedia.org/wiki/Replay_attack) is when an attacker intercepts a valid payload and its signature, then re-transmits them. To mitigate such attacks, Ablr includes a timestamp in the ​`x-ablr-sig` header. Because this timestamp is part of the signed payload, it is also verified by the signature, so an attacker cannot change the timestamp without invalidating the signature. If the signature is valid but the timestamp is too old, you can have your application reject the payload.

We recommend that you set a default tolerance (e.g five minutes) between the timestamp and the current time. Use Network Time Protocol ([NTP](https://en.wikipedia.org/wiki/Network_Time_Protocol)) to ensure that your server’s clock is accurate and synchronizes with the time on Ablr's servers.

### Steps to verify webhook signatures

#### Step 1: Extract the timestamp and signatures from the header

Split the header, using the , character as the separator, to get a list of elements. Then split each element, using the = character as the separator, to get a prefix and value pair.
The value for the prefix `t` corresponds to the timestamp, and `h` corresponds to the signature. You can discard all other elements.

#### Step 2: Prepare the `signed_payload` string

The `signed_payload` string is created by concatenating:
- The timestamp (as a string)
- The character `.`
- The actual JSON payload (i.e., the request body)

#### Step 3: Determine the expected signature

Compute an HMAC with the SHA256 hash function. Use the endpoint’s signing secret as the key, and use the `signed_payload` string as the message.

#### Step 4: Compare the signatures

Compare the signature in the header to the expected signature. For an equality match, compute the difference between the current timestamp and the received timestamp, then decide if the difference is within your tolerance.

