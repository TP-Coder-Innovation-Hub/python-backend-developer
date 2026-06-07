# Variables and Types

`[Entry]`

Variables store data. Types describe what kind of data. Python figures out the type automatically (dynamic typing), but you still need to understand what's happening.

## Variables

A variable is a name that points to a value. You create one with `=` (assignment):

```python
name = "Ada"       # name now points to the string "Ada"
age = 30           # age now points to the integer 30
height = 1.75      # height now points to the float 1.75
is_student = True  # is_student now points to the boolean True
```

Step by step:
- `name = "Ada"` -- Python creates a string object `"Ada"` in memory, then makes the label `name` point to it.
- `age = 30` -- Python creates an integer object `30`, makes `age` point to it.

You can change what a variable points to:

```python
x = 10
print(x)    # 10
x = 20
print(x)    # 20
```

Variables are labels, not boxes. `x` isn't a container holding 10; it's a label pointing to the object `10`. Reassigning `x = 20` moves the label to a different object.

## Common Types

| Type | What it holds | Example |
|------|--------------|---------|
| `int` | Whole numbers | `42`, `-7`, `0` |
| `float` | Decimal numbers | `3.14`, `-0.5`, `1.0` |
| `str` | Text (string) | `"hello"`, `'world'` |
| `bool` | True or False | `True`, `False` |
| `NoneType` | Nothing / absence of value | `None` |

## Working with Types

```python
# int -- arithmetic works as expected
x = 10
y = 3
print(x + y)   # 13 (addition)
print(x / y)   # 3.3333... (division always returns float)
print(x // y)  # 3 (integer division -- rounds down)
print(x % y)   # 1 (remainder / modulo)
print(x ** y)  # 1000 (exponentiation: 10 to the power of 3)

# str -- strings can be combined (concatenated) and sliced
first = "Ada"
last = "Lovelace"
full = first + " " + last    # "Ada Lovelace"
print(len(full))             # 12 (length of string)
print(full[0])               # "A" (first character, index 0)
print(full[0:3])             # "Ada" (characters from index 0 to 2)

# bool -- used in decisions
is_active = True
is_admin = False
print(is_active and is_admin)  # False (both must be True)
print(is_active or is_admin)   # True (at least one must be True)
print(not is_active)           # False (negation)

# None -- represents "nothing" or "not set"
result = None
print(result)   # None
```

## Dynamic Typing

Python doesn't require you to declare types. The same variable can hold different types:

```python
x = 10          # x is an int
x = "hello"     # now x is a str
x = True        # now x is a bool
```

This is dynamic typing. Python determines the type at runtime based on the value assigned.

You can check a variable's type:

```python
x = 42
print(type(x))        # <class 'int'>
print(isinstance(x, int))  # True
```

## Type Conversion

Convert between types when needed:

```python
# str to int
age_str = "30"
age_int = int(age_str)     # 30 (now you can do math)

# int to str
number = 42
text = str(number)         # "42" (now you can concatenate)

# float to int (truncates, doesn't round)
price = 9.99
whole = int(price)         # 9
```

Conversion fails if the value doesn't make sense:

```python
int("hello")   # ValueError: invalid literal for int()
```

## Common Mistake: None Checks

`None` is not the same as `False`, `0`, or an empty string. But in a boolean context, all of these are "falsy":

```python
x = None
if x:                     # False (None is falsy)
    print("has value")

if x is None:             # True (explicit None check -- preferred)
    print("is None")
```

Always use `is None` or `is not None` to check for `None`, not `==`.
