# ePost Sign

8 operations of the ePost API, generated from `epost-openapi.json`. Canonical page: https://developer.klara.ch/epost-preview/#reference

### GET /epost/signing/billing-companies

List companies the authenticated user may bill a signing to.

Returns the companies the authenticated user is allowed to charge a signing request to — i.e. where the user holds the `DOCUMENT_SIGN_BILLING` company role in the KLARA web client role system. Used by the UI to render the selection. A user can hold the role in several companies. The returned `companyTenantId` values are valid inputs for `billToCompanyTenantId` on `POST /signing/processes`. luz-documentSign only checks the role and holds no own mapping; revoking the role later does not affect already-submitted processes.

- operationId: `listSigningBillingCompanies`

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/json | Companies the user may bill to. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | ePost Sign widget not subscribed. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### GET /epost/signing/processes

List signing processes of the calling tenant.

Returns a lightweight page of signing processes belonging to the calling tenant: creator, title, optional description, status, deadline and creation timestamp. Use `GET /signing/processes/{processId}` for the full view. Access requires the ePost Sign widget subscription.

- operationId: `listSigningProcesses`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `limit` | query | no | integer | Maximum number of items per page. Defaults to 25, capped at 200. |
| `offset` | query | no | integer | Number of items to skip from the start of the result set. Defaults to 0. Page (zero-based) `n` is `offset = n * limit`; the total item count is returned as `total` in the response. |
| `status` | query | no | string | Optional filter on lifecycle status. Allowed values: `CREATED`, `IN_PROGRESS`, `COMPLETED`, `EXPIRED`, `CANCELLED`. If omitted, all processes are returned. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | SigningProcessListResponse | A page of signing processes. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | ePost Sign widget not subscribed. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### POST /epost/signing/processes

Submit a document for signing and create a signing process.

Submits exactly ONE document to be signed and creates a signing process. Uses `multipart/form-data` analogous to `POST /epost/v2/deliveries`: a `file` part (the PDF) and a `metadata` JSON part (`SigningProcessCreateRequest`). The document is stored, a slot per participant is created, and the signature document is made available to each participant in the signing room. Participants without an ePost account receive a forced-onboarding email when `forceOnboarding = true` AND the credential is an email address (onboarded users can complete only `SES`). Returns the created process id and its initial status; full details via `GET /signing/processes/{processId}`. Access requires the ePost Sign widget subscription. Billing a signing to a company requires the `DOCUMENT_SIGN_BILLING` company role in the KLARA web client role system on the paying company.

- operationId: `createSigningProcess`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `Idempotency-Key` | header | no | string | Client-generated key that prevents duplicate process creation on retries. Optional: if omitted, retries may create a duplicate process. Same key + same payload returns the original result; same key + different payload → HTTP 409. |

