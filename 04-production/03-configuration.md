# Configuration

``

Hardcoded values (database URLs, API keys, port numbers) in your source code are a problem. They change between environments (development, staging, production) and secrets shouldn't be in git. Configuration externalizes these values.

## Environment Variables

Environment variables are the standard way to configure services. Every value that changes between environments should be an environment variable.

```python
import os

database_url = os.environ["DATABASE_URL"]
port = int(os.environ.get("PORT", "8000"))
debug = os.environ.get("DEBUG", "false").lower() == "true"
```

- `os.environ["DATABASE_URL"]` -- raises `KeyError` if not set. Use this for required values.
- `os.environ.get("PORT", "8000")` -- returns `"8000"` if not set. Use this for values with sensible defaults.

Set them when running:

```bash
DATABASE_URL=postgresql://localhost/mydb DEBUG=true uv run uvicorn main:app
```

## .env Files

Typing environment variables every time is tedious. A `.env` file stores them:

```
DATABASE_URL=postgresql://localhost/mydb
PORT=8000
DEBUG=true
SECRET_KEY=your-secret-key-here
```

Load with a settings class (see below). **Never commit `.env` files to git.** Add `.env` to `.gitignore`.

## Settings Class with Pydantic

Use Pydantic's `BaseSettings` to manage configuration with type validation:

```bash
uv add pydantic-settings
```

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str
    port: int = 8000
    debug: bool = False
    allowed_origins: list[str] = ["*"]

    class Config:
        env_file = ".env"

settings = Settings()
```

Step by step:
- `database_url: str` -- required. Raises `ValidationError` if not set.
- `port: int = 8000` -- optional, defaults to 8000.
- `debug: bool = False` -- automatically converts `"true"` / `"false"` strings to boolean.
- `env_file = ".env"` -- reads values from a `.env` file if it exists.

Use it in your application:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/config")
async def show_config():
    return {
        "port": settings.port,
        "debug": settings.debug,
        "database_url": settings.database_url,
    }
```

## Why Not Hardcode

```python
# BAD -- hardcoded
database_url = "postgresql://prod-db.example.com/myapp"

# GOOD -- from environment
database_url = settings.database_url
```

| Problem | Hardcoded | Environment variable |
|---------|-----------|---------------------|
| Different per environment | Change code for each env | Same code, different values |
| Secrets in git | Yes, dangerous | No, safe |
| Easy to change | Requires code change + deploy | Restart with new value |
| Team members have different setups | Everyone uses same value | Each person has their own `.env` |

## Secret Management Basics

For production, don't store secrets in `.env` files on the server. Use a secret manager:

| Tool | When to use |
|------|------------|
| `.env` file | Local development only |
| Cloud secret manager (AWS Secrets Manager, GCP Secret Manager) | Production |
| Kubernetes secrets | If running on Kubernetes |
| HashiCorp Vault | Enterprise, on-premises |

The principle: secrets should never be in source control, never in container images, and always encrypted at rest.

## Configuration Checklist

- Every value that differs between environments is an environment variable.
- All secrets come from environment variables or a secret manager.
- `.env` is in `.gitignore`.
- `Settings` class validates types and provides defaults.
- A `.env.example` file documents required variables (without real values).
