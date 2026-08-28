# ePost Communication Platform Monitoring

1 operation of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### GET /epost/deliveries/monitoring

Get monitoring data for the deliveries.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `delivery-id` | query | no | integer (int64) | Filter recipient tracking base on the delivery id. If this parameter is empty, the API will return the records for all deliveries. |
| `limit` | query | no | integer (int32) | Define the limit of the recipient tracking list that this API returns. For example, if a sender have 10 recipient tracking in the database, and this limit parameter is 5, and the offset parameter is 0, then this API will return the list containing the first 5 records If not set, then default value of 48 is used |
| `offset` | query | no | integer (int32) | Define which position to start getting recipient tracking list. For example, if a user have 10 recipient tracking in the database, and this offset parameter is 5, then this API will return recipient tracking list from the 5th record. If not set, then default value of 0 is used |
| `sort` | query | no | object | Sorting the list for getting newest (DESC) or oldest (ASC) results. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | List of recipient tracking was returned |
| `400` | ErrorMessage | Data invalid |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |
