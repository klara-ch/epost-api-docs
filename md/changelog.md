# Changelog

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#changelog

## Changelog

What changed and when. Changes that can break an existing integration are marked as such.

> **Note: Preview, third build**
>
> No change to the endpoint set. This build adds machine-readable versions of the documentation and corrects three statements in the text.
>
> - **Machine-readable formats.** [`llms.txt`](llms.txt) indexes every chapter as a Markdown file and every area of the reference as a Markdown file with parameters and request and response bodies. Built in the same run as this page, from the same source. The specification itself is unchanged and remains the contract.
>
> - The specification is now linked at the top of this page, not only inside its own chapter.
>
> - This preview is no longer excluded from search engines. It stays marked as a preview, and [developer.epost.ch](https://developer.epost.ch) remains the documentation in force.
>
> - Free-text search and filtering by field are described separately. Free-text search exists on one endpoint, `GET /epost/v2/letters/search`. Nine endpoints filter by field, with 22 parameters in total, and there you have to know the value you are filtering on. One endpoint accepts a sort order; everywhere else the order is not guaranteed.
>
> - The page said three authentication endpoints. This contract publishes `POST /core/latest/token`, `POST /core/latest/token/by-microsoft` and `POST /core/latest/tenants`, so two of them are token endpoints.
>
> - A tag description in the specification referred to operations that the file does not contain. Corrected, so the specification now describes only what it carries.

> **Note: Preview, second build**
>
> Two changes to the published contract, both from an external review:
>
> - `POST /epost/v2/single-file-deliveries` removed. Its own description says it exists only for trying the API out in Stoplight, so it does not belong in a customer contract. Ask [enterprise support](mailto:support.enterprise@epostservice.ch) if you were using it.
>
> - `GET /epost/preview/delivery-prices/{preview-id}/status`: the response previously keyed as free text is now keyed `404`. The API behaviour is unchanged, only the specification was invalid before.
>
> The test server is now part of the specification, so generated clients and imported collections can select it directly.

> **Note: Preview, first build**
>
> First build of this documentation from the published specification, across sending, identity matching, the digital letterbox and authentication.
