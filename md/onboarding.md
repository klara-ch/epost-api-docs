# Getting recipients onto ePost

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#onboarding

## Getting recipients onto ePost

`POST /epost/onboarding` generates a link or a QR code. When a recipient opens or scans it, the actions you defined in the request body are carried out. Useful for getting a customer into ePost from your own onboarding flow.

**The `Accept` header decides the format:** `text/html` returns the URL, `image/png` returns the QR code image.

The verification score accepts `0` or `50`, and only one credential in the request body may carry one.
