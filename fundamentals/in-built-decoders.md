# Python Built-in Decorators Guide

This document explains 10 widely used built-in decorators in Python with practical examples, use cases, and guidance on when to choose each one.

---

## What is a decorator?

A decorator is a callable that takes another function or class and returns a modified version of it. In Python, the `@decorator_name` syntax is just a clean way to apply that transformation.

Example:

```python
@my_decorator
def greet():
    print("Hello")
```

This is roughly equivalent to:

```python
def greet():
    print("Hello")

greet = my_decorator(greet)
```

Decorators are useful because they let you add behavior without rewriting the original function body.

---

# 1) `@property`

## What it does

`@property` turns a method into a managed attribute. You access it like a normal attribute, but behind the scenes Python runs a method.

This is often used when:

* a value is computed dynamically,
* you want to hide internal implementation,
* you want to preserve a clean attribute-like API,
* you want to add validation logic later without changing how callers use the class.

## Why it matters

Without `@property`, you would need to call a method:

```python
player.chips()
```

With `@property`, you can write:

```python
player.chips
```

That feels natural and keeps the class interface simple.

## Example

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    @property
    def area(self):
        return self.width * self.height
```

Usage:

```python
rect = Rectangle(10, 20)
print(rect.area)  # 200
```

## Best use cases

* computed values like `area`, `full_name`, `balance`
* read-only public API
* backward-compatible refactoring from a plain attribute to a computed value
* controlled access to internal state

## Real-world example

Imagine a `User` class:

```python
class User:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

    @property
    def full_name(self):
        return f"{self.first_name} {self.last_name}"
```

Usage:

```python
user = User("Omkar", "Patil")
print(user.full_name)  # Omkar Patil
```

This is better than storing `full_name` manually because it always stays in sync with `first_name` and `last_name`.

## Important note

If you define a property with only a getter, it is read-only from the public API:

```python
user.full_name = "Something"
```

will raise an error unless you also define a setter.

---

# 2) `@property` with `@setter`

## What it does

The setter lets you control what happens when someone assigns to a property.

## Why it matters

This is how you add validation when a value is changed.

## Example

```python
class Player:
    def __init__(self, chips):
        self._chips = chips

    @property
    def chips(self):
        return self._chips

    @chips.setter
    def chips(self, value):
        if not isinstance(value, int):
            raise TypeError("chips must be an integer")
        if value < 0:
            raise ValueError("chips cannot be negative")
        self._chips = value
```

Usage:

```python
player = Player(100)
player.chips = 250   # valid
player.chips = -10   # ValueError
```

## Best use cases

* validation on assignment
* normalizing values
* enforcing business rules
* keeping internal and external APIs clean

## Example use case

For a banking app:

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    @balance.setter
    def balance(self, value):
        if value < 0:
            raise ValueError("Balance cannot be negative")
        self._balance = value
```

This lets you protect the object from invalid state.

## Important limitation

A setter only runs when users assign through the property name:

```python
player.chips = 100
```

If someone directly changes the internal variable:

```python
player._chips = -999
```

that bypasses validation. In Python, the underscore is a convention, not strict enforcement.

---

# 3) `@staticmethod`

## What it does

A static method belongs to the class namespace, but it does not automatically receive `self` or `cls`.

It is just a function placed inside the class for organization.

## Why it matters

Use it when the logic is related to the class, but does not need object state or class state.

## Example

```python
class Card:
    @staticmethod
    def is_valid_suit(suit):
        return suit in ["Hearts", "Diamonds", "Clubs", "Spades"]
```

Usage:

```python
print(Card.is_valid_suit("Hearts"))  # True
print(Card.is_valid_suit("Stars"))   # False
```

## Best use cases

* input validation helpers
* formatting helpers
* utility functions closely tied to the class conceptually
* small helper logic that does not touch instance or class data

## Example use case

A `MathUtils` or `StringUtils` style class:

```python
class StringUtils:
    @staticmethod
    def is_blank(text):
        return text is None or text.strip() == ""
```

This groups related helper logic together without requiring an object.

## Why not always use a regular function?

You could write this outside the class too. `@staticmethod` is useful when the function belongs conceptually to the class domain and improves organization.

