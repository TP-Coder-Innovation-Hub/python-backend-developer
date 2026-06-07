# Setup

`[Entry]`

Get Python running on your machine. This takes about 10 minutes.

## Step 1: Install Python

Python 3.12+ is required. Check if you already have it:

```bash
python3 --version
```

If the output shows Python 3.12 or higher, skip to Step 2.

If not, the easiest way to install Python is with **uv** (the modern Python toolchain):

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Then install Python:

```bash
uv python install 3.13
```

Verify:

```bash
uv python list    # Shows installed Python versions
```

## Step 2: Install an Editor

Use **VS Code**. It's free, well-supported, and has excellent Python integration.

1. Download from https://code.visualstudio.com
2. Install the **Python** extension (by Microsoft) and the **Pylance** extension.

These give you:
- Syntax highlighting
- Auto-completion
- Type checking
- Run and debug Python files directly from the editor

## Step 3: Create Your First Project

```bash
# Create a project directory
mkdir my-first-python
cd my-first-python

# Initialize with uv
uv init
```

This creates:
- `pyproject.toml` -- project configuration file
- `hello.py` -- a starter Python file
- `.python-version` -- pins the Python version

## Step 4: Run Your First Program

Open `hello.py` and replace its contents:

```python
print("Hello, world!")
```

Run it:

```bash
uv run hello.py
```

Output:

```
Hello, world!
```

You just ran a Python program.

## Step 5: Try the REPL

Python has an interactive mode called the REPL (Read-Eval-Print Loop). It lets you type code and see results immediately.

```bash
uv run python
```

You'll see a prompt:

```
>>>
```

Type Python code and press Enter:

```python
>>> 2 + 2
4
>>> name = "Ada"
>>> print(name)
Ada
>>> exit()
```

The REPL is useful for experimenting, testing small ideas, and learning. Use it liberally.

## What You Have Now

| Tool | Purpose |
|------|---------|
| Python 3.13 | Runs your code |
| uv | Manages Python versions and packages |
| VS Code | Edits your code with autocompletion and type checking |
| REPL | Experiments and quick tests |

This is your development environment. Everything else in this learning path builds on top of this setup.
