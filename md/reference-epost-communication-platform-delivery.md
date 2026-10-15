# ePost Communication Platform Delivery

4 operations of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### POST /epost/v2/deliveries

Creates a delivery and send documents for a sender.

Delivery size limit: 2GBEach document size (binary + metadata) limit: 200MBTotal number of documents per delivery limit: 2,000Total number of recipients per delivery limit: 2,000

- operationId: `createDelivery`

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

- operationId: `getDeliveryStatus`

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

### GET /epost/v2/iletters/{iletter-id}/response-files/{response-file-id}/content

Download a file a recipient uploaded as an answer to an iLetter.

Only the sender tenant of the iLetter can read its response files. The content is available for 90 days after the answer was submitted; after that the call returns 410 with the file metadata.

- operationId: `getILetterResponseFileContent`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `iletter-id` | path | yes | string | Id of the iLetter, as sent in the letterId field of the answer webhook. |
| `response-file-id` | path | yes | string | Id of the response file, as sent in the files field of the answer webhook. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/octet-stream | The file content. Content-Type and Content-Disposition carry the original type and file name. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `410` | ResponseFileGoneResponse | The file content was removed: its 90-day retention ended, or the answer was replaced or deleted. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### POST /epost/v2/synchronous-deliveries

Creates a synchronous delivery to send documents for a sender and receive status immediately.

Delivery size limit: 2GBEach document size (binary + metadata) limit: 200MBTotal number of documents per delivery limit: 1500Total number of recipients per delivery limit: 1500

- operationId: `createSynchronousDelivery`

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
