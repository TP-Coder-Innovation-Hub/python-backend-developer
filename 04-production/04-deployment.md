# Deployment

`[Mid]`

Deployment is how your code gets from your machine to a server where users can access it. Docker and CI/CD are the standard approach.

## Docker Basics for Python

Docker packages your application and all its dependencies into a container -- a lightweight, portable unit that runs the same everywhere.

### Dockerfile

A Dockerfile describes how to build your container image:

```dockerfile
FROM python:3.13-slim

WORKDIR /app

COPY pyproject.toml uv.lock ./

RUN pip install uv && uv sync --frozen --no-dev

COPY . .

EXPOSE 8000

CMD ["uv", "run", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Line by line:

| Line | What it does |
|------|-------------|
| `FROM python:3.13-slim` | Start with a minimal Python 3.13 image |
| `WORKDIR /app` | Set the working directory inside the container |
| `COPY pyproject.toml uv.lock ./` | Copy dependency files first (for caching) |
| `RUN pip install uv && uv sync --frozen --no-dev` | Install dependencies (no dev dependencies) |
| `COPY . .` | Copy the rest of your source code |
| `EXPOSE 8000` | Document that the container uses port 8000 |
| `CMD [...]` | The command that runs when the container starts |

The dependency copy comes before the source copy. This is intentional -- Docker caches layers. If only your source code changed (not dependencies), Docker reuses the cached dependency installation layer. This makes builds much faster.

### Build and Run

```bash
# Build the image
docker build -t my-api .

# Run the container
docker run -p 8000:8000 --env-file .env my-api
```

- `-t my-api` -- tags the image with the name `my-api`.
- `-p 8000:8000` -- maps port 8000 on your machine to port 8000 in the container.
- `--env-file .env` -- passes environment variables to the container.

### .dockerignore

Prevent unnecessary files from entering the container:

```
.git
__pycache__
.env
.venv
*.pyc
.pytest_cache
```

## CI/CD Concept

CI/CD automates the build-test-deploy cycle:

```
Push code  -->  CI: Run tests  -->  CD: Build image  -->  CD: Deploy
                  (automated)       (automated)         (automated)
```

- **CI (Continuous Integration):** Every push triggers automated tests. If tests fail, the push is rejected. This prevents broken code from reaching the main branch.
- **CD (Continuous Deployment):** After tests pass, automatically build a Docker image and deploy it to your infrastructure.

A simple GitHub Actions workflow:

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v4
      - run: uv sync --frozen
      - run: uv run pytest
      - run: uv run ruff check .

  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: docker build -t my-api .
      # Push to registry and deploy (cloud-specific steps here)
```

## Where to Deploy

| Platform | Best for | Complexity |
|----------|---------|------------|
| Railway / Render | Small projects, quick deploys | Low |
| AWS (ECS, App Runner) | Production workloads on AWS | Medium-High |
| Google Cloud Run | Serverless containers on GCP | Medium |
| Azure Container Apps | Serverless containers on Azure | Medium |
| Fly.io | Global deployment, low cost | Medium |
| Kubernetes | Large-scale, complex infrastructure | High |

For a first deployment, start with Railway or Render. They connect to your GitHub repo and deploy automatically on every push. Move to AWS/GCP when you need more control.

## Production Checklist

| Item | Why |
|------|-----|
| Docker image builds and runs locally | Catch issues before deploying |
| Tests pass in CI | Never deploy broken code |
| Environment variables configured | Different values per environment |
| Health check endpoint | Monitoring can detect failures |
| Logging configured | You can debug production issues |
| HTTPS enabled | Encrypt traffic (most platforms do this automatically) |
| No secrets in code or Docker image | Use environment variables or secret managers |