## When to avoid it

Do not use `@staticmethod` just to avoid thinking about design. If the method really has nothing to do with the class, a module-level function may be cleaner.

---

# 4) `@classmethod`

## What it does

A class method receives the class as the first argument, usually named `cls`.

It can read or modify class-level state and is often used as an alternate constructor.

## Why it matters

It becomes useful when behavior should depend on the class, not on a single instance.

## Example

```python
class Card:
    suits = ["Hearts", "Diamonds", "Clubs", "Spades"]

    @classmethod
    def is_valid_suit(cls, suit):
        return suit in cls.suits
```

Usage:

```python
print(Card.is_valid_suit("Hearts"))  # True
```

## Best use cases

* alternate constructors
* class-level configuration
* factory methods
* behavior that must work with inheritance

## Example: alternate constructor

```python
class Employee:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_string(cls, text):
        name, age = text.split(",")
        return cls(name.strip(), int(age.strip()))
```

Usage:

```python
emp = Employee.from_string("Omkar, 28")
print(emp.name)  # Omkar
print(emp.age)   # 28
```

## Why `@classmethod` is better than `@staticmethod` here

Because `cls` allows the method to create instances of the current class. If a subclass inherits this method, it still creates the subclass correctly.

## Inheritance benefit

```python
class Employee:
    def __init__(self, name):
        self.name = name

    @classmethod
    def from_name(cls, name):
        return cls(name)

class Manager(Employee):
    pass

manager = Manager.from_name("Alice")
print(type(manager))  # <class '__main__.Manager'>
```

This is one of the strongest reasons to prefer `@classmethod` over `@staticmethod` when the method needs to create or depend on the class.

---

# 5) `@abstractmethod`

## What it does

`@abstractmethod` marks a method that must be implemented by subclasses.

It is used with the `abc` module, usually inside an abstract base class.

## Why it matters

It helps define a contract. You say, “any subclass must provide this method.”

## Example

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def speak(self):
        pass
```

Now any subclass must implement `speak()`:

```python
class Dog(Animal):
    def speak(self):
        return "Woof"
```

## Best use cases

* plugin architectures
* frameworks
* base interfaces
* enforcing common behavior across subclasses

## Example use case

```python
from abc import ABC, abstractmethod

class PaymentGateway(ABC):
    @abstractmethod
    def charge(self, amount):
        pass

class StripeGateway(PaymentGateway):
    def charge(self, amount):
        return f"Charging {amount} via Stripe"
```

This is helpful when different implementations must follow the same shape.

## Important behavior

You cannot instantiate an abstract class if it still has unimplemented abstract methods.

```python
animal = Animal()  # TypeError
```

That is useful because it prevents incomplete objects from being created.

---

# 6) `@dataclass`

## What it does

`@dataclass` automatically generates common methods for classes that mainly store data.

It can generate:

* `__init__`
* `__repr__`
* `__eq__`
* optional ordering methods
* default handling for fields

## Why it matters

It reduces repetitive boilerplate.

## Example

```python
from dataclasses import dataclass

@dataclass
class Card:
    suit: str
    value: str
```

This is roughly similar to writing:

```python
class Card:
    def __init__(self, suit, value):
        self.suit = suit
        self.value = value
```

plus useful extras like readable printing and equality.

## Best use cases

* DTOs
* configuration objects
* simple domain models
* response structures
* classes that are mostly data containers

## Example use case

```python
from dataclasses import dataclass

@dataclass
class Player:
    name: str
    chips: int
```

Usage:

```python
p1 = Player("Omkar", 1000)
p2 = Player("Omkar", 1000)

print(p1)        # Player(name='Omkar', chips=1000)
print(p1 == p2)  # True
```

## Why it is useful

You get cleaner code and less chance of bugs caused by hand-written boilerplate.

## Nice features

You can also define defaults:

```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    is_active: bool = True
```

## Important note

`@dataclass` is not a full replacement for all class design. It is best when your class mostly holds data rather than complex behavior.

---

# 7) `@cached_property`

## What it does

`@cached_property` computes a value once and stores it on the instance so later accesses reuse the same result.

It is useful for expensive computations that should not run repeatedly.

## Why it matters

Some values are derived from object state but are expensive to calculate.

## Example

```python
from functools import cached_property

