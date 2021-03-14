# API Specification

## Environments

Below are a list of the endpoints for API in both staging (UAT) and production environments:

- Staging: https://api.uat.ablr.com/merchant/v1
- Production: https://api.ablr.com/merchant/v1


## Authentication

The Ablr Merchant API uses secret API keys to identify and authorize the merchants.
You would first need to obtain the secret API keys from Developer Support. It is important that you keep these keys secret - they must not be placed in public such as public repositories or client side code.
The API key must be provided in a HTTP Authorization header (`Authorization: Token {your_secret_api_key}`) for all requests, which need to be made over HTTPS (TLS 1.2 and above).
