# Previewing channel and price

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#preview

## Previewing channel and price

Two services answer the question «what would happen if I sent this» without sending anything. Both are two-step: you post the request, then poll for the result.

| Question | Post to | Then poll |
|---|---|---|
| Which channels can I reach this recipient on? | `POST /epost/preview/delivery-channels` | `GET /epost/preview/delivery-channels/{preview-id}/status` |
| What would this delivery cost? | `POST /epost/preview/delivery-prices` | `GET /epost/preview/delivery-prices/{preview-id}/status` |

Both take `multipart/form-data` and both carry the same size limits as a real delivery: 2 GB per request, 200 MB per document.

> **Note: Why this matters more than it looks**
>
> Delivery is charged per item and the price depends on the channel. Asking first turns a cost surprise into a decision you make in code.
