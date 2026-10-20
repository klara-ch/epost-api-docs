# Changelog

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#changelog

## Changelog

What changed and when. Changes that can break an existing integration are marked as such.

### API changes

#### 20 October 2026 · 6 new · 1 removed · breaking

> **Warning: Breaking changes**
>
> Existing integrations may be affected. This concerns `POST /epost/signing/processes/{processId}/retry-delivery`.

#### New

- GET `/epost/signing/billing-allowlist` — List the calling company's billing allowlist.

- POST `/epost/signing/billing-allowlist` — Authorise an individual ePost user to bill signing requests to the calling company.

- DELETE `/epost/signing/billing-allowlist/{entry-id}` — Remove an entry from the calling company's billing allowlist.

- GET `/epost/v2/archives/directories` — documentation correction: this endpoint is not new in the API. It has been in the specification since at least 18 September 2026 and is published in this documentation for the first time. Existing integrations are not affected

- GET `/epost/v2/archives/letters` — documentation correction: this endpoint is not new in the API. It has been in the specification since at least 18 September 2026 and is published in this documentation for the first time. Existing integrations are not affected

- POST `/epost/v2/archives/upload` — documentation correction: this endpoint is not new in the API. It has been in the specification since at least 18 September 2026 and is published in this documentation for the first time. Existing integrations are not affected

#### Removed

- POST `/epost/signing/processes/{processId}/retry-delivery` — Retry delivery of a signing process.

#### 15 October 2026 · 9 new · 31 changed

#### New

- GET `/epost/signing/billing-companies` — List companies the authenticated user may bill a signing to.

- GET `/epost/signing/processes` — List signing processes of the calling tenant.

- POST `/epost/signing/processes` — Submit a document for signing and create a signing process.

- GET `/epost/signing/processes/{processId}` — Get a signing process — submitted data plus current signing status.

- PATCH `/epost/signing/processes/{processId}` — Update a running signing process (ADMIN-only).

- GET `/epost/signing/processes/{processId}/document` — Download the final signed document.

- PATCH `/epost/signing/processes/{processId}/participants/{participantId}` — Update a participant of a running signing process (ADMIN-only).

- POST `/epost/signing/processes/{processId}/retry-delivery` — Retry delivery of a signing process.

- GET `/epost/v2/iletters/{iletter-id}/response-files/{response-file-id}/content` — Download a file a recipient uploaded as an answer to an iLetter.

#### Changed

- **27 endpoints** — operationId added, `…`. A generated client used to invent a method name from the path and will now use this idShow the endpointsDELETE `/epost/v2/letters/{letter-id}`

- GET `/epost/deliveries/monitoring`

- GET `/epost/preview/delivery-channels/{preview-id}/status`

- GET `/epost/preview/delivery-prices/{preview-id}/status`

- GET `/epost/v2/deliveries/{delivery-id}/status`

- GET `/epost/v2/letters`

- GET `/epost/v2/letters/deleted`

- GET `/epost/v2/letters/inbox/count`

- GET `/epost/v2/letters/search`

- GET `/epost/v2/letters/{letter-id}`

- GET `/epost/v2/letters/{letter-id}/content`

- GET `/epost/v2/letters/{letter-id}/thumbnail`

- GET `/epost/v2/matching-runs/processing/{matching-run-id}`

- GET `/epost/v2/matching-runs/{matching-run-id}`

- GET `/epost/v2/recipients`

- GET `/epost/v2/standard-matching-runs/processing/{matching-run-id}`

- GET `/epost/v2/standard-matching-runs/{matching-run-id}`

- PATCH `/epost/v2/letters/{letter-id}/archive`

- POST `/epost/onboarding`

- POST `/epost/preview/delivery-channels`

- POST `/epost/preview/delivery-prices`

- POST `/epost/v2/deliveries`

- POST `/epost/v2/letters/read`

- POST `/epost/v2/letters/{letter-id}/accept`

- POST `/epost/v2/letters/{letter-id}/reject`

- POST `/epost/v2/letters/{letter-id}/restore`

- POST `/epost/v2/synchronous-deliveries`

POST `/epost/v2/matching-runs` — request body changed; operationId added, `createMatchingRun`. A generated client used to invent a method name from the path and will now use this id POST `/epost/v2/matchings` — request body changed; operationId added, `matchRecipients`. A generated client used to invent a method name from the path and will now use this id POST `/epost/v2/standard-matching-runs` — request body changed; operationId added, `createStandardMatchingRun`. A generated client used to invent a method name from the path and will now use this id POST `/epost/v2/standard-matchings` — request body changed; operationId added, `matchStandardRecipients`. A generated client used to invent a method name from the path and will now use this id

#### 18 September 2026 · 33 changed

#### Changed

- POST `/core/latest/tenants` — response body now documented for 429 (ErrorMessage)

- POST `/core/latest/token` — response body now documented for 400 (ErrorMessage), 401 (ErrorMessage), 429 (ErrorMessage), 500 (ErrorMessage)

