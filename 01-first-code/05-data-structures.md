# Data Structures

`[Entry]`

Data structures are containers for organizing data. Python has four built-in structures you'll use constantly.

## list -- Ordered, Mutable Sequence

A list holds items in order. You can add, remove, and change items.

```python
fruits = ["apple", "banana", "cherry"]

# Access by index (starts at 0)
print(fruits[0])       # apple
print(fruits[-1])      # cherry (negative index counts from end)

# Add to end
fruits.append("date")
print(fruits)          # ["apple", "banana", "cherry", "date"]

# Insert at position
fruits.insert(1, "blueberry")
print(fruits)          # ["apple", "blueberry", "banana", "cherry", "date"]

# Remove by value
fruits.remove("banana")
print(fruits)          # ["apple", "blueberry", "cherry", "date"]

# Remove by index
removed = fruits.pop(0)
print(removed)         # apple
print(fruits)          # ["blueberry", "cherry", "date"]

# Length
print(len(fruits))     # 3

# Check membership
print("cherry" in fruits)   # True
```

Use lists when: order matters, you need to add/remove items, or you're iterating over a collection.

## dict -- Key-Value Mapping

A dictionary maps keys to values. Look up a value by its key.

```python
user = {"name": "Ada", "age": 30, "role": "engineer"}

# Access by key
print(user["name"])          # Ada

# Add or update
user["email"] = "ada@example.com"
user["age"] = 31
print(user)
# {"name": "Ada", "age": 31, "role": "engineer", "email": "ada@example.com"}

# Safe access with default (avoids KeyError if key doesn't exist)
print(user.get("phone", "N/A"))   # N/A

# Remove a key
del user["role"]

# Check if key exists
print("name" in user)       # True

# Iterate over keys and values
for key, value in user.items():
    print(f"{key}: {value}")
```

Use dicts when: you need to look up values by a key, represent structured data, or count occurrences.

## set -- Unordered Collection of Unique Items

A set holds unique items. Duplicates are automatically removed.

```python
colors = {"red", "green", "blue"}

# Add
colors.add("yellow")
colors.add("red")       # no effect -- already exists
print(colors)           # {"red", "green", "blue", "yellow"}

# Remove
colors.discard("green")
print(colors)           # {"red", "blue", "yellow"}

# Set operations
a = {1, 2, 3}
b = {3, 4, 5}
print(a | b)            # {1, 2, 3, 4, 5} (union -- all unique items)
print(a & b)            # {3} (intersection -- items in both)
print(a - b)            # {1, 2} (difference -- in a but not in b)
```

Use sets when: you need uniqueness, membership testing (`x in my_set` is O(1)), or set math operations.

## tuple -- Ordered, Immutable Sequence

A tuple is like a list but cannot be changed after creation.

```python
coordinates = (10, 20)

# Access by index
print(coordinates[0])   # 10

# Cannot modify -- this would raise an error:
# coordinates[0] = 15   # TypeError: 'tuple' object does not support item assignment

# Unpacking
x, y = coordinates
print(x)                # 10
print(y)                # 20
```

Use tuples when: data should not change (immutable), returning multiple values from a function, or using as dictionary keys (lists can't be keys, tuples can).

## When to Use Each

| Structure | Ordered | Mutable | Use when |
|-----------|---------|---------|----------|
| `list` | Yes | Yes | Ordered collection that changes |
| `dict` | Yes (insertion order, 3.7+) | Yes | Key-value lookups |
| `set` | No | Yes | Uniqueness, membership testing |
| `tuple` | Yes | No | Fixed data that won't change |

## Common Operations

```python
# Sort a list
numbers = [3, 1, 4, 1, 5, 9]
numbers.sort()            # sorts in place: [1, 1, 3, 4, 5, 9]
sorted_numbers = sorted(numbers)  # returns a new sorted list

# List comprehension (concise way to create lists)
squares = [x * x for x in range(5)]
print(squares)            # [0, 1, 4, 9, 16]

# Dict comprehension
word_lengths = {word: len(word) for word in ["hello", "world"]}
print(word_lengths)       # {"hello": 5, "world": 5}

# Merge dicts (Python 3.9+)
defaults = {"color": "blue", "size": "M"}
overrides = {"size": "L"}
merged = defaults | overrides
print(merged)             # {"color": "blue", "size": "L"}
```
