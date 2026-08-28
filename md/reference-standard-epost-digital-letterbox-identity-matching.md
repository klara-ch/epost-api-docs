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
| `400` | none declared | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `429` | none declared | API rate limit exceeded |

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
| `401` | none declared | No Authorization header found or invalid token |
| `404` | none declared | Resource not found |
| `429` | none declared | API rate limit exceeded |

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
| `401` | none declared | No Authorization header found or invalid token |
| `404` | none declared | Resource not found |
| `429` | none declared | API rate limit exceeded |

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
| `400` | none declared | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `429` | none declared | API rate limit exceeded |
