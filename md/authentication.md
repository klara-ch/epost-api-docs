# Authentication

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#authentication

## Authentication

The API accepts two mechanisms, and every endpoint that requires authentication accepts either.

| Mechanism | How | Best for |
|---|---|---|
| `apiKeyAuth` | Header `X-API-KEY` | Server-to-server integrations. Generate the key in the ePost interface |
| `bearerAuth` | Header `Authorization: Bearer <JWT>` | Flows that act for a signed-in user |

SwissID is not supported for API authentication.

### Token flow

A user can belong to several tenants. Resolve the tenant first, then request a token. Both endpoints take a form body, not JSON.

```
curl -X POST "https://api.epost.ch/core/latest/tenants" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "username=$EPOST_USER" \
  --data-urlencode "password=$EPOST_PASSWORD"

curl -X POST "https://api.epost.ch/core/latest/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "grant_type=password" \
  --data-urlencode "username=$EPOST_USER" \
  --data-urlencode "password=$EPOST_PASSWORD" \
  --data-urlencode "tenant_id=$TENANT_ID" \
  --data-urlencode "company_id=$COMPANY_ID"
```

Refreshing works the same way with `grant_type=refresh_token` and the `refresh_token` you received.

> **Warning: The specification marks no form field as required**
>
> The table above is taken from the endpoint descriptions, not from the schema. Expect `400` if a field is missing, and treat this as the working contract until the specification is corrected.

#### Endpoints that need no authentication

3 endpoints require no authentication, because they are how you obtain a token in the first place. Everything else in this reference accepts either an API key or a bearer token, and no endpoint accepts only one of the two.

- POST `/core/latest/tenants`: Returns all tenants of a user

- POST `/core/latest/token`: Generate tokens to access other KLARA core endpoints

- POST `/core/latest/token/by-microsoft`: Exchange Microsoft access token for system token
