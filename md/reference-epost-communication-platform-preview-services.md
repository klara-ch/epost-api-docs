# ePost Communication Platform Preview Services

4 operations of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### POST /epost/preview/delivery-channels

Creates a preview of all the available delivery channels without delivering any document.

Delivery size limit: 2GBEach document size (binary + metadata) limit: 200MB

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `preview-option` | query | no | object | Specify should include 1st or all channels in the result. Acceptable values: ALL_AVAILABLE_CHANNELS include all channels in result FIRST_AVAILABLE_CHANNEL include only first channel in result ALL_AVAILABLE_CHANNELS is chosen by default |

**Request body** (`multipart/form-data`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `files` | array | no |  |
| `largeFile` | object | no |  |
| `metadata` | object | no |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `201` | DeliveryResponseV2 | Delivery preview created |
| `400` | ErrorMessage | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `415` | none declared | Unsupported Media Type |
| `429` | none declared | API rate limit exceeded |

### GET /epost/preview/delivery-channels/{preview-id}/status

Get delivery channel preview status.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `preview-id` | path | yes | string | The id of the preview |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | PreviewDeliveryChannelResponse | OK |
| `400` | ErrorMessage | Preview id is invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `429` | none declared | API rate limit exceeded |

### POST /epost/preview/delivery-prices

Creates a preview of all the available delivery channels without delivering any document.

Delivery size limit: 2GBEach document size (binary + metadata) limit: 200MB

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `preview-option` | query | no | object | Specify should include 1st or all channels in the result. Acceptable values: ALL_AVAILABLE_CHANNELS include all channels in result FIRST_AVAILABLE_CHANNEL include only first channel in result ALL_AVAILABLE_CHANNELS is chosen by default |

**Request body** (`multipart/form-data`, not marked required in the specification)

| Field | Type | Required | Notes |
|---|---|---|---|
| `files` | array | no |  |
| `largeFile` | object | no |  |
| `metadata` | object | no |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `201` | DeliveryResponseV2 | Delivery pricing preview created |
| `400` | ErrorMessage | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `415` | none declared | Unsupported Media Type |
| `429` | none declared | API rate limit exceeded |

### GET /epost/preview/delivery-prices/{preview-id}/status

Get delivery pricing preview status.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `preview-id` | path | yes | string | The id of the preview |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | PreviewDeliveryPricingResponse | OK |
| `400` | ErrorMessage | Preview id is invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Preview id is not found |
| `429` | none declared | API rate limit exceeded |
