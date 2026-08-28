# ePost Communication Platform Delivery

3 operations of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### POST /epost/v2/deliveries

Creates a delivery and send documents for a sender.

Delivery size limit: 2GBEach document size (binary + metadata) limit: 200MBTotal number of documents per delivery limit: 2,000Total number of recipients per delivery limit: 2,000

**Request body** (`multipart/form-data`, not marked required in the specification)

| Field | Type | Required | Notes |
|---|---|---|---|
| `files` | array | no |  |
| `largeFile` | object | no |  |
| `metadata` | object | no |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `201` | DeliveryResponseV2 | Delivery created |
| `400` | ErrorMessage | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `415` | none declared | Unsupported Media Type |
| `429` | none declared | API rate limit exceeded |

### GET /epost/v2/deliveries/{delivery-id}/status

Get Delivery status.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `delivery-id` | path | yes | string | The id of the delivery |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | DeliveryStatusResponseV2 | OK |
| `400` | ErrorMessage | Delivery id is invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `429` | none declared | API rate limit exceeded |

### POST /epost/v2/synchronous-deliveries

Creates a synchronous delivery to send documents for a sender and receive status immediately.

Delivery size limit: 2GBEach document size (binary + metadata) limit: 200MBTotal number of documents per delivery limit: 1500Total number of recipients per delivery limit: 1500

**Request body** (`multipart/form-data`, not marked required in the specification)

| Field | Type | Required | Notes |
|---|---|---|---|
| `files` | array | no |  |
| `metadata` | object | no |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | DeliveryStatusResponseV2 | Status of the delivery |
| `400` | ErrorMessage | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `415` | none declared | Unsupported Media Type |
| `429` | none declared | API rate limit exceeded |
