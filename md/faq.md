# FAQ

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#faq

## FAQ

Straight answers, including where the API cannot help you yet.

### Which authentication method should I use?

An API key for server-to-server integrations, generated in the ePost interface. The token flow when you act for a signed-in user. Every endpoint that requires authentication accepts either. SwissID is not supported for the API.

### How do I find out what a delivery costs before sending it?

`POST /epost/preview/delivery-prices`, then poll the status endpoint. Nothing is delivered and nothing is charged. See [Channels and prices](https://developer.klara.ch/epost-preview/#preview).

### What is the difference between the two identity matching tiers?

The standard endpoints return basic information and are included. The detailed endpoints return whether a specific credential matched, and the specification states that access is restricted and available against a fee. See [Identity matching](https://developer.klara.ch/epost-preview/#matching).

### Do I have to send personal data unhashed for matching?

No. The specification states that the metadata may be hashed before sending, and `GET /epost/v2/recipients` returns hashed metadata for lookup.

### My delivery was accepted but nothing arrived. What now?

`GET /epost/v2/deliveries/{delivery-id}/status` with the id you received on creation. Keep that id: it is the only way back to a delivery.

### Why do the authentication endpoints sit under /core/latest?

ePost and KLARA business software run on the same platform and share the authentication layer. Therefore the two token endpoints are the same for both. Everything else in this documentation is ePost only.

### Is an OpenAPI specification available?

Yes. [Download it here](https://developer.klara.ch/epost-preview/#openapi-spec). It contains exactly the endpoints documented on this page, no more and no less.