- POST `/core/latest/token/by-microsoft` — response body now documented for 400 (ErrorMessage), 401 (ErrorMessage), 429 (ErrorMessage), 500 (ErrorMessage)

- **6 endpoints** — response body now documented for 401 (ErrorMessage)Show the endpointsDELETE `/epost/v2/letters/{letter-id}`

- GET `/epost/deliveries/monitoring`

- PATCH `/epost/v2/letters/{letter-id}/archive`

- POST `/epost/v2/letters/{letter-id}/accept`

- POST `/epost/v2/letters/{letter-id}/reject`

- POST `/epost/v2/letters/{letter-id}/restore`

**12 endpoints** — response body now documented for 401 (ErrorMessage), 429 (ErrorMessage)Show the endpoints

- GET `/epost/preview/delivery-channels/{preview-id}/status`

- GET `/epost/preview/delivery-prices/{preview-id}/status`

- GET `/epost/v2/deliveries/{delivery-id}/status`

- GET `/epost/v2/letters`

- GET `/epost/v2/letters/deleted`

- GET `/epost/v2/letters/inbox/count`

- GET `/epost/v2/letters/search`

- GET `/epost/v2/letters/{letter-id}`

- GET `/epost/v2/letters/{letter-id}/content`

- GET `/epost/v2/letters/{letter-id}/thumbnail`

- POST `/epost/onboarding`

- POST `/epost/v2/letters/read`

POST `/epost/preview/delivery-channels` — response body now documented for 401 (ErrorMessage), 415 (ErrorMessage), 429 (ErrorMessage) POST `/epost/preview/delivery-prices` — response body now documented for 401 (ErrorMessage), 415 (ErrorMessage), 429 (ErrorMessage) POST `/epost/v2/deliveries` — response body now documented for 401 (ErrorMessage), 415 (ErrorMessage), 429 (ErrorMessage) POST `/epost/v2/synchronous-deliveries` — response body now documented for 401 (ErrorMessage), 415 (ErrorMessage), 429 (ErrorMessage) POST `/epost/v2/matching-runs` — response body now documented for 400 (ErrorMessage), 401 (ErrorMessage), 429 (ErrorMessage) POST `/epost/v2/matchings` — response body now documented for 400 (ErrorMessage), 401 (ErrorMessage), 429 (ErrorMessage) POST `/epost/v2/standard-matching-runs` — response body now documented for 400 (ErrorMessage), 401 (ErrorMessage), 429 (ErrorMessage) POST `/epost/v2/standard-matchings` — response body now documented for 400 (ErrorMessage), 401 (ErrorMessage), 429 (ErrorMessage) GET `/epost/v2/matching-runs/{matching-run-id}` — response body now documented for 401 (ErrorMessage), 404 (ErrorMessage), 429 (ErrorMessage) GET `/epost/v2/matching-runs/processing/{matching-run-id}` — response body now documented for 401 (ErrorMessage), 404 (ErrorMessage), 429 (ErrorMessage) GET `/epost/v2/standard-matching-runs/{matching-run-id}` — response body now documented for 401 (ErrorMessage), 404 (ErrorMessage), 429 (ErrorMessage) GET `/epost/v2/standard-matching-runs/processing/{matching-run-id}` — response body now documented for 401 (ErrorMessage), 404 (ErrorMessage), 429 (ErrorMessage)

#### 31 August 2026 · 3 changed

#### Changed

- POST `/core/latest/tenants` — operationId added, `listTenants`. A generated client used to invent a method name from the path and will now use this id

- POST `/core/latest/token` — operationId added, `createPublicApiToken`. A generated client used to invent a method name from the path and will now use this id

- POST `/core/latest/token/by-microsoft` — operationId added, `createPublicApiTokenByMicrosoft`. A generated client used to invent a method name from the path and will now use this id

#### 12 August 2026 · 1 changed · 1 removed · breaking

> **Warning: Breaking changes**
>
> Existing integrations may be affected.

#### Changed

- GET `/epost/preview/delivery-prices/{preview-id}/status` — neue Statuscodes: 404; Statuscodes entfallen: Resource not found

#### Removed

- POST `/epost/v2/single-file-deliveries` — [FOR TRYING OUT IN STOPLIGHT] Creates a single document delivery.

### Preview builds

#### Preview, eighth build · Delivery response and status corrected, async limits

Corrections from a large production run against this API:

- **Correction: the delivery id comes back as `id`.** Getting started showed `deliveryId`. The status response uses `deliveryId` for the same value.

- **Correction: the status example now shows the real structure,** with delivered and failed documents and a `detail` list per recipient.

- **Correction: the asynchronous endpoint has limits.** At most 2,000 documents and 2,000 recipients per delivery. This page said it had none.

- **Set `displayDeliveredDocument` to see who received what.** Without it the status response does not list recipients.

- Smaller changes: how to check a delivery after a timeout, what the identity matching limit is not, and two new entries under Known limitations.

#### Preview, seventh build · eArchive, c/o addresses, iLetter callbacks corrected

Additions and corrections from a comparison with the ePost developer portal, plus eArchive:

