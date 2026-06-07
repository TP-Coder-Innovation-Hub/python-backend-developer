# Programming Paradigms

`[Entry]`

A paradigm is a way of thinking about how to organize code. Different paradigms give you different tools for structuring your instructions.

## The Four Main Paradigms

### Imperative

Tell the computer exactly what to do, step by step. Like giving someone turn-by-turn driving directions.

```
set total to 0
for each number in the list:
    add number to total
print total
```

Most code you write is imperative, regardless of language.

### Procedural

Organize imperative code into named blocks (procedures/functions). Like a recipe with sections: "Prep the vegetables", "Make the sauce", "Cook the pasta".

```
function calculate_total(numbers):
    total = 0
    for each number in numbers:
        total = total + number
    return total

result = calculate_total([1, 2, 3])
```

Procedural programming is imperative code split into reusable pieces. It's the baseline for most languages.

### Object-Oriented (OOP)

Organize code around "objects" that combine data and behavior. Like modeling a restaurant with Chef, Kitchen, and Menu objects that interact.

```
class Chef:
    name = "Ada"
    
    function cook(dish):
        return "Cooked " + dish

chef = new Chef()
meal = chef.cook("pasta")
```

OOP shines when your program models real-world entities with relationships: users have orders, orders have items, items have prices.

### Functional

Treat computation as evaluating mathematical functions. No shared state, no mutation. Like a spreadsheet -- each cell is a formula based on other cells.

```
function calculate_total(numbers):
    return sum(numbers)

result = calculate_total([1, 2, 3])
```

Functional programming emphasizes pure functions (same input always gives same output) and avoiding side effects. It's excellent for data transformation and parallel processing.

## Where Python Fits

Python is multi-paradigm. You can write procedural code, object-oriented code, or functional-style code -- sometimes all in the same file.

```python
# Procedural
total = sum([1, 2, 3])

# OOP
class Calculator:
    def add(self, a, b):
        return a + b

# Functional-style (using built-in functions)
squares = list(map(lambda x: x * x, [1, 2, 3]))
```

In practice, most Python backend code is a mix of procedural and OOP, with some functional patterns for data handling. You don't need to pick one. You need to understand what each is good for so you can choose the right tool for each problem.

## Comparison

| Paradigm | Organizes around | Best for |
|----------|-----------------|----------|
| Imperative | Steps | Simple scripts, algorithms |
| Procedural | Functions | Utility code, small programs |
| OOP | Objects (data + behavior) | Domain models, large applications |
| Functional | Pure functions and data flow | Data pipelines, transformations |
