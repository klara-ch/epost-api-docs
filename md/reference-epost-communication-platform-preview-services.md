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
| `400` | ErrorMessage | Data invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `415` | ErrorMessage | Unsupported Media Type Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

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
| `400` | ErrorMessage | Preview id is invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

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
| `400` | ErrorMessage | Data invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `415` | ErrorMessage | Unsupported Media Type Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

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
| `400` | ErrorMessage | Preview id is invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Preview id is not found |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
