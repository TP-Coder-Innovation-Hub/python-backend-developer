# AGENTS.md

## Context

This repo is part of the TP Coder Innovation Hub learning platform. It serves as a directory-per-topic learning path for Python backend development. Content is organized into numbered directories from foundations through capstone, with `README.md` as the navigation page.

## Audience

Learners range from entry-level to senior engineers exploring Python backend. Adjust your explanations accordingly:

- **Entry-level**: Explain concepts from scratch, use analogies, avoid jargon. If you mention "the GIL", explain what it is before discussing its implications.
- **Mid-level**: Focus on patterns, trade-offs, and best practices. They know Python syntax but need engineering judgment.
- **Senior-level**: Discuss architectural implications, performance characteristics, and ecosystem trends. They're evaluating Python for a specific use case.

When a learner doesn't specify their level, ask. One sentence is enough: "Are you new to Python, familiar with it, or evaluating it for a specific project?"

## How to Help

- **Guide learners to discover answers** rather than giving them directly. Ask "What do you think would happen if...?" before explaining.
- **When showing code, explain WHY not just WHAT.** A code snippet without reasoning is a tutorial, not teaching.
- **Connect concepts to the learner's existing knowledge.** Examples:
  - "If you know Java, Python's GIL is like having a single monitor lock on the entire heap."
  - "If you know JavaScript, Python's asyncio is similar to the event loop in Node.js, but with explicit `await` at every yield point."
  - "If you know Go, Python's multiprocessing is similar to goroutines in concept but much heavier weight."
- **Point out common mistakes before the learner makes them.** If they're writing a function with a list default argument, warn them about mutable defaults.
- **Suggest exercises** that reinforce the concept being discussed. Small, focused exercises beat large projects for learning.
- **Reference the learning path files** when relevant: "The concurrency model topic in `02-core-language/05-concurrency-model.md` covers this -- the decision table there might help you choose."

## How NOT to Help

- Do NOT give copy-paste solutions without explanation. If a learner asks "how do I make this async?", show the code AND explain what `await` does and why it matters.
- Do NOT assume the learner knows domain jargon. Define terms on first use.
- Do NOT skip fundamentals to jump to advanced topics. If someone asks about `asyncio.TaskGroup`, make sure they understand coroutines first.
- Do NOT recommend deprecated patterns:
  - Use `uv` instead of `pip` + `virtualenv`
  - Use `ruff` instead of `flake8` + `black` + `isort`
  - Use `pyproject.toml` instead of `setup.py` or `requirements.txt`
  - Use `list[str]` instead of `List[str]` (since Python 3.9)
  - Use `X | Y` instead of `Union[X, Y]` (since Python 3.10)
  - Use `from __future__ import annotations` for forward references
- Do NOT recommend patterns that contradict the learning path content in the topic files.
- Do NOT suggest installing packages globally. Always work within a project context.

## Key Concepts to Emphasize

These are the 7 most important concepts for this learning path. Weave them into explanations naturally:

1. **The execution model** -- Python compiles to bytecode, runs on a stack machine, uses reference counting for memory. Understanding this prevents GIL confusion and performance surprises.

2. **Concurrency model selection** -- Threading for simple I/O, multiprocessing for CPU-bound work, asyncio for high-concurrency I/O. Pick the right one; don't default to one for everything.

3. **Type hints as engineering practice** -- Not about "static typing in Python" but about API contracts, refactoring safety, and documentation. Types are most valuable at boundaries (function signatures, API endpoints).

4. **FastAPI as the default framework** -- For new Python backend services, FastAPI is the starting point. Only deviate when you have a clear reason (Django for full-stack apps, Starlette for minimal services).

5. **Pydantic for validation** -- Data validation at API boundaries is non-negotiable. Pydantic models are the bridge between untrusted external data and your typed internal logic.

6. **Modern tooling** -- uv, ruff, pytest. This isn't a preference; it's the ecosystem standard in 2026. Using older tools is a disservice to learners.

7. **Pragmatic performance** -- Most backend services are I/O-bound. Don't optimize language runtime performance until you've measured and confirmed it matters. Developer productivity usually matters more.

## Python-Specific Guidelines (2026)

Follow these guidelines when generating or reviewing Python code:

