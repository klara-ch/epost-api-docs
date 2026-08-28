# ePost Digital LetterBox Identity Matching with detailed recipient feedback

5 operations of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### POST /epost/v2/matching-runs

Identity matching process.

Send metadata of your customers for matching them with ePost users. There is the possibility to hash the metadata before sending it. It isn't mandatory to send all data for identity matching, but the sent data has to be unique in it's combination. The access to this API request is restricted, because it returns detailed information about whether a credential has been matched or not. Against a fee you are able to use this detailed identity matching API. Please get in touch with an administrator to get more information.

**Request body** (`application/json`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `recipients` | array | yes |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `202` | MatchingRunResultLocation | Request accepted |
| `400` | none declared | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `429` | none declared | API rate limit exceeded |

### GET /epost/v2/matching-runs/processing/{matching-run-id}

Checking matching process.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `matching-run-id` | path | yes | string |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | MatchingRunProcessResponse | Matching process is processing. |
| `303` | none declared | Finished matching process. Then auto redirect to get matching result api /matching-runs/{matching-run-id} and return matching result. |
| `401` | none declared | No Authorization header found or invalid token |
| `404` | none declared | Resource not found |
| `429` | none declared | API rate limit exceeded |

### GET /epost/v2/matching-runs/{matching-run-id}

Get all of matching results from matching run id.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `matching-run-id` | path | yes | string |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | MatchingRunProcessResponse | Retrieve successfully. The matching status could be processing or finished. |
| `401` | none declared | No Authorization header found or invalid token |
| `404` | none declared | Resource not found |
| `429` | none declared | API rate limit exceeded |

### POST /epost/v2/matchings

Identity matching process.

Send metadata of your customers for matching them with ePost users. The limit size of the metadata is 5 There is the possibility to hash the metadata before sending it. It isn't mandatory to send all data for identity matching, but the sent data has to be unique in it's combination. The access to this API request is restricted, because it returns detailed information about whether a credential has been matched or not. Against a fee you are able to use this detailed identity matching API. Please get in touch with an administrator to get more information.

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

### GET /epost/v2/recipients

Get hashed metadata from ePost users for matching lookup.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `limit` | query | no | integer (int32) |  |
| `min-timestamp` | query | no | string | The minimum timestamp that sender want to filter the result. Ex: 2021-04-14T01:27:05.382Z |
| `minimum-level-of-trust` | query | yes | object | The minimum LOT that sender want to filter the result. Ex: SILVER |
| `offset` | query | no | integer (int32) |  |
| `primary-credential` | query | yes | object | The primary value that sender want to get a hashed value. Possible values: email\|mobilenumber\|postaladdress |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | MatchingLookup | Retrieve successfully. |
