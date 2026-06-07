# Control Flow

`[Entry]`

Control flow determines which code runs and how many times. Without it, every program would execute top-to-bottom, every time. Control flow lets your program make decisions and repeat work.

## if / elif / else

Make decisions based on conditions.

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"

print(grade)   # B
```

Step by step:
1. `score >= 90` -- is 85 >= 90? No. Skip.
2. `score >= 80` -- is 85 >= 80? Yes. Set `grade = "B"`. Skip the rest.

Python checks conditions top-to-bottom and runs the **first** matching branch. Once a branch matches, all others are skipped.

You can omit `elif` and `else` if you don't need them:

```python
if score >= 70:
    print("Passed")
# No else needed if there's nothing to do when the condition is False
```

## Comparison Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `==` | Equal to | `x == 10` |
| `!=` | Not equal to | `x != 10` |
| `>` | Greater than | `x > 10` |
| `<` | Less than | `x < 10` |
| `>=` | Greater than or equal | `x >= 10` |
| `<=` | Less than or equal | `x <= 10` |
| `in` | Contained in collection | `"a" in "abc"` |
| `is` | Same object (identity) | `x is None` |

Combine conditions with `and`, `or`, `not`:

```python
age = 25
has_id = True

if age >= 18 and has_id:
    print("Allowed")
```

## for Loop

Iterate over a sequence of items.

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(fruit)
# apple
# banana
# cherry
```

The loop runs once for each item. `fruit` takes the value of each item in turn.

Common pattern: iterate with an index using `enumerate`:

```python
fruits = ["apple", "banana", "cherry"]

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
# 0: apple
# 1: banana
# 2: cherry
```

Range-based loops:

```python
for i in range(5):
    print(i)
# 0, 1, 2, 3, 4
```

`range(5)` produces numbers 0 through 4. `range(1, 6)` produces 1 through 5.

## while Loop

Repeat as long as a condition is true.

```python
count = 3
while count > 0:
    print(count)
    count = count - 1
print("Go!")
# 3, 2, 1, Go!
```

The condition (`count > 0`) is checked **before** each iteration. If it's `False` from the start, the loop body never runs.

Warning: if the condition never becomes `False`, the loop runs forever (infinite loop). Always ensure the condition will eventually change.

## break and continue

Control what happens inside a loop:

```python
# break: exit the loop entirely
for number in range(10):
    if number == 5:
        break        # stops the loop
    print(number)
# prints 0, 1, 2, 3, 4

# continue: skip to the next iteration
for number in range(5):
    if number == 2:
        continue     # skips printing 2
    print(number)
# prints 0, 1, 3, 4
```

## match / case (Python 3.10+)

Structural pattern matching -- a more powerful alternative to long `if/elif` chains:

```python
command = "start"

match command:
    case "start":
        print("Starting...")
    case "stop":
        print("Stopping...")
    case "pause":
        print("Pausing...")
    case _:
        print("Unknown command")
```

The `_` pattern matches anything (like `else` in an `if` chain). `match/case` also supports destructuring, which makes it powerful for complex data.