class Deck:
    def __init__(self, cards):
        self.cards = cards

    @cached_property
    def card_count(self):
        print("Calculating card count...")
        return len(self.cards)
```

Usage:

```python
deck = Deck([1, 2, 3])
print(deck.card_count)  # Calculates
print(deck.card_count)  # Uses cached value
```

## Best use cases

* expensive computations
* derived values that rarely change
* parsing operations
* lazy loading of internal data

## Example use case

```python
from functools import cached_property

class Report:
    def __init__(self, raw_data):
        self.raw_data = raw_data

    @cached_property
    def parsed(self):
        # Imagine this is expensive
        return [item.strip().lower() for item in self.raw_data.split(",")]
```

## When not to use it

If the value must always reflect the latest internal state, caching may be wrong. In that case, use `@property` instead.

## Important detail

If you mutate the underlying data after the cached property is computed, the cached value may become stale unless you clear or redesign the cache.

---

# 8) `@lru_cache`

## What it does

`@lru_cache` caches function results using a Least Recently Used strategy.

If the function is called again with the same arguments, Python returns the cached result instead of recomputing it.

## Why it matters

This can massively improve performance for expensive pure functions.

## Example

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)
```

Usage:

```python
print(fib(10))
```

## Best use cases

* recursive functions
* pure functions
* expensive repeated calculations
* repeated lookups with stable inputs

## Example use case

```python
from functools import lru_cache

@lru_cache(maxsize=1000)
def normalize_username(username):
    return username.strip().lower()
```

This can be useful when the same input appears many times.

## Important rule

`@lru_cache` works best for functions whose output depends only on their inputs.

Avoid using it for functions that depend on:

* time
* random values
* external state
* database state that may change

## Useful methods

`lru_cache`-wrapped functions provide:

```python
fib.cache_info()
fib.cache_clear()
```

These help with debugging and performance tuning.

---

# 9) `@singledispatch`

## What it does

`@singledispatch` lets you write a generic function and register specialized implementations based on the type of the first argument.

## Why it matters

It gives a clean way to simulate function overloading in Python.

## Example

```python
from functools import singledispatch

@singledispatch
def show(value):
    print(f"Default: {value}")

@show.register
def _(value: int):
    print(f"Integer: {value}")

@show.register
def _(value: str):
    print(f"String: {value}")
```

Usage:

```python
show(10)      # Integer: 10
show("abc")   # String: abc
show(3.14)    # Default: 3.14
```

## Best use cases

* type-based behavior selection
* serialization/deserialization helpers
* formatting functions
* data transformation pipelines

## Example use case

```python
from functools import singledispatch

@singledispatch
def serialize(value):
    return str(value)

@serialize.register
def _(value: dict):
    return "{dict serialization}"

@serialize.register
def _(value: list):
    return "[list serialization]"
```

## Important limitation

`singledispatch` dispatches only on the first argument’s type. If you need more complex type-based behavior, you may need a different design.

---

# 10) `@wraps`

## What it does

`@wraps` is used inside custom decorators to preserve the original function’s metadata.

Without it, the wrapped function may lose:

* name
* docstring
* module information
* signature-related metadata

## Why it matters

It makes debugging and introspection much better.

## Example without `@wraps`

```python
def log_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before call")
        result = func(*args, **kwargs)
        print("After call")
        return result
    return wrapper
```

This works, but metadata is not preserved.

## Example with `@wraps`

```python
from functools import wraps

def log_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print("Before call")
        result = func(*args, **kwargs)
        print("After call")
        return result
    return wrapper
```

Usage:

```python
@log_decorator
def greet():
    """Say hello"""
    print("Hello")
```

Now `greet.__name__` and `greet.__doc__` remain meaningful.

## Best use cases

* custom decorators
* logging decorators
* timing decorators
* authorization decorators
* retry decorators

## Example use case

```python
from functools import wraps

def require_login(func):
    @wraps(func)
    def wrapper(user, *args, **kwargs):
        if not user.is_logged_in:
            raise PermissionError("Login required")
        return func(user, *args, **kwargs)
    return wrapper
```

