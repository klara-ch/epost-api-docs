# Known limitations

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#roadmap

## Known limitations

Gaps we know about, listed here so you find out before you build rather than halfway through.

- **Most endpoints carry only a one-line summary.** 18 of 34 have no description beyond the title, and the digital letterbox is affected almost entirely. The parameters and schemas are complete, the prose is not.

- **No endpoint carries an `operationId`.** Generated clients therefore invent method names from the path, and those names change if a path ever changes.

- **No permissions are documented.** No endpoint states which role or permission it needs.

- **The rate limit is not published.** See [Limits](https://developer.klara.ch/epost-preview/#limits).

- **eArchive is not part of this documentation.** Three endpoints exist under `/epost/v2/archives` but are not in the published API group.
