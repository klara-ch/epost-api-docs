# Known limitations

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#roadmap

## Known limitations

Gaps we know about, listed here so you find out before you build rather than halfway through.

- **Most endpoints carry only a one-line summary.** 20 of 48 have no description beyond the title, and the digital letterbox is affected almost entirely. The parameters and schemas are complete, the prose is not.

- **No permissions are documented.** No endpoint states which role or permission it needs.

- **The rate limit is not published.** See [Limits](https://developer.klara.ch/epost-preview/#limits).

- **No processing time is published.** The specification names the statuses of a delivery, but not how long one may stay in `PROCESSING` or whether the platform gives up after a while. Poll at a steady pace and keep the delivery id until the status is final.

- **Failure codes are not listed.** `errorCode` and `errorMessage` of a failed delivery carry values the specification does not enumerate. Store them as they arrive, so you can ask about them later.

- **The print-partner interfaces are not part of this documentation.** The APIs with which a print partner reports the result of a print job, and the printer-driver API, exist but are meant for print partners, not for senders. Ask [enterprise support](mailto:support.enterprise@epostservice.ch) if you are one.