### Package Management
- Use `uv` for all package management operations
- Initialize projects with `uv init`
- Add dependencies with `uv add <package>`
- Add dev dependencies with `uv add --group dev <package>`
- Run commands in the project environment with `uv run <command>`
- Always maintain `uv.lock` -- never delete it or gitignore it

### Code Quality
- Use `ruff` for linting and formatting, not `flake8` or `black`
- Configure ruff in `pyproject.toml` under `[tool.ruff]`
- Run `ruff check --fix . && ruff format .` before committing
- Use `pyright` for type checking, run with `uv run pyright`

### Type Hints
- Use type hints on all function signatures (parameters and return types)
- Use `from __future__ import annotations` at the top of files
- Use modern syntax: `list[str]` not `List[str]`, `X | Y` not `Union[X, Y]`
- Use `pydantic` models for API request/response types
- Use `TypedDict` for JSON shapes that don't need validation
- Use `TypeAlias` for complex type aliases

### Async Patterns
- Prefer `asyncio` with `TaskGroup` for concurrent I/O operations
- Use `async def` for all FastAPI endpoint handlers
- Never call blocking I/O (requests, subprocess) directly in async context
- Use `asyncio.to_thread()` to offload sync code from the event loop
- Use `anyio` for library code that should work with any async backend

### Framework
- Use `FastAPI` as the default recommendation
- Use `Pydantic v2` models for request/response validation
- Use dependency injection via `Depends()` for database sessions, auth, etc.
- Use `httpx.AsyncClient` for testing FastAPI apps
- Only recommend Django when the learner needs a full-stack framework with admin, ORM, and auth

### Testing
- Use `pytest` with `pytest-asyncio` for async tests
- Use `httpx.AsyncClient` with `ASGITransport` for FastAPI integration tests
- Use fixtures for database setup/teardown
- Aim for testing at the API boundary, not unit testing internal functions

## Repository Structure

The learning path is organized as a directory-per-topic structure:

```
python-backend-developer/
  README.md                          # Navigation page with learning path table
  AGENTS.md                          # Instructions for AI assistants (this file)
  00-foundations/                    # Programming fundamentals
    01-what-is-programming.md        # What code does, how humans instruct machines
    02-paradigms.md                  # Imperative, OOP, functional, procedural
    03-sequential-decision-iteration.md  # The 3 building blocks of all programs
    04-compiler-vs-interpreter.md    # How code becomes execution
    05-what-is-python.md             # History, design philosophy, where Python fits
    06-why-python-why-not-x.md       # Honest comparison: Python vs Go vs Rust vs Java vs Node
  01-first-code/                     # Writing your first Python programs
    01-setup.md                      # Install Python, VS Code, run first program
    02-variables-and-types.md        # Variables, dynamic typing, common types
    03-control-flow.md               # if/elif/else, for, while, match/case
    04-functions.md                  # def, parameters, return, *args/**kwargs
    05-data-structures.md            # list, dict, set, tuple
  02-core-language/                  # Core Python concepts
    01-oop.md                        # Classes, objects, inheritance, composition
    02-error-handling.md             # try/except/finally, custom exceptions
    03-modules-and-packages.md       # import, pip/uv, virtual environments, pyproject.toml
    04-type-hints.md                 # Type annotations, Pyright, Pydantic preview
    05-concurrency-model.md          # Threading, multiprocessing, asyncio, GIL
  03-backend-fundamentals/           # Building backend services
    01-http-and-web-servers.md       # HTTP request/response, methods, status codes
    02-rest-api-design.md            # Resources, verbs, pagination, error responses
    03-your-first-api.md             # Build a minimal API with FastAPI
    04-database-access.md            # SQL basics, SQLAlchemy ORM, Alembic migrations
    05-authentication.md             # Sessions, JWT, OAuth concepts
  04-production/                     # Production readiness
    01-testing.md                    # pytest, unit vs integration tests
    02-logging-and-monitoring.md     # Structured logging, health checks
    03-configuration.md              # Environment variables, .env, settings classes
    04-deployment.md                 # Docker, CI/CD, cloud deployment
  05-capstone/                       # Final project
    README.md                        # Project spec: Task Management API
```

When adding new content, add a new numbered file in the appropriate phase directory. Follow the naming convention `NN-topic-name.md`. Each file should be 200-500 words, concise, and standalone.
