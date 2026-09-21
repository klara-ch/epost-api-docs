# Standard ePost Digital Letterbox Identity Matching

4 operations of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### POST /epost/v2/standard-matching-runs

Standard identity matching process.

Send metadata of your customers for matching them with ePost users. There is the possibility to hash the metadata before sending it. It isn't mandatory to send all data for identity matching, but the sent data has to be unique in it's combination.

**Request body** (`application/json`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `recipients` | array | yes |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `202` | none declared | Request accepted |
| `400` | ErrorMessage | Data invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### GET /epost/v2/standard-matching-runs/processing/{matching-run-id}

Checking matching process.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `matching-run-id` | path | yes | string |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | StandardMatchingRunProcessResponse | Matching process is processing. |
| `303` | none declared | Finished matching process. Then auto redirect to get matching result api /matching-runs/{matching-run-id} and return matching result. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### GET /epost/v2/standard-matching-runs/{matching-run-id}

Get all of matching results from matching run id.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `matching-run-id` | path | yes | string |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | StandardMatchingRunProcessResponse | Retrieve successfully. The matching status could be processing or finished. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### POST /epost/v2/standard-matchings

Identity matching process. Standard version just return the user's basic information.

Send metadata of your customers for matching them with ePost users. The limit size of the metadata is 5 There is the possibility to hash the metadata before sending it. It isn't mandatory to send all data for identity matching, but the sent data has to be unique in it's combination.

**Request body** (`application/json`, not marked required in the specification)

| Field | Type | Required | Notes |
|---|---|---|---|
| `recipients` | array | yes |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Get matched users |
| `400` | ErrorMessage | Data invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
