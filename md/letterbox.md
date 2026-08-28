# Digital letterbox

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#letterbox

## Digital letterbox

Thirteen endpoints read and manage the letters of the authenticated user: list, search, read status, content, thumbnail, accept, reject, archive, delete and restore.

These act on the letterbox of the user behind the credentials, not on a company mailbox. They are the receiving side of ePost, where the rest of this documentation is about sending.

> **Warning: A private account needs a password first**
>
> Private recipients are onboarded through the mobile app only, companies through the web app at `app.epost.ch`. Private accounts sign in with SwissID, which the API does not accept. Set a password once, then authenticate with email and password. Steps under [Getting access](https://developer.klara.ch/epost-preview/#access).

See the [reference](https://developer.klara.ch/epost-preview/#ref-digital-letterbox) for the full list. Note that most of these endpoints carry only a one-line summary in the specification today.

### When a letterbox will not accept your delivery

Recipients control their own letterbox, and three of those controls cause deliveries to fail in ways that look like a technical error but are not.

| The recipient has | What happens to your delivery |
|---|---|
| Deactivated the letterbox entirely | No digital delivery |
| Blocked you as a sender | No digital delivery from you, while other senders still get through |
| Opted out of a document type | No digital delivery for that type. The platform reads the type from your `documentTypes` field |

> **Warning: Letterboxes go dormant after 200 days**
>
> If a recipient receives a new eLetter and does not open their letterbox for 200 days, ePost deactivates it. Recipients are warned beforehand by email, push notification and SMS. So a participant id that matched last year may no longer be deliverable today. Match again rather than caching participant ids indefinitely.

This is also why [matching before delivery](https://developer.klara.ch/epost-preview/#matching) pays off at volume: it tells you who is actually reachable before you commit to a run.
