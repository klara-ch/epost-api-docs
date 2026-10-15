# iLetter

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#iletter

## iLetter

An iLetter is an interactive letter. You design it once as a Template in Studio and send it to many recipients through the normal delivery endpoints. The recipient reads it in the ePost app, fills it in and answers. While the letter is in use, the platform calls two endpoints of yours: one to fetch data for the letter, one to hand over the answer.

> **Note: iLetter replaces Smart Letter**
>
> The media type for Smart Letter is no longer listed in the specification. Use iLetter for interactive letters.

### Sending an iLetter

1. **Copy the Template ID in Studio.** Supply it on the document as `digitalAdditionalInfo.iLetter.templateId`. Every iLetter document needs one.

2. **Mark the document as an iLetter.** Set `mediaType` to `application/vnd.ch.klara.epost.v1+iletter`. Such a document carries no file binary, the content comes from the Template. The `iLetter` object is considered only on a document with this media type. On any other document it has no effect, and the delivery is neither rejected nor altered.

3. **Group deliveries if you want to.** Deliveries with the same `iLetter.batchId` add their recipients to one iLetter record in Studio. Without it, every delivery creates its own record.

4. **Supply the values the Template needs**, see below.

5. **Send** with `POST /epost/v2/deliveries` or `POST /epost/v2/synchronous-deliveries`, as for any other document.

Two optional fields shape how answers are collected: `confidentialResponses` keeps answers apart from who gave them, and `responseDeadline` tells recipients by when to answer.

### URL parameters and placeholders

A Template can declare two kinds of values. You find them in Studio in the Data view of the Template.

| Kind | Field | What it does |
|---|---|---|
| URL parameters | `iLetter.parameters` | Appended to the calls the letter makes to your data source, so your endpoint returns only the data for that recipient's case |
| Placeholders | `iLetter.placeholders` | Substituted into the text the recipient reads |

Both are lists of `{ "key": "…", "value": "…" }` entries and can be given at two levels. Under `digitalAdditionalInfo.iLetter` they are the default for every recipient of the document. Under `recipients[].iLetter` they override that default for one recipient. The rule applies per key: the recipient's value if supplied, otherwise the document's. A blank value does not fall back to the other level; leave the key out to inherit.

|  | URL parameters | Placeholders |
|---|---|---|
| Entries per level | at most 20 entries | at most 100 entries |
| Key length | at most 64 characters | at most 64 characters |
| Value length | at most 128 characters | at most 2000 characters |

Keys start with a letter or an underscore, followed by letters, digits, underscores or hyphens. A placeholder key the letter does not define is ignored.

#### When something is missing or wrong

| Situation | Outcome |
|---|---|
| A recipient has no value for a required URL parameter | The request is accepted, but the whole document ends in status `ERROR` with `ILETTER_PARAMETER_REQUIRED_VALUE_MISSING`, naming the key. No recipient of that document is delivered |
| A recipient has no value for a required placeholder | The request is accepted. That one recipient fails with `ILETTER_PLACEHOLDER_REQUIRED_VALUE_MISSING`, the others are delivered and the document ends in `DELIVERED_WITH_ERROR` |
| A URL parameter key the Template does not declare | Rejected with `ILETTER_PARAMETER_KEY_UNKNOWN` |
| Too many placeholders or a value that is too long | Rejected with `ILETTER_PLACEHOLDER_LIMIT_EXCEEDED`, naming the first offending key |

#### Example: one iLetter, three recipients

The `metadata` part of the multipart request. The Template declares the URL parameters `customerNumber` (required) and `advisorName`, and the placeholders `claimNumber` (required) and `greeting`. Recipient 2 overrides `advisorName`, the others inherit it. Recipient 3 has no `claimNumber`: the request is accepted, recipient 3 fails, recipients 1 and 2 are delivered.

```
{
  "costCenter": "Claims 2026",
  "fileMetadata": [
    {
      "mediaType": "application/vnd.ch.klara.epost.v1+iletter",
      "senderName": "Example Insurance AG",
      "senderCaseId": "CASE-2026-0815",
      "senderEndToEndId": "e2e-7f3a1c",
      "documentTitle": "Your claim - please confirm",
      "documentDescription": "Confirm the details of your claim",
      "documentTypes": ["message"],
      "deliveryChannelPreferences": ["DIGITAL"],
      "digitalAdditionalInfo": {
        "iLetter": {
          "templateId": "6ac910d20d2b211929b4280d",
          "batchId": "claims-2026-10",
          "parameters":   [ { "key": "advisorName", "value": "Anna Keller" } ],
          "placeholders": [ { "key": "greeting", "value": "Grüezi" } ]
        },
        "documentInfoByLanguage": {
          "DE": {
            "title": "Ihr Schadenfall - bitte bestätigen",
            "description": "Bestätigen Sie die Angaben zu Ihrem Schadenfall"
          }
        }
      },
      "recipients": [
        {
          "senderUserId": "K-88421",
          "unhashedCredentials": { "email": "a.muster@example.ch" },
          "iLetter": {
            "parameters":   [ { "key": "customerNumber", "value": "K-88421" } ],
            "placeholders": [ { "key": "claimNumber", "value": "CLM-2026-000471" } ]
          }
        },
        {
          "senderUserId": "K-88422",
          "unhashedCredentials": { "email": "b.muster@example.ch" },
          "iLetter": {
            "parameters": [
              { "key": "customerNumber", "value": "K-88422" },
              { "key": "advisorName", "value": "Marc Bieri" }
            ],
            "placeholders": [ { "key": "claimNumber", "value": "CLM-2026-000472" } ]
          }
        },
        {
          "senderUserId": "K-88423",
          "unhashedCredentials": { "email": "c.muster@example.ch" },
          "iLetter": {
            "parameters": [ { "key": "customerNumber", "value": "K-88423" } ]
          }
        }
      ]
    }
  ]
}
```

