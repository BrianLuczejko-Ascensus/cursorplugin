---
name: api-design
description: REST API design guidelines and conventions
---

# API Design Rules

These rules define the REST API conventions that DevPulse enforces.

## URL Structure

- Use lowercase, hyphen-separated paths: `/user-profiles` not `/userProfiles`
- Use nouns for resources: `/orders`, `/users`
- Use verbs only for actions that don't map to CRUD: `/reports/generate`
- Nest related resources: `/users/:id/orders`
- Max nesting depth: 2 levels

## HTTP Methods

| Method | Purpose | Idempotent | Request Body |
|--------|---------|------------|--------------|
| GET | Read resource(s) | Yes | No |
| POST | Create resource | No | Yes |
| PUT | Full replace | Yes | Yes |
| PATCH | Partial update | Yes | Yes |
| DELETE | Remove resource | Yes | No |

## Response Codes

- `200` — Success (GET, PUT, PATCH)
- `201` — Created (POST)
- `204` — No Content (DELETE)
- `400` — Bad Request (validation failure)
- `401` — Unauthorized (missing/invalid auth)
- `403` — Forbidden (insufficient permissions)
- `404` — Not Found
- `409` — Conflict (duplicate resource)
- `422` — Unprocessable Entity (semantic validation)
- `429` — Too Many Requests (rate limited)
- `500` — Internal Server Error

## Pagination

All list endpoints must support pagination:

```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 25,
    "total": 142,
    "total_pages": 6
  }
}
```

Use query parameters: `?page=2&per_page=25`

## Error Responses

Return consistent error objects:

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "Email address is invalid",
    "details": [
      { "field": "email", "message": "must be a valid email address" }
    ]
  }
}
```

## Versioning

- Use URL path versioning: `/api/v1/users`
- Never break existing versions; add new versions for breaking changes
- Deprecate old versions with a sunset header and migration guide

## Input Validation

- Validate all inputs at the API boundary
- Return specific field-level errors (not generic "bad request")
- Apply size limits to string fields and array lengths
- Reject unknown fields in strict mode or ignore them silently (pick one and be consistent)
