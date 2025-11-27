---
layout: default
title: Versioning
nav_order: 5
---

# Versioning and Deprecation Policy

## Versioning Standard

We use **Header-Based Versioning** with custom Vendor Media Types.

```http
Accept: application/vnd.idi-fly.api-v3+json
```

## Default Behavior

- **Default Version:** When the `Accept` header is missing, the API defaults to the latest stable version.
- **Fallback Logic:** Unsupported versions are routed to the nearest compatible version when possible.

## Deprecation Communication

```http
Warning: 299 - "This version will be deprecated on 2025-12-31"
Sunset: Sat, 31 Dec 2025 23:59:59 GMT
```

## Unsupported Version Error

```json
{
  "error": "UnsupportedMediaType",
  "message": "The requested API version is not supported",
  "supportedVersions": [
    "application/vnd.idi-fly.api-v3+json",
    "application/vnd.idi-fly.api-v2+json"
  ],
  "code": 415
}
```
