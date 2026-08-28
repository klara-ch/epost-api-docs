# ePost Digital Letterbox

13 operations of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### GET /epost/v2/letters

Get letters of current user.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `from-date` | query | no | string | Filter letters with the send date (yyyy-MM-dd). Letters were sent since 'from-date' value will be return. Both from-date and to-date need to be specified to filter letters within a time period. Example: 2021-10-01 |
| `is-business-tenant` | query | no | boolean |  |
| `letter-folder` | query | no | object | Filter letters in specific folder Acceptable values: INBOX_FOLDER for inbox letters SENT_FOLDER for outbox letters INBOX_FOLDER is chosen by default |
| `letter-types` | query | yes | array | Filter letters with this media type Acceptable values: CLASSIC_LETTER for classic letters SMART_LETTER for smart letters SMART_LETTER_ANSWER for smart letter answers SIMPLE_SHORT_MESSAGE for simple short message INCAMAIL for IncaMail SECURESEND for SecureSend CLASSIC_LETTER is chosen by default |
| `limit` | query | no | integer (int32) | Define the limit of the letter list that this API returns. For example, if a user have 20 letters in their inbox, and this limit parameter is 5, and the offset parameter is 0, then this API will return letter list containing the first 5 letters If not set, then default value of 48 is used |
| `offset` | query | no | integer (int32) | Define which position to start getting letter list. For example, if a user have 20 letters in their inbox, and this offset parameter is 5, then this API will return letter list from the 5th letter. If not set, then default value of 0 is used |
| `read-status` | query | no | object | Filter letters with read status Acceptable values: READ for read letters UNREAD for unread letters ALL for all letters ALL is chosen by default |
| `senderCaseId` | query | no | string | Filter letters with sender case id |
| `senderEndToEndId` | query | no | string | Filter letters with sender end to end id |
| `senderParticipantId` | query | no | string | Filter letters with sender participant id |
| `senderUserId` | query | no | string | Filter letters with sender user id |
| `to-date` | query | no | string | Filter letters with the send date (yyyy-MM-dd). Letters were sent before 'to-date' value will be return Both from-date and to-date need to be specified to filter letters within a time period. Example: 2021-10-01 |
| `with-reminder` | query | no | boolean |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Retrieve letters successfully. |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### GET /epost/v2/letters/deleted

Get deleted letters and its remaining days to be permanently deleted.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `limit` | query | no | integer (int32) | Define the limit of the letter list that this API returns. For example, if a user have 20 letters in their inbox, and this limit parameter is 5, and the offset parameter is 0, then this API will return letter list containing the first 5 letters If not set, then default value of 48 is used |
| `offset` | query | no | integer (int32) | Define which position to start getting letter list. For example, if a user have 20 letters in their inbox, and this offset parameter is 5, then this API will return letter list from the 5th letter. If not set, then default value of 0 is used |
| `sender-participant-id` | query | no | string | Filter count letters from a specific sender by their participant id. Example: aaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Retrieve deleted letters successfully. |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### GET /epost/v2/letters/inbox/count

Get the inbox unread letters count of the current user.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `sender-participant-id` | query | no | string | Filter count letters from a specific sender by their participant id. Example: aaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Retrieve letters count successfully. |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### POST /epost/v2/letters/read

Update the READ/UNREAD status for selected letters. The default status is READ.

**Request body** (`application/json`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `readStatus` | object | yes |  |
| `letterIds` | array | yes | Letter ids |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Successful operation |
| `207` | application/json | Partial success |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### GET /epost/v2/letters/search

Search for letters using keywords that can be found in the letter's title, sender's name, or content.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `limit` | query | no | integer (int32) | Define the limit of the letter list that this API returns. For example, if a user have 20 letters in their inbox, and this limit parameter is 5, and the offset parameter is 0, then this API will return letter list containing the first 5 letters If not set, then default value of 48 is used |
| `offset` | query | no | integer (int32) | Define which position to start getting letter list. For example, if a user have 20 letters in their inbox, and this offset parameter is 5, then this API will return letter list from the 5th letter. If not set, then default value of 0 is used |
| `search-location` | query | no | string | Search letters with location Acceptable values: ALL for all location, is chosen by default INBOX for inbox folder STORAGE for letters in storage TRASH for deleted letters in trash folder |
| `sender-participant-id` | query | no | string | Sender participant id of letters (Optional), This unique ID can be used to reference the sender |
| `value` | query | no | string | Key words to search letters, it can be sender name, letter title, text content, etc. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Retrieve letters successfully. |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### DELETE /epost/v2/letters/{letter-id}

Delete letter

Delete letter by letter id

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `letter-id` | path | yes | string | Id of the letter needed to be deleted |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `204` | none declared | Successful operation |
| `400` | ErrorMessage | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `404` | ErrorMessage | Resource not found |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### GET /epost/v2/letters/{letter-id}

Get a letter of current user with given id.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `letter-id` | path | yes | string | The id of the letter |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | Letter | Retrieve letter successfully. |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### POST /epost/v2/letters/{letter-id}/accept

Accept a letter by id.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `letter-id` | path | yes | string | Id of the letter to accept |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `204` | none declared | Successful operation |
| `401` | none declared | No Authorization header found or invalid token |
| `404` | ErrorMessage | Resource not found |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### PATCH /epost/v2/letters/{letter-id}/archive

Archive a letter from Letterbox to the root or to a specific folder in Storage.

If the letter is already archived and in the Storage, the exception will be thrown.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `letter-id` | path | yes | string | The ID of the letter to archive |
| `destination-directory-id` | query | no | string | Destination folder id. If empty, the letter will be stored in the root storage. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `204` | none declared | Store letter successfully. |
| `401` | none declared | No Authorization header found or invalid token |
| `404` | ErrorMessage | Resource not found |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### GET /epost/v2/letters/{letter-id}/content

Get the content of a letter with given id.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `letter-id` | path | yes | string | The id of the letter |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/octet-stream, application/json | OK |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### POST /epost/v2/letters/{letter-id}/reject

Reject a letter by id.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `letter-id` | path | yes | string | Id of the letter to reject |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `204` | none declared | Successful operation |
| `401` | none declared | No Authorization header found or invalid token |
| `404` | ErrorMessage | Resource not found |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### POST /epost/v2/letters/{letter-id}/restore

Restore a letter being deleted from inbox by letter id.

Recover letter by letter id

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `letter-id` | path | yes | string | Id of the letter needed to be restored |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `204` | none declared | Successful operation |
| `400` | ErrorMessage | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `404` | ErrorMessage | Resource not found |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |

### GET /epost/v2/letters/{letter-id}/thumbnail

Get thumbnail of a letter with given id.

Thumbnail is returned as an stream of bytes with JPEG image format. Default image size is 90x128 pixels.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `letter-id` | path | yes | string | Id of the letter to get the thumbnail for. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/octet-stream | Retrieve the letter thumbnail successfully. |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |
