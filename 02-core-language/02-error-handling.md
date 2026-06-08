# Error Handling

``

Programs fail. Networks drop, files disappear, users type wrong data. Error handling is how your program deals with these failures without crashing.

## The Problem Without Error Handling

```python
result = 10 / 0
# ZeroDivisionError: division by zero
# Program crashes. Nothing after this line runs.
```

Without error handling, any failure terminates the entire program. In a backend server, that means one bad request can bring down the whole service.

## try / except

Wrap risky code in `try`, handle failures in `except`:

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    result = 0
    print("Cannot divide by zero, using 0 instead")

print(result)   # 0
# Program continues running
```

Step by step:
1. `try:` -- attempt the code inside this block.
2. `result = 10 / 0` -- this raises a `ZeroDivisionError`.
3. `except ZeroDivisionError:` -- Python jumps here because the error matches.
4. `result = 0` -- handle the error by setting a safe default.
5. Program continues normally.

If no error occurs, the `except` block is skipped entirely:

```python
try:
    result = 10 / 2
except ZeroDivisionError:
    result = 0

print(result)   # 5 (except block never ran)
```

## Catching Multiple Exceptions

```python
try:
    value = int("not a number")
except ValueError:
    print("That's not a valid number")
except TypeError:
    print("Wrong type provided")
```

Or catch multiple exceptions in one handler:

```python
try:
    value = int(some_input)
except (ValueError, TypeError) as e:
    print(f"Invalid input: {e}")
```

The `as e` captures the exception object so you can inspect its message.

## finally

Code in `finally` runs no matter what -- whether an error occurred or not:

```python
file = open("data.txt")
try:
    content = file.read()
    process(content)
except FileNotFoundError:
    print("File not found")
finally:
    file.close()     # always runs, even if an error occurred
```

Use `finally` for cleanup: closing files, releasing connections, resetting state.

## Raising Exceptions

Use `raise` to signal that something went wrong:

```python
def set_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 150:
        raise ValueError("Age seems unrealistic")
    return age

try:
    set_age(-5)
except ValueError as e:
    print(e)   # Age cannot be negative
```

Raise exceptions when your function receives invalid input. It's better to fail loudly than to silently produce wrong results.

## Custom Exceptions

Create your own exception types for domain-specific errors:

```python
class InsufficientFundsError(Exception):
    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        super().__init__(
            f"Cannot withdraw ${amount}. Balance: ${balance}"
        )

def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientFundsError(balance, amount)
    return balance - amount

try:
    withdraw(50, 100)
except InsufficientFundsError as e:
    print(e)         # Cannot withdraw $100. Balance: $50
    print(e.balance) # 50
```

Custom exceptions carry context. A caller can catch specific error types and handle them appropriately.

## Best Practices

| Practice | Why |
|----------|-----|
| Catch specific exceptions | `except Exception` hides bugs. Catch the error you expect. |
| Don't silence errors | An empty `except: pass` hides problems. Always log or handle. |
| Raise early | Validate inputs at the top of a function, fail fast. |
| Use `finally` for cleanup | Resources (files, connections) must be released. |
| Create custom exceptions | Domain errors are clearer than generic `ValueError`. |
