# ePost eArchive

3 operations of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### GET /epost/v2/archives/directories

Get all folders (branded folders, unbranded folders and subfolders) in the Storage

- operationId: `listArchiveFolders`

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Retrieve folders successfully. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### GET /epost/v2/archives/letters

Get letters in root storage or in a specific folder.

- operationId: `listArchiveLetters`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `directory-id` | query | no | string | The ID of the folder to get for letters. If empty, it will get letters in the root storage. |
| `limit` | query | no | integer (int32) | Define the limit of the letter list that this API returns. For example, if a user have 20 letters in their inbox, and this limit parameter is 5, and the offset parameter is 0, then this API will return letter list containing the first 5 letters If not set, then default value of 48 is used |
| `offset` | query | no | integer (int32) | Define which position to start getting letter list. For example, if a user have 20 letters in their inbox, and this offset parameter is 5, then this API will return letter list from the 5th letter. If not set, then default value of 0 is used |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Retrieve letters successfully. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### POST /epost/v2/archives/upload

Uploads a document to the ePost archive of the authenticated tenant together with its metadata.

Uploads a document to the ePost archive of the authenticated tenant together with its metadata.The request contains the binary file and document metadata. After a successful upload, the document is stored in the tenant's ePost archive and can be accessed through the archive.

- operationId: `uploadArchiveDocument`

**Request body** (`multipart/form-data`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `file` | object | no |  |
| `metadata` | object | no | metadata |
| `directoryIds` | array | no | The IDs of the archive directories in which the uploaded document should be stored. All provided directory IDs must belong to the authenticated tenant. If no directory ID is provided, the document is stored without a directory assignment. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `201` | ArchiveUploadResponse | Document uploaded successfully. |
| `400` | ErrorMessage | Data invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | The current user is not allowed to access this company data Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `411` | ErrorMessage | Length required — the request did not advertise a numeric, non-negative `Content-Length`. Chunked transfer encoding is not supported on this endpoint. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `413` | ErrorMessage | Request entity too large — payload exceeds the 1GB ceiling. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `415` | ErrorMessage | Unsupported Media Type Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
