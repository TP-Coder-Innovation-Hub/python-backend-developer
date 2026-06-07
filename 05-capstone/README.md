# Capstone Project

Build a complete backend service from scratch. This project integrates everything from the learning path.

## Requirements

Build a **Task Management API** -- a service where users can create, read, update, and delete tasks. Users authenticate, and each user sees only their own tasks.

### Functional Requirements

| Feature | Details |
|---------|---------|
| User registration | POST /auth/register with name, email, password |
| User login | POST /auth/login, returns a JWT token |
| Create task | POST /tasks with title, description, status |
| List tasks | GET /tasks with pagination and optional status filter |
| Get task | GET /tasks/{id} -- only the owner's tasks |
| Update task | PATCH /tasks/{id} -- update title, description, or status |
| Delete task | DELETE /tasks/{id} -- only the owner can delete |
| Health check | GET /health -- returns service status |

### Technical Requirements

| Requirement | Implementation |
|-------------|---------------|
| Framework | FastAPI |
| Database | PostgreSQL (or SQLite for development) |
| ORM | SQLAlchemy 2.0 |
| Migrations | Alembic |
| Validation | Pydantic v2 models for all requests and responses |
| Authentication | JWT tokens |
| Testing | pytest with httpx for API tests |
| Containerization | Dockerfile for deployment |
| Configuration | pydantic-settings with environment variables |
| Code quality | ruff for linting and formatting |

### Data Model

```
User:
  id: int (primary key)
  email: str (unique)
  name: str
  hashed_password: str
  created_at: datetime

Task:
  id: int (primary key)
  title: str
  description: str (optional)
  status: enum (todo, in_progress, done)
  owner_id: int (foreign key -> User)
  created_at: datetime
  updated_at: datetime
```

### API Endpoints

```
POST   /auth/register         -- create account
POST   /auth/login            -- get JWT token
GET    /tasks                 -- list your tasks (paginated)
POST   /tasks                 -- create a task
GET    /tasks/{task_id}       -- get a specific task
PATCH  /tasks/{task_id}       -- update a task
DELETE /tasks/{task_id}       -- delete a task
GET    /health                -- health check
```

## Project Structure

```
task-api/
  pyproject.toml
  uv.lock
  Dockerfile
  .env.example
  migrations/            -- Alembic migrations
  src/
    main.py              -- FastAPI app setup
    config.py            -- Settings class
    database.py          -- SQLAlchemy engine and session
    models.py            -- SQLAlchemy models (User, Task)
    schemas.py           -- Pydantic models (request/response)
    auth.py              -- JWT creation and verification
    routers/
      auth.py            -- /auth/* endpoints
      tasks.py           -- /tasks/* endpoints
      health.py          -- /health endpoint
  tests/
    conftest.py          -- test fixtures (client, database)
    test_auth.py         -- authentication tests
    test_tasks.py        -- task CRUD tests
```

## What You Should Be Able to Do

By completing this project, you will have demonstrated:

- Writing Python code with proper structure and types
- Building a REST API with FastAPI
- Modeling data with SQLAlchemy and migrating with Alembic
- Implementing JWT authentication
- Writing tests for API endpoints
- Containerizing with Docker
- Managing configuration with environment variables
- Following code quality standards with ruff

Start with the project setup (Phase 1), then build features one at a time. Test each feature before moving to the next. This project is small enough to complete in a weekend but covers every major backend concept.
