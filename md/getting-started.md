# Getting started

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#getting-started

## Getting started

Five steps from nothing to a delivery you can verify. Where this page and the [specification](https://developer.klara.ch/epost-preview/#openapi-spec) disagree, the specification is authoritative.

### 1. Get credentials

You need an ePost business account. The API accepts either an API key that you generate in the ePost interface, or a token you obtain with your ePost username and password. See [Authentication](https://developer.klara.ch/epost-preview/#authentication).

### 2. Register on the test environment

You can onboard yourself as a sender on the test environment without asking anyone. It takes about ten minutes and it is the only safe way to try a delivery. See [Getting access](https://developer.klara.ch/epost-preview/#access).

### 3. Ask before you send

Post to `/epost/preview/delivery-channels` to learn which channels a recipient can be reached on, or `/epost/preview/delivery-prices` for what it would cost. Neither delivers anything. See [Channels and prices](https://developer.klara.ch/epost-preview/#preview).

### 4. Send your first delivery

> **Note: This calls the test environment**
>
> Nothing is charged and nothing is physically posted. Switch `EPOST_HOST` to `https://api.epost.ch` only when you are ready to deliver for real.

```
export EPOST_HOST=https://api-test.klara-epost.tech

curl -X POST "$EPOST_HOST/epost/v2/deliveries" \
  -H "X-API-KEY: $EPOST_API_KEY" \
  -F "metadata=@delivery.json;type=application/json" \
  -F "files=@letter.pdf;type=application/pdf"
```

The request is `multipart/form-data`, not JSON. The file part is called `files` and the metadata part `metadata`; those two names come from the specification and nothing else works. Both files are ready to download:

[delivery.json](examples/delivery.json) [letter.pdf](examples/letter.pdf)

Open `delivery.json` and replace the two placeholders marked `REPLACE_ME` with a recipient you control. On the test environment, use the account you registered in step 2.

The answer is `201` with the delivery id in the body:

```
{
  "deliveryId": "8f1c0c22-4c6d-4b1a-9a3e-2b4d7f0e51aa"
}
```

Keep that id. It is the only way to ask what happened. See [Sending documents](https://developer.klara.ch/epost-preview/#delivery).

### 5. Check what happened

```
curl -X GET "$EPOST_HOST/epost/v2/deliveries/{delivery-id}/status" \
  -H "X-API-KEY: $EPOST_API_KEY"
```

Immediately after sending you get `PROCESSING`. A finished delivery looks like this:

```
{
  "deliveryId": "8f1c0c22-4c6d-4b1a-9a3e-2b4d7f0e51aa",
  "status": "DELIVERED",
  "documents": [
    { "fileName": "letter.pdf", "channel": "DIGITAL", "status": "DELIVERED" }
  ]
}
```

Treat `DELIVERED`, `DELIVERED_WITH_ERROR` and `ERROR` as final and stop polling. The complete state machine is not published yet, see [Known limitations](https://developer.klara.ch/epost-preview/#roadmap).
