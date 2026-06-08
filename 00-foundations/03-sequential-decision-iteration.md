# Sequence, Decision, Iteration

``

Every program ever written -- from a one-line script to the entire Google infrastructure -- is built from exactly three constructs:

1. **Sequence** -- do this, then do that
2. **Decision** -- if this is true, do X; otherwise, do Y
3. **Iteration** -- repeat this until a condition is met

That's it. There is no fourth building block. Every programming language, every framework, every design pattern ultimately compiles down to these three things.

## Sequence

Instructions execute in order, top to bottom. The simplest construct.

```python
name = "Ada"
age = 30
print(name)       # prints: Ada
print(age)        # prints: 30
```

The computer executes line 1, then line 2, then line 3, then line 4. No surprises. Sequence is the default -- unless you introduce decision or iteration.

## Decision

Branch based on a condition. The program chooses a path.

```python
age = 17

if age >= 18:                # Check: is 17 >= 18? No.
    print("Adult")
elif age >= 13:              # Check: is 17 >= 13? Yes.
    print("Teenager")        # This line runs.
else:
    print("Child")
```

Step by step:
- `age` is 17.
- `age >= 18` evaluates to `False`. Skip that branch.
- `age >= 13` evaluates to `True`. Enter that branch.
- Print "Teenager".
- The `else` block is skipped because a branch already matched.

Decisions let your program respond differently to different inputs. Without decisions, every run of your program would produce the same result.

## Iteration

Repeat a block of code. Either a fixed number of times (`for`) or until a condition changes (`while`).

```python
# for loop: repeat for each item in a collection
names = ["Ada", "Grace", "Linus"]
for name in names:
    print(name)
# Prints: Ada, then Grace, then Linus

# while loop: repeat until a condition becomes False
count = 3
while count > 0:
    print(count)
    count = count - 1
# Prints: 3, then 2, then 1
```

Iteration is how programs handle work that scales. Instead of writing `print` 100 times, you write a loop.

## Why This Matters

These three constructs are sufficient to solve any computable problem. This isn't an opinion -- it's a proven result from computer science (look up "structured program theorem" if you're curious).

When you encounter a complex problem, break it down:

1. What needs to happen in order? **Sequence.**
2. Where do I need to make choices? **Decision.**
3. Where do I need to repeat work? **Iteration.**

Every function, every class, every API endpoint is just sequence, decision, and iteration combined in specific ways.

## A Complete Example

```python
# Sequence: set up data
scores = [85, 92, 78, 95, 60]
passed = 0

# Iteration: check each score
for score in scores:
    # Decision: is this score passing?
    if score >= 70:
        passed = passed + 1

# Sequence: output the result
print(f"{passed} out of {len(scores)} passed")
```

This program uses all three constructs. Trace through it yourself: the `for` loop iterates 5 times. Inside the loop, the `if` makes a decision for each score. The final `print` runs once in sequence.
