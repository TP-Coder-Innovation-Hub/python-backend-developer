# Type Hints

`[Mid]`

Python is dynamically typed -- you don't have to declare types. But adding type hints to your code catches bugs, improves IDE support, and serves as documentation.

## Why Type Hints Matter

```python
# Without type hints -- what are the expected types?
def process(order, user, db):
    balance = db.get_balance(user)
    if balance >= order.total:
        db.charge(user, order.total)
```

What type is `order`? What does `get_balance` return? You can't answer these without reading every function called.

```python
# With type hints -- the contract is clear
def process(order: Order, user_id: int, db: Database) -> ChargeResult:
    balance: float = db.get_balance(user_id)
    if balance >= order.total:
        db.charge(user_id, order.total)
```

Every parameter and return type is explicit. Type hints are **executable documentation**.

## Basic Annotations

```python
# Function parameters and return type
def greet(name: str) -> str:
    return f"Hello, {name}"

# Variables (optional, usually inferred)
age: int = 30
price: float = 9.99
active: bool = True
```

## Common Types

```python
from __future__ import annotations

def example(
    names: list[str],           # list of strings
    scores: dict[str, int],     # dict with str keys, int values
    unique_ids: set[int],       # set of integers
    coordinates: tuple[float, float],  # tuple of two floats
) -> str | None:               # returns str or None
    if not names:
        return None
    return names[0]
```

Note: `from __future__ import annotations` enables modern syntax (`str | None` instead of `Optional[str]`, `list[str]` instead of `List[str]`).

## Optional and Union

```python
# Optional: might be None
def find_user(user_id: int) -> User | None:
    if user_id in database:
        return database[user_id]
    return None

# Union: one of several types
def parse_value(raw: str) -> int | float | str:
    try:
        return int(raw)
    except ValueError:
        try:
            return float(raw)
        except ValueError:
            return raw
```

## Type Checking with Pyright

Type hints aren't enforced at runtime. You need a type checker to find type errors. **Pyright** is the standard tool:

```bash
# Install
uv add --group dev pyright

# Run
uv run pyright src/
```

Pyright scans your code and reports type mismatches:

```python
def double(x: int) -> int:
    return x * 2

double("hello")  # Pyright error: Argument of type "str" cannot be assigned to parameter "x" of type "int"
```

## Pydantic Preview

Pydantic uses type hints for **runtime validation**. FastAPI relies on it heavily:

```python
from pydantic import BaseModel, EmailStr

class CreateUserRequest(BaseModel):
    name: str
    email: EmailStr
    age: int

# Validates input and raises clear errors if invalid
user = CreateUserRequest(
    name="Ada",
    email="not-an-email",   # ValidationError: email must be a valid email
    age=-5,                 # you can add validators to reject this
)
```

Pydantic bridges the gap between untrusted external data (JSON from an HTTP request) and your typed internal code. It validates at the boundary so your internal logic can trust the types.

## Best Practices

| Practice | Why |
|----------|-----|
| Add `from __future__ import annotations` | Enables modern syntax in all Python 3.x versions |
| Type all function signatures | Parameters and return types are the most valuable hints |
| Use `str \| None` not `Optional[str]` | Cleaner, modern syntax (3.10+) |
| Use `list[str]` not `List[str]` | No need to import from `typing` (3.9+) |
| Run Pyright in CI | Catch type errors before they reach production |
| Use Pydantic at API boundaries | Validate external data before it enters your system |

Type hints are most valuable at **boundaries** -- function signatures, API endpoints, and public APIs. Internal code can rely more on inference. Focus your annotation effort where it provides the most value.
