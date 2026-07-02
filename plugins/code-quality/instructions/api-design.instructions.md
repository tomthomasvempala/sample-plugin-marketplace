---
description: "Use when designing, reviewing, or implementing REST API routes, controllers, or HTTP handlers. Covers resource naming, status codes, versioning, and error response shapes."
---

# REST API Design Guidelines

## Resource Naming

- Use plural nouns for collection endpoints: `/users`, `/orders`, not `/user`, `/getOrders`.
- Nest resources to show ownership, but limit to two levels: `/users/{id}/posts`, not `/users/{id}/posts/{postId}/comments`.
- Use kebab-case for multi-word segments: `/audit-logs`, not `/auditLogs`.

## HTTP Methods

| Action | Method | Example |
|--------|--------|---------|
| List | `GET` | `GET /users` |
| Get one | `GET` | `GET /users/{id}` |
| Create | `POST` | `POST /users` |
| Full replace | `PUT` | `PUT /users/{id}` |
| Partial update | `PATCH` | `PATCH /users/{id}` |
| Delete | `DELETE` | `DELETE /users/{id}` |

## Status Codes

- `200 OK` — successful GET, PUT, PATCH
- `201 Created` — successful POST that creates a resource; include `Location` header
- `204 No Content` — successful DELETE or action with no response body
- `400 Bad Request` — client validation error
- `401 Unauthorized` — missing or invalid credentials
- `403 Forbidden` — authenticated but not authorized
- `404 Not Found` — resource does not exist
- `409 Conflict` — state conflict (duplicate, optimistic lock failure)
- `422 Unprocessable Entity` — semantically invalid input
- `500 Internal Server Error` — unexpected server failure

## Error Response Shape

All errors must return a consistent JSON body:

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "Human-readable description",
    "details": [
      { "field": "email", "issue": "Must be a valid email address" }
    ]
  }
}
```

- `code` is a machine-readable constant — never change it across versions.
- `message` is for developers, not end users.
- `details` is optional and only present when field-level feedback is useful.

## Versioning

- Version via URL path prefix: `/v1/users`, `/v2/users`.
- Never remove or rename fields in a stable version — add new fields instead.
- Deprecate versions with a `Deprecation` response header before removing.

## Security

- Validate and sanitize all query parameters and request bodies before use.
- Do not include internal stack traces or database error messages in responses.
- Rate-limit all public endpoints.
