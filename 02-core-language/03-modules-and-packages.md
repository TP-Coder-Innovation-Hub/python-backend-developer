# Modules and Packages

`[Entry]`

A module is a file containing Python code. A package is a directory of modules. They let you organize code into reusable pieces instead of one giant file.

## Why Modules

Imagine a 10,000-line Python file. Finding anything is painful. Editing it means scrolling endlessly. Multiple people can't work on it simultaneously.

Modules solve this: split your code into files, each handling one concern.

## import

Use code from another file:

```python
import math

print(math.sqrt(16))    # 4.0
print(math.pi)          # 3.141592653589793
```

- `import math` -- loads the `math` module (part of Python's standard library).
- `math.sqrt(16)` -- calls the `sqrt` function from the `math` module.

## from ... import

Import specific items:

```python
from math import sqrt, pi

print(sqrt(16))    # 4.0 (no math. prefix needed)
print(pi)          # 3.141592653589793
```

Import with an alias:

```python
from math import sqrt as square_root
print(square_root(16))   # 4.0
```

## Creating Your Own Modules

Any `.py` file is a module. Create `greeting.py`:

```python
def say_hello(name):
    return f"Hello, {name}!"
```

In another file in the same directory:

```python
from greeting import say_hello

print(say_hello("Ada"))   # Hello, Ada!
```

## Packages

A package is a directory with an `__init__.py` file (can be empty). It groups related modules:

```
myapp/
    __init__.py
    users.py
    orders.py
    products.py
```

Import from a package:

```python
from myapp.users import create_user
from myapp.orders import get_order
```

## pip and uv -- Installing Packages

Python has a massive ecosystem of third-party packages. Install them with **uv** (the modern tool) or `pip`:

```bash
# Install a package
uv add requests

# Install a specific version
uv add "fastapi>=0.115"

# Install a dev-only dependency
uv add --group dev pytest
```

This adds the package to your `pyproject.toml` and creates a lockfile (`uv.lock`). The lockfile ensures everyone on your team gets the exact same versions.

## Virtual Environments

A virtual environment is an isolated Python environment for your project. Each project gets its own set of packages, independent of other projects.

Why: Project A needs Django 4.x. Project B needs Django 5.x. Without virtual environments, they conflict. With virtual environments, each has its own isolated setup.

With uv, virtual environments are created automatically:

```bash
# uv creates and manages the venv for you
uv init myproject
cd myproject
uv add fastapi
uv run python app.py     # runs in the project's venv
```

No manual `virtualenv` or `source venv/bin/activate` needed. `uv run` handles it.

## pyproject.toml

The single source of truth for your project's configuration:

```toml
[project]
name = "my-backend"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "fastapi>=0.115",
    "uvicorn[standard]>=0.34",
]

[dependency-groups]
dev = [
    "pytest>=8.0",
    "ruff>=0.9",
]
```

This replaces `setup.py`, `requirements.txt`, `Pipfile`, and all the other legacy formats. One file, one source of truth.

## What to Remember

| Concept | What it does |
|---------|-------------|
| `import` | Load a module |
| `from ... import` | Load specific items from a module |
| Package | Directory of modules with `__init__.py` |
| `uv add` | Install a third-party package |
| Virtual environment | Isolated packages per project |
| `pyproject.toml` | Project configuration and dependencies |
| `uv.lock` | Locked dependency versions for reproducibility |
