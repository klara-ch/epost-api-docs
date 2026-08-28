# ePost Digital Letterbox Onboarding and Profile Management

1 operation of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### POST /epost/onboarding

Improves users's onboarding experience and add credentials

Generate a link or QR code on which interacted/scanned, the actions defined in the request body will be executed. The response output is decided by the Accept header of the request If a value of "text/html" is entered in the Accept header, a response with "text/html" media type is returned containing the URL. If a value of "image/png" is entered in the Accept header, a response with "image/png" media type is returned containing the QR code. Verification score of the credentials Accept value 0 or 50. Only one credential has verification score in the request body.

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `encrypt-token` | query | no | boolean | Indicate that if the encryption is required for the token part of the response URL. Should be true when the token payload contains recipient's credentials. |

**Request body** (`application/json`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `pinBrandedFolder` | boolean | no |  |
| `addingCredential` | object | no |  |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `201` | image/png, application/json, text/html;charset=UTF-8, application/json | QR code or Link generated successfully. |
| `401` | none declared | No Authorization header found or invalid token |
| `403` | ErrorMessage | The current user is not allowed to access this company data |
| `404` | ErrorMessage | Resource not found |
| `429` | none declared | API rate limit exceeded |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. |
