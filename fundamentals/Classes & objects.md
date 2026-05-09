# Python Basics — Topic 6: Classes & Objects

Classes and objects are foundational for:
- backend architecture
- ORMs
- FastAPI
- Pydantic
- dependency injection
- reusable code design

Python OOP is different from:
- Java
- C#
- TypeScript

Python is:
- more dynamic
- protocol-oriented
- flexible
- heavily object-based

Everything in Python is an object.

---

# What is a Class?

A class is a blueprint for creating objects.

Example:

```python
class User:
    pass
```

---

# Creating Objects

```python
user1 = User()

print(user1)
```

Object created from class.

---

# __init__ Method

Constructor method.

Runs automatically when object created.

```python
class User:
    def __init__(self, name):
        self.name = name
```

Usage:

```python
user1 = User("Omkar")

print(user1.name)
```

Output:

```python
Omkar
```

---

# self Keyword

Represents current object instance.

Equivalent to:
- this in JavaScript

Example:

```python
class User:
    def __init__(self, name):
        self.name = name
```

---

# Instance Attributes

Variables attached to object.

```python
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Each object has separate values.

---

# Instance Methods

Functions inside class.

```python
class User:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hello {self.name}")
```

Usage:

```python
user = User("Omkar")

user.greet()
```

---

# Class Attributes

Shared across all objects.

```python
class User:
    role = "member"
```

Usage:

```python
print(User.role)
```

---

# Difference: Instance vs Class Attribute

---

## Instance Attribute

Unique per object.

```python
self.name
```

---

## Class Attribute

Shared across all instances.

```python
role = "member"
```

---

# __str__ Method

Controls object display.

```python
class User:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return self.name
```

Usage:

```python
user = User("Omkar")

print(user)
```

Output:

```python
Omkar
```

---

# Encapsulation

Python does NOT enforce strict private variables.

Convention:

```python
class User:
    def __init__(self):
        self._name = "Omkar"
```

Single underscore means:
- internal use

---

# Name Mangling

Double underscore:

```python
class User:
    def __init__(self):
        self.__password = "secret"
```

Python mangles name internally.

Not truly private.

---

# Getters & Setters

Pythonic way uses properties.

---

# Property Decorator

```python
class User:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age
```

Usage:

```python
user = User(25)

print(user.age)
```

Looks like attribute access.

---

# Setter

```python
class User:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Invalid age")

        self._age = value
```

---

# Inheritance

Child class inherits parent behavior.

```python
class Animal:
    def speak(self):
        print("Animal sound")
```

```python
class Dog(Animal):
    pass
```

Usage:

```python
dog = Dog()

dog.speak()
```

---

# Method Overriding

```python
class Dog(Animal):
    def speak(self):
        print("Bark")
```

---

# super()

Call parent class methods.

```python
class Animal:
    def __init__(self, name):
        self.name = name
```

```python
class Dog(Animal):
    def __init__(self, name):
        super().__init__(name)
```

---

# Polymorphism

Different classes sharing same interface.

```python
class Cat:
    def speak(self):
        print("Meow")
```

```python
class Dog:
    def speak(self):
        print("Bark")
```

Usage:

```python
animals = [Cat(), Dog()]

for animal in animals:
    animal.speak()
```

Python focuses heavily on behavior over strict inheritance.

---

# Duck Typing

VERY IMPORTANT PYTHON CONCEPT.

"If it behaves like duck, treat it like duck."

Python cares more about:
- behavior
than:
- exact type

Example:

```python
class File:
    def read(self):
        print("Reading file")
```

```python
class API:
    def read(self):
        print("Reading API")
```

Both usable similarly.

---

# Composition vs Inheritance

Python often prefers composition.

Example:

```python
class Engine:
    def start(self):
        print("Engine started")
```

```python
class Car:
    def __init__(self):
        self.engine = Engine()
```

Car HAS-A engine.

Instead of:
- Car IS-A Engine

---

# Class Methods

Operate on class itself.

```python
class User:
    total_users = 0

    @classmethod
    def increment_users(cls):
        cls.total_users += 1
```

---

# Static Methods

Utility functions inside class.

```python
class Math:
    @staticmethod
    def add(a, b):
        return a + b
```

---

# Dataclasses (VERY IMPORTANT)

Modern Python feature.

```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    age: int
```

Automatically creates:
- __init__
- __repr__
- comparisons

Used heavily in backend systems.

---

# Type Hints in Classes

```python
class User:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age
```

Very important for:
- FastAPI
- IDE support
- large codebases

---

# Backend Relevance

Classes power:
- ORM models
- service layers
- authentication systems
- dependency injection
- API schemas
- reusable architecture

FastAPI and Pydantic heavily use OOP concepts.

---

# Mini Exercises

## 1. What’s the output?

```python
class User:
    def __init__(self, name):
        self.name = name

user = User("Omkar")

print(user.name)
```

---

## 2. What’s the output?

```python
class Animal:
    def speak(self):
        print("Animal")

class Dog(Animal):
    pass

dog = Dog()

dog.speak()
```

---

## 3. Why use @property?

Think about:
- validation
- cleaner APIs
- encapsulation

---

# Important Takeaways

You MUST deeply understand:
- self
- instance vs class attributes
- inheritance
- polymorphism
- composition
- properties
- duck typing

These concepts are foundational for advanced Python backend development.
