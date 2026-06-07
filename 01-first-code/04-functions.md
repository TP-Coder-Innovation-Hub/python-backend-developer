# Functions

`[Entry]`

A function is a reusable block of code with a name. You define it once, call it anywhere.

## Why Functions

Without functions, you'd copy-paste the same code everywhere. With functions, you write logic once and reuse it. This means:

- **Less repetition** -- define once, use many times.
- **Easier to change** -- fix a bug in one place, it's fixed everywhere.
- **Easier to understand** -- a good function name explains what the code does.

## Defining a Function

```python
def greet(name):
    return f"Hello, {name}!"
```

Step by step:
- `def` -- keyword to define a function.
- `greet` -- the function's name. Choose descriptive names.
- `(name)` -- the parameter. This is the input the function accepts.
- `return` -- the output the function produces. Without `return`, the function returns `None`.

## Calling a Function

```python
message = greet("Ada")
print(message)   # Hello, Ada!
```

- `greet("Ada")` calls the function, passing `"Ada"` as the argument.
- The function returns `"Hello, Ada!"`.
- That return value is stored in `message`.

## Parameters and Arguments

Parameters are the variables in the function definition. Arguments are the actual values you pass when calling.

```python
# Single parameter
def double(x):
    return x * 2

print(double(5))   # 10

# Multiple parameters
def add(a, b):
    return a + b

print(add(3, 4))   # 7
```

## Default Values

Give a parameter a default value. If the caller doesn't provide it, the default is used.

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet("Ada"))              # Hello, Ada!
print(greet("Ada", "Hi there"))  # Hi there, Ada!
```

- First call: `greeting` uses the default `"Hello"`.
- Second call: `greeting` is explicitly `"Hi there"`.

Parameters with defaults must come **after** parameters without defaults.

## Return Values

A function can return any type. It can also return nothing (implicitly returns `None`).

```python
def is_adult(age):
    return age >= 18          # returns True or False

def print_name(name):
    print(name)               # no return statement -> returns None
```

A function stops executing when it hits `return`:

```python
def find_first_positive(numbers):
    for n in numbers:
        if n > 0:
            return n          # exits immediately
    return None               # only reached if no positive found
```

## *args and **kwargs

Accept any number of positional or keyword arguments:

```python
# *args: any number of positional arguments
def total(*numbers):
    return sum(numbers)

print(total(1, 2, 3))       # 6
print(total(1, 2, 3, 4, 5)) # 15

# **kwargs: any number of keyword arguments
def build_profile(**info):
    return info

print(build_profile(name="Ada", age=30))
# {'name': 'Ada', 'age': 30}
```

- `*numbers` collects all positional arguments into a tuple.
- `**info` collects all keyword arguments into a dictionary.

## Step-by-Step Example

Build a function that formats a full name:

```python
def format_name(first, last, middle=""):
    if middle:
        return f"{first} {middle} {last}"
    return f"{first} {last}"

# Usage
print(format_name("Ada", "Lovelace"))                  # Ada Lovelace
print(format_name("Ada", "Lovelace", "Augusta"))       # Ada Augusta Lovelace
print(format_name(first="Grace", last="Hopper"))       # Grace Hopper
```

- Two required parameters: `first`, `last`.
- One optional parameter: `middle` (defaults to empty string).
- If `middle` is truthy (non-empty), include it. Otherwise, omit it.
- You can call it with positional or keyword arguments.
