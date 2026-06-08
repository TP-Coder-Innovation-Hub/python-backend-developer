# REST API Design

``

REST (REpresentational State Transfer) is a convention for designing APIs over HTTP. It gives your API a predictable structure that clients (browsers, mobile apps, other services) can understand.

## Resources and URLs

A REST API is organized around **resources** -- things your API manages (users, orders, products). Each resource has a URL.

```
/users              -- the collection of users
/users/42           -- a specific user (ID 42)
/users/42/orders    -- orders belonging to user 42
```

Rules for URLs:

- Use **nouns**, not verbs: `/users` not `/getUsers`.
- Use **plural** nouns: `/users` not `/user`.
- Nest to show relationships: `/users/42/orders` means "orders for user 42."
- Keep it shallow: avoid more than 2 levels of nesting. If `/users/42/orders/5/items/3` feels deep, use `/orders/5/items/3` with a filter.

## HTTP Verbs Map to Actions

> 🖼️ **[IMAGE_PLACEHOLDER]** — REST API URL mapping verbs CRUD operations table

Combine the URL (the resource) with the HTTP method (the action):

| Method | URL | Action |
|--------|-----|--------|
| GET | `/users` | List all users |
| GET | `/users/42` | Get user 42 |
| POST | `/users` | Create a new user |
| PUT | `/users/42` | Replace user 42 entirely |
| PATCH | `/users/42` | Update part of user 42 |
| DELETE | `/users/42` | Delete user 42 |

The URL tells you *what*. The method tells you *what to do with it*.

## Status Codes

Return the right status code:

| Situation | Code |
|-----------|------|
| Successful read | 200 OK |
| Successful creation | 201 Created |
| Successful deletion | 204 No Content |
| Bad request (invalid data) | 400 Bad Request |
| Not authenticated | 401 Unauthorized |
| Not allowed (wrong permissions) | 403 Forbidden |
| Resource doesn't exist | 404 Not Found |
| Validation error | 422 Unprocessable Entity |
| Server error | 500 Internal Server Error |

## Request and Response Bodies

Use JSON for both requests and responses:

```
POST /users
Content-Type: application/json

{"name": "Ada", "email": "ada@example.com"}
```

```
201 Created
Content-Type: application/json

{"id": 1, "name": "Ada", "email": "ada@example.com"}
```

## Pagination

Never return an unbounded list. Large datasets need pagination:

```
GET /users?page=2&per_page=20
```

Response includes pagination metadata:

```json
{
    "data": [{"id": 21, "name": "..."}, {"id": 22, "name": "..."}],
    "page": 2,
    "per_page": 20,
    "total": 150,
    "total_pages": 8
}
```

Common pagination strategies:

| Strategy | How it works | Best for |
|----------|-------------|----------|
| Offset-based | `?page=2&per_page=20` | Small-to-medium datasets, random access |
| Cursor-based | `?cursor=abc123` | Large datasets, real-time data |

## Error Responses

Return errors in a consistent format:

```json
{
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Email is invalid",
        "details": [
            {"field": "email", "message": "Must be a valid email address"}
        ]
    }
}
```

Consistency matters. If every endpoint returns errors in the same structure, clients can handle errors uniformly.

## What Makes a Good API

| Quality | What it looks like |
|---------|-------------------|
| Predictable | Developers can guess the URL and method before reading docs |
| Consistent | Same patterns everywhere -- error format, pagination, naming |
| Documented | Every endpoint has a description, parameters, and examples |
| Versioned | `/v1/users` so you can change the API without breaking clients |
| Secure | Authentication required, input validated, rate limiting |