**Request body** (`multipart/form-data`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `file` | object | no |  |
| `metadata` | object | no | The `SigningProcessCreateRequest` metadata JSON — see the field-by-field guide in the operation description. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `202` | SigningProcessCreated | Signing process accepted and created. |
| `400` | ErrorMessage | Data invalid Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | ePost Sign widget not subscribed, or caller lacks the `DOCUMENT_SIGN_BILLING` role on the requested billing company. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `411` | ErrorMessage | `Content-Length` header missing, blank, malformed or negative (chunked transfer encoding is not supported on this endpoint). Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `413` | ErrorMessage | Request body exceeds the 200 MB upload ceiling. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `422` | ErrorMessage | Validation failed on the metadata part. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### GET /epost/signing/processes/{processId}

Get a signing process — submitted data plus current signing status.

Returns the data submitted on `POST` plus the signing status: who has already signed and who has not. The name shown is the one used within the signing process / signature itself — per CISO, no additional profile data is resolved from ePost. Deliberately NOT returned: the full `unhashedCredentials` (personal-data protection) and the document payload (use `GET /signing/processes/{processId}/document`).

- operationId: `getSigningProcess`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `processId` | path | yes | string (uuid) | Server-assigned unique identifier of the signing process. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | SigningProcess | Current state of the signing process. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | ePost Sign widget not subscribed. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### PATCH /epost/signing/processes/{processId}

Update a running signing process (ADMIN-only).

ADMIN-only update of a running signing process. Callable by the process creator or by any active participant that holds the `ADMIN` role (LUZ-158618). All fields on the body are optional; only the fields the caller wishes to change need to be sent. Fields set to `null` are treated as "no change". Editable fields: `title`, `description`, `deadline` (must be strictly in the future), `documentHandling.allowPrint` / `documentHandling.allowSave`. Field-level diffing is applied and an audit trail entry (`PROCESS_UPDATED`) is written only when at least one value actually changed. Locked at creation (never patchable through this endpoint): `signatureType`, `mode`, `participants` (participants have their own PATCH endpoint), `forceOnboarding`, `billToCompanyTenantId`. Lifecycle: only `status = CANCELLED` is accepted — deactivates the process (SwissSign `signaturesCancelProcessing`), records a `PROCESS_CANCELLED` audit trail entry, schedules the process for deletion, delivers the partially-signed document to `RECEIVER` participants and notifies `SIGNER` / `ADMIN` participants. Any other status value is rejected with HTTP 400. Updates against a process in `COMPLETED`, `CANCELLED` or `EXPIRED` are rejected with HTTP 409. Callers that are not an `ADMIN` participant of this process receive HTTP 404 (the existence of the process is not disclosed). Access requires the ePost Sign widget subscription.

- operationId: `updateSigningProcess`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `processId` | path | yes | string (uuid) | Server-assigned unique identifier of the signing process. |

**Request body** (`application/json`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `title` | string | no | New title shown to participants in the signing room and in the process list. Omit to keep the current title. |
| `description` | string | no | New free-form description. Omit to keep the current description; send an empty string to clear it. |
| `deadline` | string (date) | no | New absolute deadline (ISO 8601). Must be strictly in the future. Omit to keep the current deadline. |
| `documentHandling` | object | no |  |
| `status` | object | no | Only `CANCELLED` is accepted — deactivates the process (SwissSign `signaturesCancelProcessing`). Any other value results in HTTP 400. One of `CREATED`, `IN_PROGRESS`, `COMPLETED`, `EXPIRED`, `CANCELLED`. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | SigningProcess | Updated signing process. |
| `400` | ErrorMessage | Body invalid, or `status` supplied but not equal to `CANCELLED`. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | ePost Sign widget not subscribed, or caller is not an `ADMIN` of this process. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `409` | ErrorMessage | Process is in a terminal status (`COMPLETED`, `CANCELLED` or `EXPIRED`) and cannot be updated. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `422` | ErrorMessage | Validation failed on the request body (e.g. `deadline` not strictly in the future). Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### GET /epost/signing/processes/{processId}/document

Download the final signed document.

Returns the FINAL signed PDF once the process is `COMPLETED`. While signing is ongoing the document is only viewable within the signing room and is NOT exposed here. Access is controlled per tenant.

- operationId: `getSignedDocument`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `processId` | path | yes | string (uuid) | Server-assigned unique identifier of the signing process. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | application/pdf | The final signed PDF document. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | ePost Sign widget not subscribed. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `409` | ErrorMessage | Process not yet `COMPLETED` — the signed document is not available. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### PATCH /epost/signing/processes/{processId}/participants/{participantId}

Update a participant of a running signing process (ADMIN-only).

ADMIN-only update of a single participant. Callable by the process creator or by any active participant that holds the `ADMIN` role (LUZ-158618). Only reachable while the participant is `PENDING` or `FORCED_ONBOARDING` — once the participant has `SIGNED`, `REJECTED`, `EXPIRED` or has already been `REMOVED`, the request is rejected with HTTP 409. All fields on the body are optional; field-level diffing is applied and an audit trail entry (`PARTICIPANT_UPDATED`) is written only when at least one value actually changed. Removal: send `status = REMOVED` to remove the participant from the process. Records a `PARTICIPANT_REMOVED` audit trail entry. Any other status value is rejected with HTTP 400. Credential-change re-matching (LUZ-158618): changing any credential field — `hashedPrimaryCredential`, `hashedPrimaryValue`, `derivedHashedCredentials`, `derivedHashedValue`, `unhashedCredentials`, or `allCredentialShouldMatch` — re-runs tenant-directory matching for this participant slot. If the resolved recipient changes, the previously-resolved tenant is remembered (so their subsequent sign attempts are rejected with `REMOVED_BY_SENDER`) and the initial signing notification is re-delivered to the newly resolved recipient. The previous recipient's letterbox entry is intentionally NOT withdrawn. If the new credential matches multiple accounts the request is rejected with HTTP 422. Last-ADMIN protection: removing the last `ADMIN` — or stripping the `ADMIN` role from the last `ADMIN` by supplying a `roles` list that does not contain `ADMIN` — is rejected with HTTP 409; the participant is left unchanged. Callers that are not an `ADMIN` participant of this process receive HTTP 404 (the existence of the process is not disclosed). Access requires the ePost Sign widget subscription.

- operationId: `updateSigningParticipant`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `participantId` | path | yes | string (uuid) | Process-local participant id (stable, also while the participant is unresolved). Matches `ParticipantStatus.internalTrackingId` from the `GET` view. |
| `processId` | path | yes | string (uuid) | Server-assigned unique identifier of the signing process. |

**Request body** (`application/json`, required)

| Field | Type | Required | Notes |
|---|---|---|---|
| `roles` | array | no | New role set. Must be non-empty when supplied. The process must retain at least one `ADMIN` and at least one `SIGNER`; removing the last `ADMIN` results in HTTP 409. |
| `hashedPrimaryCredential` | string | no | Which credential is hashed as the primary key (e.g. `email`, `mobileNumber`). Omit to keep the current value. |
| `hashedPrimaryValue` | string | no | Hash of the primary credential value. Omit to keep the current value. |
| `derivedHashedCredentials` | array | no | Names of the additional credentials mixed into the derived hash. Omit to keep the current value. |
| `derivedHashedValue` | string | no | Hash of the derived credential combination. Omit to keep the current value. |
| `unhashedCredentials` | object | no | Full participant credentials — submission only. Never returned by `GET` endpoints. Omit to keep the current value. |
| `allCredentialShouldMatch` | boolean | no | If `true`, every supplied credential MUST match to resolve the participant. Omit to keep the current value. |
| `sequenceIndex` | integer (int32) | no | 1-based order of this participant when the process runs in `SEQUENTIAL` mode. Ignored in `PARALLEL` mode. Omit to keep the current value. |
| `signatureFields` | array | no | Signature field positions for this participant. When supplied, replaces the current fields as a whole; empty list = SwissSign default placement. Omit to keep the current value. |
| `status` | object | no | Only `REMOVED` is accepted — removes the participant from the process. Any other value results in HTTP 400. Only reachable while the participant is `PENDING` or `FORCED_ONBOARDING`. One of `PENDING`, `FORCED_ONBOARDING`, `SIGNED`, `REJECTED`, `EXPIRED`, `REMOVED`, `NOT_REQUIRED`, `FULFILLED_BY_OTHER`. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | ParticipantStatus | Updated participant. |
| `400` | ErrorMessage | Body invalid, or `status` supplied but not equal to `REMOVED`. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | ePost Sign widget not subscribed, or caller is not an `ADMIN` of this process. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `409` | ErrorMessage | Participant is no longer in an editable state (not `PENDING` / `FORCED_ONBOARDING`), or the change would leave the process without any `ADMIN`. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `422` | ErrorMessage | Validation failed on the request body, or a credential change re-matches to multiple accounts. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |

### POST /epost/signing/processes/{processId}/retry-delivery

Retry delivery of a signing process.

Re-arms the outstanding delivery tasks of the signing process and triggers an immediate retry of only the not-yet-completed steps (e.g. delivering the signed document to `RECEIVER` participants or sending forced-onboarding invitations). Already-completed steps are not repeated. Returns the refreshed process view with the recomputed delivery state.

- operationId: `retrySigningDelivery`

**Parameters**

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `processId` | path | yes | string (uuid) | Server-assigned unique identifier of the signing process. |

**Responses**

| Status | Body | Meaning |
|---|---|---|
| `200` | SigningProcess | Delivery retried; refreshed process state. |
| `401` | ErrorMessage | No Authorization header found or invalid token Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `403` | ErrorMessage | ePost Sign widget not subscribed. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `404` | ErrorMessage | Resource not found Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `429` | ErrorMessage | API rate limit exceeded Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
| `500` | ErrorMessage | Something went wrong on our side while processing the request. Please kindly contact our support. Failures detected by a downstream service carry that service's own `code`, which this reference does not enumerate, so the `code` field may hold a value not documented here. Treat an unrecognised code as a generic failure of the status it arrives with, and do not branch on a code this reference does not list. |
