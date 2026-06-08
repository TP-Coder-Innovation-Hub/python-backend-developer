# Your First API

``

Build a working REST API in under 5 minutes using FastAPI.

## Step 1: Set Up the Project

```bash
mkdir my-api && cd my-api
uv init
uv add fastapi uvicorn
```

This creates a project and installs two packages:
- **FastAPI** -- the web framework. You write your API logic here.
- **uvicorn** -- the server that runs your FastAPI application.

## Step 2: Write the API

Create `main.py`:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class CreateUserRequest(BaseModel):
    name: str
    email: str

class UserResponse(BaseModel):
    id: int
    name: str
    email: str

users_db: list[UserResponse] = []
next_id = 1

@app.get("/users", response_model=list[UserResponse])
async def list_users():
    return users_db

@app.post("/users", response_model=UserResponse, status_code=201)
async def create_user(req: CreateUserRequest):
    global next_id
    user = UserResponse(id=next_id, name=req.name, email=req.email)
    users_db.append(user)
    next_id += 1
    return user

@app.get("/users/{user_id}", response_model=UserResponse)
async def get_user(user_id: int):
    for user in users_db:
        if user.id == user_id:
            return user
    from fastapi import HTTPException
    raise HTTPException(status_code=404, detail="User not found")
```

Line by line for the key parts:

- `app = FastAPI()` -- creates the application instance.
- `class CreateUserRequest(BaseModel)` -- defines what a create-user request looks like. Pydantic validates incoming JSON against this model.
- `class UserResponse(BaseModel)` -- defines the response shape.
- `users_db` -- an in-memory list (not a real database, but enough for learning).
- `@app.get("/users")` -- maps GET requests to `/users` to the `list_users` function.
- `@app.post("/users", status_code=201)` -- maps POST requests to `/users`, returns 201 on success.
- `@app.get("/users/{user_id}")` -- `{user_id}` is a path parameter. FastAPI extracts it and passes it to the function as `user_id: int`. The `: int` part also validates the input.
- `raise HTTPException(status_code=404)` -- returns a 404 error response.

## Step 3: Run the API

```bash
uv run uvicorn main:app --reload
```

- `main:app` -- tells uvicorn to find the `app` object in `main.py`.
- `--reload` -- automatically restarts when you change the code (for development only).

Output:

```
INFO:     Uvicorn running on http://127.0.0.1:8000
```

## Step 4: Test It

Open a new terminal. Test with `curl`:

```bash
# Create a user
curl -X POST http://localhost:8000/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Ada", "email": "ada@example.com"}'

# Response:
# {"id":1,"name":"Ada","email":"ada@example.com"}

# List users
curl http://localhost:8000/users

# Response:
# [{"id":1,"name":"Ada","email":"ada@example.com"}]

# Get a specific user
curl http://localhost:8000/users/1

# Response:
# {"id":1,"name":"Ada","email":"ada@example.com"}

# Get a non-existent user
curl http://localhost:8000/users/999

# Response:
# {"detail":"User not found"}
```

## Step 5: Check the Auto-Generated Docs

Open http://localhost:8000/docs in your browser. FastAPI automatically generates an interactive API documentation page (Swagger UI) from your code. You can test every endpoint directly from the browser.

This is one of FastAPI's biggest strengths: your type hints and Pydantic models produce complete, accurate documentation with zero extra work.

## What You Built

| Feature | How |
|---------|-----|
| REST endpoints | `@app.get`, `@app.post` decorators |
| Request validation | Pydantic `BaseModel` classes |
| Response formatting | `response_model` parameter |
| Error handling | `HTTPException` |
| Auto documentation | FastAPI generates from your code |

This is a complete, functional API. Real applications swap the in-memory list for a database and add authentication, but the structure is the same.
