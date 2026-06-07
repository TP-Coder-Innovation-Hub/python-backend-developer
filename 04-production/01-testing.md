# Testing

`[Mid]`

Tests verify that your code works correctly. Without tests, you discover bugs in production. With tests, you discover bugs before they reach users.

## Why Test

- **Catch bugs early.** A test that runs in 1 second can catch a bug that would take hours to debug in production.
- **Fearless refactoring.** Change code with confidence -- if tests pass, nothing broke.
- **Documentation.** Tests show how your code is supposed to behave. Reading tests is often faster than reading documentation.
- **Regression prevention.** A test written for a bug ensures that bug never comes back.

## pytest Basics

pytest is the standard testing framework for Python. Install it:

```bash
uv add --group dev pytest
```

### Testing a Function

Create `calculator.py`:

```python
def add(a, b):
    return a + b

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

Create `test_calculator.py`:

```python
from calculator import add, divide

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0

def test_divide():
    assert divide(10, 2) == 5.0
    assert divide(9, 3) == 3.0

def test_divide_by_zero():
    import pytest
    with pytest.raises(ValueError, match="Cannot divide by zero"):
        divide(10, 0)
```

Run tests:

```bash
uv run pytest
```

Output:

```
test_calculator.py ...
3 passed
```

Step by step:
- `assert add(2, 3) == 5` -- if the assertion is `True`, the test passes. If `False`, it fails.
- `pytest.raises(ValueError)` -- asserts that the code inside the `with` block raises a `ValueError`.
- `match="Cannot divide by zero"` -- also checks that the error message matches.

### Testing an API Endpoint

For FastAPI, use `httpx` with `ASGITransport` to test without starting a real server:

```bash
uv add --group dev httpx pytest-asyncio
```

```python
import pytest
from httpx import AsyncClient, ASGITransport
from main import app

@pytest.fixture
async def client():
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as c:
        yield c

async def test_create_user(client):
    response = await client.post("/users", json={
        "name": "Ada",
        "email": "ada@example.com"
    })
    assert response.status_code == 201
    data = response.json()
    assert data["name"] == "Ada"
    assert data["email"] == "ada@example.com"
    assert "id" in data

async def test_list_users(client):
    response = await client.get("/users")
    assert response.status_code == 200
    assert isinstance(response.json(), list)
```

Step by step:
- `@pytest.fixture` -- creates a test client that's reused across tests. The `yield` provides the client; code after `yield` runs cleanup.
- `ASGITransport(app=app)` -- sends requests directly to your FastAPI app in memory, no network needed.
- `await client.post(...)` -- sends a POST request to your app.
- `assert response.status_code == 201` -- verifies the response status.

## Unit vs Integration Tests

| Type | What it tests | Speed | Example |
|------|--------------|-------|---------|
| Unit | A single function in isolation | Fast (<1ms) | Testing `add(2, 3)` |
| Integration | Multiple components working together | Slower | Testing a full API endpoint with database |

Both are necessary. Write unit tests for complex logic. Write integration tests for API endpoints.

## Testing Strategy

1. **Test the happy path first** -- does it work with valid input?
2. **Test edge cases** -- what happens with empty input? None? Wrong types?
3. **Test error paths** -- does it fail gracefully with invalid input?
4. **Run tests before every commit** -- catch regressions immediately.

```bash
# Run all tests
uv run pytest

# Run with verbose output
uv run pytest -v

# Run a specific test file
uv run pytest test_calculator.py

# Run tests matching a name pattern
uv run pytest -k "test_add"
```
