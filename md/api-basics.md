# API basics

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#api-basics

## API basics

### API routes

Two hosts, the same endpoints on both. Paths in this reference are relative and must be combined with a host:

| Environment | Host | What it does |
|---|---|---|
| Test | `https://api-test.klara-epost.tech` | Self-service, free, nothing is physically posted |
| Production | `https://api.epost.ch` | Deliveries are chargeable |

```
POST /epost/v2/deliveries
→ https://api-test.klara-epost.tech/epost/v2/deliveries
```

Two prefixes are in use. `/epost/v2` holds deliveries, identity matching and the digital letterbox. `/epost/preview` holds the two preview services. The three authentication endpoints sit on `/core/latest`, because they are shared with KLARA business software and run on the same platform.

### HTTP verbs

| Verb | Used for | Endpoints |
|---|---|---|
| GET | Retrieving resources | 16 |
| POST | Creating resources, and some actions such as `/send` | 16 |
| DELETE | Removing a resource | 1 |
| PATCH | Partial updates | 1 |

34 endpoints in total, which is the same number as in the reference below and in the downloadable specification.

### Headers

| Header | Value | When |
|---|---|---|
| `X-API-KEY` | your API key | API key authentication |
| `Authorization` | `Bearer <JWT>` | Token authentication |
| `Accept` | depends on the endpoint | Mostly `application/json`. The onboarding endpoint returns `text/html` or `image/png` depending on what you ask for |
| `Content-Type` | depends on the endpoint | Deliveries and previews take `multipart/form-data`, identity matching takes `application/json`, see below |

### Media types

#### Request bodies

Send the matching `Content-Type`. Form-encoded endpoints reject a JSON body, which is the most common reason a first token request fails.

| Media type | Endpoints | Which ones |
|---|---|---|
| `application/json` | 6 | `POST /epost/onboarding` `POST /epost/v2/letters/read` `POST /epost/v2/matching-runs` `POST /epost/v2/matchings` `POST /epost/v2/standard-matching-runs` `POST /epost/v2/standard-matchings` |
| `multipart/form-data` | 4 | `POST /epost/preview/delivery-channels` `POST /epost/preview/delivery-prices` `POST /epost/v2/deliveries` `POST /epost/v2/synchronous-deliveries` |
| `application/x-www-form-urlencoded` | 3 | `POST /core/latest/tenants` `POST /core/latest/token` `POST /core/latest/token/by-microsoft` |

#### Responses

Ask for what the endpoint returns. `Accept: application/json` is correct almost everywhere, but not for the binary and PDF responses below.

| Media type | Endpoints | Which ones |
|---|---|---|
| `application/json` | 33 | every endpoint that has one |
| `application/octet-stream` | 1 | `GET /epost/v2/letters/{letter-id}/thumbnail` |
| `application/octet-stream, application/json` | 1 | `GET /epost/v2/letters/{letter-id}/content` |
| `image/png, application/json` | 1 | `POST /epost/onboarding` |
| `text/html;charset=UTF-8, application/json` | 1 | `POST /epost/onboarding` |