This is a very common pattern in real applications.

---

# 11) `@contextmanager`

## What it does

`@contextmanager` turns a generator function into a context manager, allowing it to be used with the `with` statement.

It is part of `contextlib`.

## Why it matters

It helps manage setup and cleanup logic in a readable way.

## Example

```python
from contextlib import contextmanager

@contextmanager
def open_resource():
    print("Setup")
    try:
        yield "resource"
    finally:
        print("Cleanup")
```

Usage:

```python
with open_resource() as resource:
    print("Using", resource)
```

## Best use cases

* temporary resources
* lock management
* transaction-style logic
* setup/cleanup wrappers

## Example use case

```python
from contextlib import contextmanager

@contextmanager
def database_transaction(connection):
    connection.begin()
    try:
        yield connection
        connection.commit()
    except Exception:
        connection.rollback()
        raise
```

This is useful because it keeps transaction logic compact and readable.

## Important behavior

The code before `yield` is setup, and the code after `yield` is cleanup.

---

# Quick comparison table

| Decorator               | Main purpose                               | Common use case          |
| ----------------------- | ------------------------------------------ | ------------------------ |
| `@property`             | Attribute-like access to a method          | Computed attributes      |
| `@property` + `@setter` | Controlled assignment                      | Validation on write      |
| `@staticmethod`         | Class-scoped helper without `self`/`cls`   | Utility helpers          |
| `@classmethod`          | Behavior tied to the class                 | Alternate constructors   |
| `@abstractmethod`       | Force subclasses to implement methods      | Interfaces / contracts   |
| `@dataclass`            | Auto-generate boilerplate for data classes | Simple models            |
| `@cached_property`      | Cache a computed attribute                 | Expensive derived values |
| `@lru_cache`            | Cache function results                     | Expensive pure functions |
| `@singledispatch`       | Type-based function specialization         | Overloaded behavior      |
| `@wraps`                | Preserve metadata in custom decorators     | Writing decorators       |
| `@contextmanager`       | Create context managers from generators    | Resource cleanup         |

---

# Which one should you use?

## Use `@property` when:

* you want attribute-style access,
* the value is computed,
* or you want a cleaner public API.

## Use `@setter` when:

* you want to validate or normalize values during assignment.

## Use `@staticmethod` when:

* the method is related to the class,
* but needs neither instance nor class state.

## Use `@classmethod` when:

* the method works with class data,
* or should create instances in a subclass-friendly way.

## Use `@abstractmethod` when:

* you want to enforce an interface for subclasses.

## Use `@dataclass` when:

* your class mainly stores data.

## Use `@cached_property` when:

* a property is expensive to compute,
* and the result can be reused.

## Use `@lru_cache` when:

* a function is pure and frequently called with the same inputs.

## Use `@singledispatch` when:

* behavior should change based on the type of the first argument.

## Use `@wraps` when:

* you are writing your own decorator.

## Use `@contextmanager` when:

* you need a clean way to manage setup and cleanup.

---

# Common interview answers

## Q: What is a decorator?

A: A decorator is a callable that takes a function or class and returns a modified version of it.

## Q: Why use `@property`?

A: To expose computed logic as attribute access and keep the API clean.

## Q: Why use `@classmethod` instead of `@staticmethod`?

A: Use `@classmethod` when you need access to the class itself, especially for alternate constructors and inheritance-friendly factories.

## Q: Why use `@wraps`?

A: It preserves the original function’s metadata when writing custom decorators.

## Q: What is the benefit of `@dataclass`?

A: It removes boilerplate for simple data-holder classes.

---

# Final takeaway

A decorator is not magic; it is a clean way to modify behavior.

The most important built-in decorators to remember are:

* `@property`
* `@staticmethod`
* `@classmethod`
* `@abstractmethod`
* `@dataclass`
* `@cached_property`
* `@lru_cache`
* `@singledispatch`
* `@wraps`
* `@contextmanager`

Among these, `@property`, `@staticmethod`, `@classmethod`, `@abstractmethod`, and `@dataclass` are especially common in interviews and real-world application code.
