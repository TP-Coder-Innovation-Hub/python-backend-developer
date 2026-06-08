# HTTP and Web Servers

``

When you type `https://example.com/users` into a browser and press Enter, here's what happens:

1. Your browser sends an **HTTP request** to the server at `example.com`.
2. The server processes the request and sends back an **HTTP response**.
3. Your browser renders the response.

HTTP (HyperText Transfer Protocol) is the language of the web. Every website, every API, every mobile app backend uses it.

> 🖼️ **[IMAGE_PLACEHOLDER]** — HTTP request response cycle browser server web server

## HTTP Request

A request has three parts:

```
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{"name": "Ada", "email": "ada@example.com"}
```

| Part | What it is | Example |
|------|-----------|---------|
| Method + URL | What action and where | `POST /users` |
| Headers | Metadata about the request | `Content-Type: application/json` |
| Body | Data being sent (optional) | `{"name": "Ada"}` |

## HTTP Methods

Methods describe the type of action:

| Method | Purpose | Has Body | Example |
|--------|---------|----------|---------|
| GET | Retrieve data | No | `GET /users` -- fetch all users |
| POST | Create something | Yes | `POST /users` -- create a new user |
| PUT | Replace something entirely | Yes | `PUT /users/1` -- replace user 1 |
| PATCH | Partially update something | Yes | `PATCH /users/1` -- update user 1's email |
| DELETE | Remove something | No | `DELETE /users/1` -- delete user 1 |

## HTTP Response

A response has three parts:

```
HTTP/1.1 201 Created
Content-Type: application/json

{"id": 1, "name": "Ada", "email": "ada@example.com"}
```

| Part | What it is | Example |
|------|-----------|---------|
| Status code + reason | Did it work? What happened? | `201 Created` |
| Headers | Metadata about the response | `Content-Type: application/json` |
| Body | Data being returned (optional) | `{"id": 1, "name": "Ada"}` |

## Status Codes

Status codes tell the client what happened. They're grouped by the first digit:

| Range | Category | Common Codes |
|-------|----------|-------------|
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirect | 301 Moved Permanently, 302 Found |
| 4xx | Client error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| 5xx | Server error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

Key codes to know:

- **200 OK** -- the request succeeded.
- **201 Created** -- a new resource was created (used with POST).
- **400 Bad Request** -- the client sent invalid data.
- **401 Unauthorized** -- the client isn't authenticated (not logged in).
- **403 Forbidden** -- the client is authenticated but not allowed to do this.
- **404 Not Found** -- the resource doesn't exist.
- **500 Internal Server Error** -- something broke on the server.

## Headers

Headers are key-value pairs that carry metadata. Common headers:

| Header | Direction | Purpose |
|--------|-----------|---------|
| `Content-Type` | Both | What format is the body (`application/json`) |
| `Authorization` | Request | Authentication credentials |
| `Accept` | Request | What formats the client can handle |
| `Cache-Control` | Both | Caching instructions |
| `User-Agent` | Request | What software is making the request |

## What is a Web Server

A web server is a program that listens for HTTP requests and returns HTTP responses. In Python backend development, your FastAPI (or Django, Flask) application **is** the web server's logic. A server like uvicorn or Gunicorn runs your application and handles the network communication.

```
Client (browser/app)  -->  HTTP request  -->  Web server (uvicorn)  -->  Your app (FastAPI)
                                                                   <--  HTTP response
```

You write the application logic. The server handles receiving requests and sending responses over the network.
