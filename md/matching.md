# Identity matching

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#matching

## Identity matching

Identity matching answers whether a recipient of yours exists in ePost, so you know before sending whether a digital channel is available.

> **Warning: The detailed tier is chargeable and needs approval**
>
> `POST /epost/v2/matchings` and `POST /epost/v2/matching-runs` return detailed recipient feedback. The specification states that access is restricted and available against a fee. Clarify this with [enterprise support](mailto:support.enterprise@epostservice.ch) before you design around them. The standard tier needs no approval.

### Two tiers, and the difference matters commercially

| Tier | Endpoints | Returns | Access |
|---|---|---|---|
| **Standard** | `POST /epost/v2/standard-matchings` `POST /epost/v2/standard-matching-runs` | The user's basic information | Included |
| **Detailed** | `POST /epost/v2/matchings` `POST /epost/v2/matching-runs` | Detailed information on whether a credential matched | **Restricted and chargeable.** The specification says access is restricted and available against a fee, and asks you to contact an administrator |

### Single call or run

The `-matchings` endpoints answer in the same request. The `-matching-runs` endpoints return `202` and are polled through `/processing/{matching-run-id}` and then read with `/{matching-run-id}`. Use runs for larger sets.

### What to send

- **`recipients` is the only required field.**

- **You may hash the metadata before sending it.** The specification states this explicitly, and `GET /epost/v2/recipients` returns hashed metadata from ePost users for exactly this purpose.

- **You do not have to send everything,** but the combination you send has to be unique. Sending only a common surname will not identify anyone.

> **Warning: One limit in the specification is incomplete**
>
> The description reads «The limit size of the metadata is 5» without a unit. Five what, fields or entries, is not stated. Treat it as a hard limit of five until it is clarified.

### Before delivery or on delivery

There are two ways to combine matching with sending, and the choice matters at volume.

|  | Matching before delivery | Matching on delivery |
|---|---|---|
| How | You match first, keep the participant ids, then deliver against them | You pass the credentials with the delivery and the platform matches and routes in one call |
| Use it for | **High volume.** Explicitly recommended | Small numbers, and when you want the fallback to happen automatically |
| You get back | Participant ids you can store | A delivery result |

#### Matching before delivery

Diagram: Sequence: optional matching lookup, then identity matching, then matches including the participant id

Matching before delivery. You get participant ids back and use them for the delivery.

#### Matching on delivery

Diagram: Sequence: one call carries document and credentials, ePost matches and either delivers digitally, hands over to a print partner, or returns an error

Matching on delivery. One call, and the channel is decided inside it.

### Which credentials identify a recipient

Every letterbox carries a **participant id**, a long unique key such as `1a234567-1234-12a3-a1bc-12a3b4c5d678`. It is not published anywhere. You obtain it through matching and then use it for delivery.

A credential is either **primary** or not. Only primary credentials can produce a match, and at least one primary credential must be present. Everything else narrows the result.

#### Private recipients

| Credential | Primary | How it gets verified |
|---|---|---|
| Postal address | **yes** | Code by physical letter |
| Email address | **yes** | Code by email |
| Mobile phone number | **yes** | Code by SMS |
| PO box | **yes** | Code by physical letter |
| Participant ID | **yes** | The unique id of a letterbox. Can be used directly for delivery |
| Legacy ID | **yes** | Old key from E-Post Office |
| First name, last name, date of birth | no | Would need identity document verification, which is not available |
| Social security number | no |  |

#### Company recipients

| Credential | Primary | Note |
|---|---|---|
| Postal address | **yes** | Code by physical letter |
| PO box | **yes** | Code by physical letter |
| Participant ID | **yes** | Can be used directly for delivery |
| UID | no | Cannot produce a match on its own. Supply a postal address, PO box or participant id as well |

> **Note: Only verified credentials count**
>
> If a recipient entered a new email address but has not confirmed it, that address is not used for matching. This is why an address you know to be correct can still fail to match.

> **Warning: Matching lookup is not the default path**
>
> The optional lookup step in the diagram above is open to selected partners only, it is not recommended, and it lowers the match rate noticeably. Use plain matching unless you have agreed otherwise with [enterprise support](mailto:support.enterprise@epostservice.ch).

### How to get the best match rate

- **Send every primary credential you have** and set `allCredentialShouldMatch` to `false`. That is the single most effective setting. With `true`, one wrong field kills the whole match.

- **Be specific about the recipient type.** Fill `firstName` and `lastName` for a person, `companyName` for a company. Use `universalName` only when you genuinely do not know which it is.

- **Structure the address.** Use `unstructuredAddressLine` only when you cannot split the parts. A structured address matches better.

- **The more precise the data, the higher the digital delivery rate.** Matching quality and postage cost are the same lever.

### Level of trust

Every recipient carries a level that says how well they are identified. You can require a minimum through `minimumLevelOfTrust`.

| Level | Private recipients | Company recipients |
|---|---|---|
| `BRONZE` | Email and mobile number verified. Every private recipient starts here | not applicable |
| `SILVER` | Postal address verified by letter | Postal address verified by letter. Every company starts here |
| `GOLD` | **Not implemented.** Identity document plus live photo. Ask [enterprise support](mailto:support.enterprise@epostservice.ch) if you need it |  |

> **Warning: A higher requirement means fewer matches**
>
> Recipients are encouraged to raise their level but never forced. Requiring `SILVER` excludes everyone who has not verified a postal address, even though they exist in ePost and could be reached.

### Hashing the credentials

You may send credentials hashed instead of in clear text. **It is not recommended**, because match quality drops, particularly for postal addresses. If your data protection rules require it, the rules are exact.

- **Algorithm: SHA-256.**

- **Email:** `local-part@domain`, all lower case.

- **Mobile number:** E.164, so country code with a leading `+`, twelve characters in total.

- **Postal address:** first name, last name, street, street number, postcode and city concatenated. Names normalised first: special characters removed, everything lower case. Additional address lines and further names are ignored.

- **Derived credentials** are hashed on top of a primary one: hash the derived value, append it to the primary hash, then hash the two together.

### Two tenant types, and what they may do

ePost distinguishes private and company tenants. It affects what you can expect.

- **Private recipients** are onboarded through the mobile app and cannot send, except when replying to a Smart eLetter.

- **Company tenants** use the web interface at `app.epost.ch` and can send anything.

- **One person can hold several tenants,** and there is no limit. A single communication can therefore reach several letterboxes. Deliver to all active ones. ePost keeps only letterboxes that are actually used marked as active.
