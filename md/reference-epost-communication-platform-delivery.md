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
| `400` | ErrorMessage | Data invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `415` | ErrorMessage | Unsupported Media Type Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

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
| `400` | ErrorMessage | Delivery id is invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

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
| `400` | ErrorMessage | Data invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `415` | ErrorMessage | Unsupported Media Type Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
