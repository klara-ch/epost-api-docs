# Limits and rate limiting

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#limits

## Limits and rate limiting

The API enforces a rate limit. When you exceed it, the response is `429 API rate limit exceeded`. It is declared on 28 of the 35 endpoints.

> **Warning: The limit is not published yet**
>
> The specification declares the `429` response but names no threshold, no time window, no counting scope and no `Retry-After` or rate limit headers. Until those are confirmed, retry with exponential backoff and do not assume a number.

### How to build for it

- **Retry with exponential backoff** on `429`. Do not retry in a tight loop.

- **Use the preview services** rather than sending and correcting. One preview call costs less than a delivery you did not want.

- **Batch identity matching.** The matching-run endpoints take a set and are designed for volume, unlike calling the single-shot endpoint per recipient.

> **Warning: Do not retry a delivery automatically**
>
> Backing off and retrying is right for reads. `POST /epost/v2/deliveries` is chargeable, and the API defines no idempotency key, no request id and no deduplication rule. A retry after an ambiguous response can produce a second paid delivery. Until that contract is published, treat a timeout as possibly successful and look the delivery up by your own `senderEndToEndId` before sending it again.

### Size limits

| What | Limit | Applies to |
|---|---|---|
| Delivery | 2 GB | Deliveries and both preview services |
| Single document | 200 MB | Binary and metadata counted together |
| Synchronous delivery | 1500 documents | One document per recipient, or one document to 1500 recipients |
| Identity matching metadata | 5 | Unit not stated, see above |

All size figures are decimal, not binary. One kilobyte is 103 bytes, one megabyte 106, one gigabyte 109. In exact numbers: the platform rejects anything above **2,000,000,000 bytes** per request and **200,000,000 bytes** per document.

> **Warning: Compare against the decimal value, not against 1024**
>
> A client that treats 200 MB as 209,715,200 bytes lets a 205 MB file through, and the platform then rejects it. The error appears after the upload, not before it.

### Pagination

5 endpoints accept pagination parameters:

| Endpoint | Parameters |
|---|---|
| GET `/epost/deliveries/monitoring` | `limit`, `offset` |
| GET `/epost/v2/letters` | `limit`, `offset` |
| GET `/epost/v2/letters/deleted` | `limit`, `offset` |
| GET `/epost/v2/letters/search` | `limit`, `offset` |
| GET `/epost/v2/recipients` | `limit`, `offset` |

### Searching and filtering

#### Free-text search

1 endpoint takes a search term and matches it against several fields at once. This is the only way to find a record without knowing an id.

| Endpoint | Parameter |
|---|---|
| GET `/epost/v2/letters/search` | `value` |

#### Filtering by field

9 endpoints narrow the result by a field value, 22 parameters in total. Filtering is not searching: you have to know the value you are filtering on.

| Endpoint | Parameters |
|---|---|
| GET `/epost/deliveries/monitoring` | `delivery-id` |
| POST `/epost/preview/delivery-channels` | `preview-option` |
| POST `/epost/preview/delivery-prices` | `preview-option` |
| GET `/epost/v2/letters` | `from-date`, `is-business-tenant`, `letter-folder`, `letter-types`, `read-status`, `senderCaseId`, `senderEndToEndId`, `senderParticipantId`, `senderUserId`, `to-date`, `with-reminder` |
| GET `/epost/v2/letters/deleted` | `sender-participant-id` |
| GET `/epost/v2/letters/inbox/count` | `sender-participant-id` |
| GET `/epost/v2/letters/search` | `search-location`, `sender-participant-id` |
| PATCH `/epost/v2/letters/{letter-id}/archive` | `destination-directory-id` |
| GET `/epost/v2/recipients` | `min-timestamp`, `minimum-level-of-trust`, `primary-credential` |

#### Sorting

1 endpoint accepts a sort order: `/epost/deliveries/monitoring`. Everywhere else the order is not guaranteed, which matters when you page through a large collection.
