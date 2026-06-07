# Authentication

`[Mid]`

Authentication proves **who you are**. Authorization decides **what you can do**. These are different things, and both matter.

## Authentication vs Authorization

| Concept | Question | Example |
|---------|----------|---------|
| Authentication | "Who are you?" | Login with email and password |
| Authorization | "What can you do?" | Admins can delete users; regular users cannot |

Authentication comes first. You can't check permissions if you don't know who's asking.

## Sessions

The traditional approach:

1. User sends username and password.
2. Server validates credentials.
3. Server creates a **session** (stored server-side) and sends back a **session ID** in a cookie.
4. On subsequent requests, the browser sends the cookie. The server looks up the session.

```
Login:    POST /login  {"email": "...", "password": "..."}
Response: Set-Cookie: session_id=abc123

Request:  GET /users  Cookie: session_id=abc123
Server:   Looks up session_id=abc123 -> user_id=42 -> authorized
```

**Pros:** Server can invalidate sessions instantly (delete from store). Simple for browsers (cookies are sent automatically).

**Cons:** Server must store session state (memory or database). Doesn't scale well across multiple servers without a shared session store.

## JWT (JSON Web Tokens)

The modern approach for APIs:

1. User sends credentials.
2. Server validates credentials.
3. Server creates a **JWT** (a signed token) and returns it.
4. Client stores the token and sends it in the `Authorization` header on every request.
5. Server verifies the token's signature to confirm authenticity.

```
Login:    POST /login  {"email": "...", "password": "..."}
Response: {"token": "eyJhbGciOiJIUzI1NiIs..."}

Request:  GET /users  Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Server:   Verifies token signature -> extracts user_id=42 -> authorized
```

A JWT has three parts (separated by dots):

```
header.payload.signature
```

- **Header:** algorithm and token type.
- **Payload:** claims (user ID, role, expiration time).
- **Signature:** proves the token wasn't tampered with (signed with a secret key).

**Pros:** Stateless -- server doesn't store anything. Scales easily. Works across services.

**Cons:** Can't invalidate a token before it expires (unless you add a blocklist). Token contains readable data (don't put secrets in it).

## OAuth Concepts

OAuth is a protocol for **delegated access**. Instead of managing passwords yourself, you let a provider (Google, GitHub, Microsoft) authenticate users for you.

The flow:

1. Your app redirects the user to the provider's login page.
2. User logs in on the provider's site and grants your app permission.
3. Provider redirects back to your app with an authorization code.
4. Your app exchanges the code for an access token.
5. Your app uses the token to fetch the user's profile from the provider.

OAuth solves the "don't store passwords" problem. Users trust Google with their password, not your new app.

## Implementing Auth in FastAPI

```python
from fastapi import FastAPI, Depends, HTTPException
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
import jwt

SECRET = "your-secret-key"
app = FastAPI()
security = HTTPBearer()

def get_current_user(credentials: HTTPAuthorizationCredentials = Depends(security)):
    token = credentials.credentials
    try:
        payload = jwt.decode(token, SECRET, algorithms=["HS256"])
        return payload
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401, detail="Invalid token")

@app.get("/me")
async def me(user=Depends(get_current_user)):
    return {"user_id": user["sub"], "email": user["email"]}
```

Step by step:
- `HTTPBearer()` -- tells FastAPI to expect a `Bearer` token in the `Authorization` header.
- `Depends(security)` -- extracts the token from the request automatically.
- `jwt.decode()` -- verifies the signature and extracts the payload.
- `Depends(get_current_user)` -- any endpoint that needs authentication uses this dependency.

## Security Rules

| Rule | Why |
|------|-----|
| Never store plaintext passwords | Use bcrypt or argon2 to hash passwords |
| Use HTTPS | Tokens and passwords sent over HTTP are visible to anyone |
| Set token expiration | JWTs should expire (15 min access + refresh token pattern) |
| Validate everything | Never trust data from the client without validation |
| Keep secrets secret | Never commit API keys or JWT secrets to git |
