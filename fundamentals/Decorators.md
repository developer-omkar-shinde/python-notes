# Python Basics — Topic 8: Decorators

Decorators are one of the MOST IMPORTANT advanced Python concepts.

Decorators are used heavily in:
- FastAPI
- Flask
- Django
- authentication
- middleware
- logging
- caching
- rate limiting
- dependency injection

Understanding decorators deeply is ESSENTIAL for backend engineering.

---

# What is a Decorator?

Decorator is:
- a function that modifies another function

In Python:
functions are first-class objects.

Meaning:
- functions can be passed around
- stored in variables
- returned from functions

Decorators are built on this concept.

---

# Functions as Objects

```python
def greet():
    print("Hello")
```

Assign function to variable:

```python
say_hello = greet

say_hello()
```

Output:

```python
Hello
```

---

# Passing Functions as Arguments

```python
def greet():
    print("Hello")

def execute(func):
    func()

execute(greet)
```

Output:

```python
Hello
```

This concept powers decorators.

---

# Returning Functions

```python
def outer():
    def inner():
        print("Inside inner")

    return inner
```

Usage:

```python
func = outer()

func()
```

Output:

```python
Inside inner
```

---

# Basic Decorator Structure

```python
def decorator(func):
    def wrapper():
        print("Before function")

        func()

        print("After function")

    return wrapper
```

---

# Applying Decorator Manually

```python
def greet():
    print("Hello")

greet = decorator(greet)

greet()
```

Output:

```python
Before function
Hello
After function
```

---

# @ Syntax

Python shorthand.

Instead of:

```python
greet = decorator(greet)
```

Use:

```python
@decorator
def greet():
    print("Hello")
```

Cleaner and standard approach.

---

# Decorator Execution Flow

```python
@decorator
def greet():
    print("Hello")
```

Internally becomes:

```python
greet = decorator(greet)
```

VERY important understanding.

---

# Decorators with Arguments

Problem:

```python
@decorator
def add(a, b):
    return a + b
```

Wrapper must support arguments.

---

# Using *args and **kwargs

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        print("Before")

        result = func(*args, **kwargs)

        print("After")

        return result

    return wrapper
```

This is standard practice.

---

# Preserving Return Values

VERY important.

```python
return result
```

Without this:
decorated function may break.

---

# functools.wraps

Decorators can overwrite metadata.

Problem:

```python
print(greet.__name__)
```

May show:
- wrapper

instead of:
- greet

---

# Correct Approach

```python
from functools import wraps

def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)

    return wrapper
```

Always use:
- @wraps

in production decorators.

---

# Real Example: Logging Decorator

```python
from functools import wraps

def logger(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")

        result = func(*args, **kwargs)

        print(f"Finished {func.__name__}")

        return result

    return wrapper
```

Usage:

```python
@logger
def add(a, b):
    return a + b

print(add(2, 3))
```

---

# Decorators with Parameters

Advanced pattern.

```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)

        return wrapper

    return decorator
```

Usage:

```python
@repeat(3)
def greet():
    print("Hello")
```

Output:

```python
Hello
Hello
Hello
```

---

# Stacked Decorators

```python
@decorator1
@decorator2
def greet():
    pass
```

Equivalent to:

```python
greet = decorator1(decorator2(greet))
```

Order matters.

---

# Common Backend Decorator Use Cases

Decorators are everywhere in backend frameworks.

---

# Authentication

```python
@login_required
def dashboard():
    pass
```

---

# Caching

```python
@cache
def get_data():
    pass
```

---

# Rate Limiting

```python
@limit_requests
def api():
    pass
```

---

# Logging

```python
@logger
def handler():
    pass
```

---

# Timing Performance

```python
import time
from functools import wraps

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()

        result = func(*args, **kwargs)

        end = time.time()

        print(end - start)

        return result

    return wrapper
```

---

# FastAPI Decorators

FastAPI heavily uses decorators.

Example:

```python
@app.get("/users")
def get_users():
    return []
```

@app.get is decorator.

It registers function as API route.

This is why decorators are ESSENTIAL for FastAPI.

---

# Class-Based Decorators

Decorators can also use classes.

```python
class Logger:
    def __call__(self, func):
        def wrapper(*args, **kwargs):
            print("Logging")

            return func(*args, **kwargs)

        return wrapper
```

Advanced topic.

---

# Closures + Decorators

Decorators rely heavily on:
- closures
- enclosing scope

Example:

```python
def repeat(times):
    def decorator(func):
        def wrapper():
            for _ in range(times):
                func()

        return wrapper

    return decorator
```

wrapper remembers:
- times

through closure.

---

# Important Decorator Concepts

You MUST understand:
- functions as objects
- nested functions
- closures
- *args/**kwargs
- returning functions

Decorators are built on ALL these concepts.

---

# Backend Relevance

Decorators power:
- routing
- middleware
- auth
- validation
- caching
- logging
- dependency injection
- performance monitoring

Modern Python frameworks rely heavily on decorators.

---

# Mini Exercises

## 1. What’s the output?

```python
def decorator(func):
    def wrapper():
        print("Before")

        func()

    return wrapper

@decorator
def greet():
    print("Hello")

greet()
```

---

## 2. What does this become internally?

```python
@decorator
def test():
    pass
```

---

## 3. Why use *args and **kwargs in wrapper?

Think about:
- flexibility
- supporting all functions

---

# Important Takeaways

You MUST deeply understand:
- decorators
- closures
- wrappers
- function references
- @ syntax
- functools.wraps

Decorators are foundational for advanced Python backend engineering and FastAPI.
