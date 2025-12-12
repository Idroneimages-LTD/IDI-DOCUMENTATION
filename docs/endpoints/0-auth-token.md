---
layout: default
title: Auth - Get Token
parent: Endpoints
nav_order: 0
---

# Get Access Token

Obtain an OAuth 2.0 access token using client credentials.

| Detail | Specification |
|:-------|:--------------|
| **Method** | `POST` |
| **URI** | `/v3/auth/token` |
| **Security** | API Key (`x-api-key` header) |
| **Success Response** | `200 OK` |

## Request Headers

| Header | Required | Description |
|:-------|:---------|:------------|
| `x-api-key` | Yes | Your API key |
| `Content-Type` | Yes | `application/json` |

## Request Body

| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `grant_type` | String | Yes | Must be `client_credentials` |
| `client_id` | String | Yes | Your client ID |
| `client_secret` | String | Yes | Your client secret |

## Example Request

```bash
curl -X POST {{baseUrl}}/v3/auth/token \
  -H "x-api-key: {{apiKey}}" \
  -H "Content-Type: application/json" \
  -d '{
    "grant_type": "client_credentials",
    "client_id": "{{clientId}}",
    "client_secret": "{{clientSecret}}"
  }'
```

## Success Response (200)

```json
{
  "access_token": "eyJraWQiOiJEaWVoK0U0...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

| Field | Type | Description |
|:------|:-----|:------------|
| `access_token` | String | JWT token for API authentication |
| `token_type` | String | Always `Bearer` |
| `expires_in` | Integer | Token validity in seconds (3600 = 1 hour) |

## Error Responses

| Status | Error | Description |
|:-------|:------|:------------|
| 400 | `invalid_request` | Missing or invalid parameters |
| 400 | `unsupported_grant_type` | Only `client_credentials` is supported |
| 401 | `invalid_client` | Invalid client credentials |
| 403 | - | Missing or invalid API key |
