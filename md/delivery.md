# Sending documents

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#delivery

## Sending documents

A delivery carries one or more documents to one or more recipients. The platform decides how to reach each one, or you decide for it.

### Asynchronous or synchronous

| Endpoint | Returns | Use it when |
|---|---|---|
| `POST /epost/v2/deliveries` | `201` and a delivery id | Normal case. Accepts the delivery and processes it in the background. Poll the status endpoint afterwards |
| `POST /epost/v2/synchronous-deliveries` | `200` with the result | You need the outcome in the same request. Limited in volume, see below |

### Limits that the specification states

- **2 GB per delivery.**

- **200 MB per document**, counting the binary and its metadata together.

- **The synchronous endpoint takes at most 1500 documents** in one delivery when each document goes to one recipient, or one document to 1500 recipients. The asynchronous endpoint states no such limit.

### Checking what happened

```
GET /epost/v2/deliveries/{delivery-id}/status
```

Keep the delivery id. It is the only way back to a delivery you created.

### What the channel logic actually does

With `deliveryChannelPreferences: ["AUTO"]` the platform picks the channel from the credentials you supply. Four patterns, taken from the published examples:

| You supply | What happens |
|---|---|
| Postal address | Delivered into the ePost app if the recipient is there, otherwise printed and posted |
| Email address, document type `invoice` | ePost app first, then eBill, then plain email as the last resort. eBill only applies to invoices |
| Mobile number, short message | Push notification and message in ePost if the recipient is there, otherwise an ordinary SMS |
| Several recipients at once | Each one is routed on its own. One may get a printed letter while the others get email |

Set `deliveryChannelPreferences: ["DIGITAL"]` when you want digital delivery only and no physical fallback.

#### Fields worth knowing

- **`costCenter`** shows up in monitoring. Fill it, or you will not be able to attribute cost later.

- **`documentTypes`** steers the routing. `invoice` is what makes eBill possible at all.

- **`fileName`** must match the uploaded file exactly.

- **`allRecipientsRequired`** decides whether the whole delivery fails if one recipient cannot be reached.

- **`senderUserId`** is your own reference for the recipient. Use it to map the result back to your records.

### Monitoring across deliveries

`GET /epost/deliveries/monitoring` returns monitoring data for deliveries regardless of the channel they went out on. Use it for reporting rather than for checking a single item.