`documentInfoByLanguage` works for any digital document, not only iLetters: a recipient whose letterbox language has an entry sees that title and description instead of `documentTitle` and `documentDescription`.

### Checking the result

- `GET /epost/v2/deliveries/{delivery-id}/status` reports the delivery status.

- `GET /epost/deliveries/monitoring` returns, per recipient, the iLetter values as you sent them and, in `effectiveILetterParameters` and `effectiveILetterPlaceholders`, the values actually used for that recipient.

- Files a recipient uploads as an answer are fetched with `GET /epost/v2/iletters/{iletter-id}/response-files/{response-file-id}/content`.

### The two calls the platform makes to you

You configure both endpoints on the Template in Studio. They are not part of the OpenAPI specification, so this section is the reference for them.

| Call | Method | When |
|---|---|---|
| Data source | `GET` to your data-source URL | When a recipient opens the letter and it needs your data |
| Output | `POST` to your output URL | When a recipient submits an answer |

#### Headers on both calls

| Header | Value |
|---|---|
| `X-iLetter-Sender-Case-Id` | `senderCaseId` from the document |
| `X-iLetter-Sender-End-To-End-Id` | `senderEndToEndId` from the document |
| `X-iLetter-Sender-User-Id` | `senderUserId` from the recipient |
| `X-iLetter-Participant-Id` | Identifies the recipient on the platform |

Header values are percent-encoded. A value that was blank on the delivery produces no header. Authentication is what you configured for the endpoint: `Authorization: Bearer …`, an `X-API-Key` header or an `apikey=` query parameter.

#### Your data-source endpoint

Every data-source call carries the letter's URL parameters, so return only what they ask for: the recipient's own case, not the whole table. Plan for the traffic. Every recipient who opens the letter triggers a call, spread over the time recipients open their letters.

```
GET /claims/options?customerNumber=K-88422 HTTP/1.1
Host: api.example-insurance.ch
X-iLetter-Sender-Case-Id: CASE-2026-0815
X-iLetter-Sender-End-To-End-Id: e2e-7f3a1c
X-iLetter-Sender-User-Id: K-88422
X-iLetter-Participant-Id: 1a234567-1234-12a3-a1bc-12a3b4c5d678
Authorization: Bearer <the token configured on the data source>
```

#### The output call

The output call is asynchronous. The recipient's answer is accepted whether or not your endpoint responds. The platform makes at most three attempts. Each carries the same `Idempotency-Key` and the attempt number in `X-iLetter-Attempt`; de-duplicate on `Idempotency-Key`. If you configure an HMAC signature, it is sent in a header of your choice (default `X-Signature`) and covers the body, not the headers.

With the structured format the platform builds the body:

- `recipient`: the return data configured for the letter in Studio.

- `answers`: one entry per answered field.

- `meta`: `letterId`, `letterName`, `webhookCallId`, `submittedAt` and the four context values. `webhookCallId` identifies the submission and is the same for every endpoint the letter posts to. It is not the `Idempotency-Key`.

- `questions`: the question text per answered field, if enabled on the endpoint.

Return data that comes from your data source is what the recipient's session reported. The platform does not verify it, so validate it again in your system.

```
POST /claims/answers HTTP/1.1
Host: api.example-insurance.ch
Content-Type: application/json
X-iLetter-Attempt: 1
Idempotency-Key: cd6c92ea-b379-4f02-9250-05bd11b8d778
X-iLetter-Sender-Case-Id: CASE-2026-0815
X-iLetter-Sender-End-To-End-Id: e2e-7f3a1c
X-iLetter-Sender-User-Id: K-88422
X-iLetter-Participant-Id: 1a234567-1234-12a3-a1bc-12a3b4c5d678
X-Signature: <HMAC over the body>

{
  "recipient": { "customerNumber": "K-88422" },
  "answers": {
    "confirmClaim": { "value": "yes", "label": "Yes, the details are correct" }
  },
  "meta": {
    "letterId": "9f1e2d3c-4b5a-4678-9abc-def012345678",
    "letterName": "Claim confirmation",
    "webhookCallId": "8319ca98-31fa-4f54-bdd9-bf9ac3e3130e",
    "submittedAt": "2026-10-08T09:12:33Z",
    "senderCaseId": "CASE-2026-0815",
    "senderEndToEndId": "e2e-7f3a1c",
    "senderUserId": "K-88422",
    "participantId": "1a234567-1234-12a3-a1bc-12a3b4c5d678"
  },
  "questions": { "confirmClaim": "Are the claim details correct?" }
}
```
