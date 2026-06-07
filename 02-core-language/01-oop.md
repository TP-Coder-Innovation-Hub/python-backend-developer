# Object-Oriented Programming

`[Entry]`

OOP organizes code by grouping related data and behavior into objects. An object is a self-contained unit that knows its own state (data) and can perform actions (methods).

## Why OOP

Without OOP, related data and functions drift apart:

```python
# Procedural: data and functions are separate
name = "Ada"
email = "ada@example.com"

def format_user(name, email):
    return f"{name} ({email})"
```

With OOP, data and behavior live together:

```python
# OOP: data and behavior are bundled
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def format(self):
        return f"{self.name} ({self.email})"
```

When your program has entities with relationships (users have orders, orders have items), OOP keeps the code organized.

## Classes and Objects

A **class** is a blueprint. An **object** is a specific instance created from that blueprint.

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name       # each Dog has its own name
        self.breed = breed     # each Dog has its own breed

    def bark(self):
        return f"{self.name} says Woof!"

# Create objects (instances)
rex = Dog("Rex", "German Shepherd")
buddy = Dog("Buddy", "Golden Retriever")

print(rex.name)      # Rex
print(buddy.name)    # Buddy
print(rex.bark())    # Rex says Woof!
```

Step by step:
- `class Dog:` -- defines a new class (blueprint for dogs).
- `__init__` -- the constructor. Runs when you create a new `Dog`. `self` refers to the specific object being created.
- `self.name = name` -- stores the name on the object. Each object gets its own `name`.
- `Dog("Rex", "German Shepherd")` -- creates a new `Dog` object, calls `__init__` with `"Rex"` and `"German Shepherd"`.
- `rex.bark()` -- calls the `bark` method on the `rex` object. `self` inside `bark` refers to `rex`.

## Methods

Methods are functions defined inside a class. They always take `self` as the first parameter, which refers to the object calling the method.

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
        return self.balance

    def withdraw(self, amount):
        if amount > self.balance:
            raise ValueError("Insufficient funds")
        self.balance -= amount
        return self.balance

account = BankAccount("Ada", 100)
account.deposit(50)        # balance: 150
account.withdraw(30)       # balance: 120
```

## Inheritance

A class can inherit from another class, gaining its attributes and methods:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError

class Cat(Animal):           # Cat inherits from Animal
    def speak(self):
        return f"{self.name} says Meow!"

class Dog(Animal):
    def speak(self):
        return f"{self.name} says Woof!"

cat = Cat("Whiskers")
print(cat.speak())           # Whiskers says Meow!
print(isinstance(cat, Animal))  # True
```

`Cat` gets `__init__` from `Animal` automatically. It overrides `speak` with its own implementation.

## Composition

Often better than inheritance: objects contain other objects instead of inheriting from them.

```python
class Engine:
    def __init__(self, horsepower):
        self.horsepower = horsepower

class Car:
    def __init__(self, make, engine):
        self.make = make
        self.engine = engine       # Car HAS an Engine (composition)

engine = Engine(200)
car = Car("Toyota", engine)
print(car.engine.horsepower)       # 200
```

Prefer composition over inheritance. Inheritance creates tight coupling ("a Car IS an Engine" makes no sense). Composition is flexible ("a Car HAS an Engine").

## Key Takeaways

| Concept | What it is |
|---------|-----------|
| Class | Blueprint for creating objects |
| Object | An instance of a class |
| `__init__` | Constructor that sets up initial state |
| `self` | Reference to the current object |
| Method | A function that belongs to a class |
| Inheritance | A class gains features from a parent class |
| Composition | A class contains objects of other classes |