- **eArchive is now part of this documentation.** List the folders of your storage, list the letters in a folder, and upload a document with its metadata into your archive. The endpoints are not new in the API; they were missing from this documentation.

- **Correction: the output call of an iLetter retries for 48 hours.** This page said at most three attempts. Three quick attempts are followed by a widening schedule up to 48 hours after the first one. Keep your de-duplication on `Idempotency-Key` wide enough to cover that.

- **Correction: your data-source endpoint is called more often than once per recipient.** Also on every change to an input a parameter is bound to, and in sequence for chained data sources. The limits of the call are now documented, as is what the recipient sees while a call fails.

- **New: c/o addresses.** Supported in the physical channel only. Leaving the c/o line out of the metadata can deliver the document digitally to the wrong person.

- **The physical channel has fields you can set.** Envelope window, service level, envelope size, duplex, destination country and the QR invoice page. The envelope window you lay your template out for is declared on the delivery.

- **How to send a short message.** Media type and `documentMessage`, which the SMS section did not state.

- Smaller additions: the answer format of a custom webhook, the title of an iLetter record, fallback text for optional placeholders, the length limit of `batchId`, two cases in which a UID cannot match, and where to find the Postman collection.

#### Preview, sixth build · iLetter, ePost Sign, localised document titles

New products and fields from the API release of 15 October 2026:

- **New chapter: [iLetter](https://developer.klara.ch/epost-preview/#iletter).** How to send an interactive letter from a Template, supply URL parameters and placeholders per document and per recipient, and what the two calls look like that the platform makes to your endpoints.

- **ePost Sign is now part of this documentation.** Submit a document for electronic signing and follow the signing process. See Signing in the reference.

- **Download what a recipient sends back.** When a recipient answers an iLetter with a file, you can now fetch it.

- **Document title and description per language.** Recipients see the title and description in their letterbox language when you provide one.

- **Smart Letter is no longer listed as a media type.** Use iLetter instead.

#### Preview, fifth build · Documented error bodies, server descriptions

The endpoint set is unchanged, 34 operations. One change to the contract, from the KLARA release of 18 September that the ePost endpoints share:

- **Every error response now carries a documented body.** All 33 operations that had undocumented error statuses now declare `ErrorMessage` for them, with `uuid`, `createdTime`, `code` and `message`. Nothing changes on the wire; your client can now generate a type for it.

- The two servers carry a description in the contract: the test environment states that nothing is charged and nothing is physically posted, production states that deliveries are chargeable.

#### Preview, fourth build · operationId for three shared endpoints

The endpoint set is unchanged. One change to the contract:

- **Three endpoints now carry an `operationId`:** `POST /core/latest/token` as `createPublicApiToken`, `POST /core/latest/token/by-microsoft` as `createPublicApiTokenByMicrosoft` and `POST /core/latest/tenants` as `listTenants`. These three are shared with KLARA business software, which is where the ids come from. If you generate a client, its method names for these three change once.

- **The 31 ePost endpoints still have none.** Generated clients keep inventing their method names from the path for those, and those names change whenever a path changes. See [Known limitations](https://developer.klara.ch/epost-preview/#roadmap).

#### Preview, third build · Machine-readable formats, three corrections

No change to the endpoint set. This build adds machine-readable versions of the documentation and corrects three statements in the text.

- **Machine-readable formats.** [`llms.txt`](llms.txt) indexes every chapter as a Markdown file and every area of the reference as a Markdown file with parameters and request and response bodies. Built in the same run as this page, from the same source. The specification itself is unchanged and remains the contract.

- The specification is now linked at the top of this page, not only inside its own chapter.

- This preview is no longer excluded from search engines. It stays marked as a preview. *Later correction: this entry named developer.epost.ch as the documentation in force. Since 21 September 2026 this page states instead that it is the most current description of the API and not yet the official developer portal.*

- Free-text search and filtering by field are described separately. Free-text search exists on one endpoint, `GET /epost/v2/letters/search`. Nine endpoints filter by field, with 22 parameters in total, and there you have to know the value you are filtering on. One endpoint accepts a sort order; everywhere else the order is not guaranteed.

- The page said three authentication endpoints. This contract publishes `POST /core/latest/token`, `POST /core/latest/token/by-microsoft` and `POST /core/latest/tenants`, so two of them are token endpoints.

- A tag description in the specification referred to operations that the file does not contain. Corrected, so the specification now describes only what it carries.

#### Preview, second build · One endpoint removed, one response corrected

Two changes to the published contract, both from an external review:

- `POST /epost/v2/single-file-deliveries` removed. Its own description says it exists only for trying the API out in Stoplight, so it does not belong in a customer contract. Ask [enterprise support](mailto:support.enterprise@epostservice.ch) if you were using it.

- `GET /epost/preview/delivery-prices/{preview-id}/status`: the response previously keyed as free text is now keyed `404`. The API behaviour is unchanged, only the specification was invalid before.

- The test server is now part of the specification, so generated clients and imported collections can select it directly.

#### Preview, first build · First build from the published specification

First build of this documentation from the published specification, across sending, identity matching, the digital letterbox and authentication.
