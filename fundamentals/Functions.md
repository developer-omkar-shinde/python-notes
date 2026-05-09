# Python Basics — Topic 3: Functions

Functions are one of the MOST important concepts in Python.

In backend development, functions are everywhere:

- API routes
- business logic
- middleware
- authentication
- database queries
- dependency injection
- utilities/helpers

Python treats functions as FIRST-CLASS OBJECTS.

This becomes extremely important later in:

- FastAPI
- decorators
- async programming
- middleware
- dependency injection

---

# Defining a Function

```python
def greet():
    print("Hello")
```

Call it:

```python
greet()
```

---

# Function with Parameters

```python
def greet(name):
    print(f"Hello {name}")
```

Usage:

```python
greet("Omkar")
```

---

# Return Values

```python
def add(a, b):
    return a + b
```

```python
result = add(10, 20)

print(result)
```

---

# Important Difference

## print()

Displays value.

## return

Sends value back from function.

Very important distinction.

---

# Multiple Parameters

```python
def create_user(name, age, is_admin):
    return {
        "name": name,
        "age": age,
        "is_admin": is_admin
    }
```

---

# Default Parameters

```python
def greet(name="Guest"):
    print(f"Hello {name}")
```

Usage:

```python
greet()
```

Output:

```python
Hello Guest
```

---

# Keyword Arguments

Very Pythonic.

```python
create_user(
    name="Omkar",
    age=25,
    is_admin=True
)
```

Improves readability heavily.

Used everywhere in Python frameworks.

---

# Positional vs Keyword Arguments

## Positional

```python
add(10, 20)
```

---

## Keyword

```python
add(a=10, b=20)
```

---

# \*args

Accept multiple positional arguments.

```python
def total(*numbers):
    return sum(numbers)
```

Usage:

```python
print(total(1, 2, 3, 4))
```

Output:

```python
10
```

---

# \*\*kwargs

Accept multiple keyword arguments.

```python
def print_user(**user):
    print(user)
```

Usage:

```python
print_user(name="Omkar", age=25)
```

Output:

```python
{
    'name': 'Omkar',
    'age': 25
}
```

Very important in frameworks.

---

# Functions are Objects

THIS IS HUGE IN PYTHON.

```python
def greet():
    print("Hello")
```

Assign to variable:

```python
say_hello = greet

say_hello()
```

Functions can:

- be stored in variables
- passed to other functions
- returned from functions

This powers:

- decorators
- middleware
- FastAPI internals

---

# Nested Functions

```python
def outer():
    def inner():
        print("Inside inner")

    inner()
```

---

# Closures (IMPORTANT)

```python
def outer(message):
    def inner():
        print(message)

    return inner
```

Usage:

```python
my_func = outer("Hello")

my_func()
```

This concept becomes important for:

- decorators
- middleware
- dependency injection

---

# Lambda Functions

Anonymous one-line functions.

```python
square = lambda x: x * x

print(square(5))
```

Equivalent to:

```python
def square(x):
    return x * x
```

---

# map()

Apply function to all items.

```python
numbers = [1, 2, 3]

squared = list(map(lambda x: x * x, numbers))

print(squared)
```

---

# filter()

Filter items.

```python
numbers = [1, 2, 3, 4]

evens = list(filter(lambda x: x % 2 == 0, numbers))

print(evens)
```

---

# Scope

Variables inside function are local.

```python
def test():
    name = "Omkar"

print(name)
```

This causes error because:

- name exists only inside function

---

# Global Variables

```python
name = "Omkar"

def show():
    print(name)
```

Avoid excessive global variables in backend systems.

---

# Docstrings

Python documentation inside functions.

```python
def add(a, b):
    \"\"\"Adds two numbers\"\"\"

    return a + b
```

Used heavily in production Python.

---

# Type Hints (VERY IMPORTANT)

Python supports type hints.

```python
def add(a: int, b: int) -> int:
    return a + b
```

Very important for:

- FastAPI
- IDE autocomplete
- large backend systems

Even though Python is dynamically typed.

---

# Backend Relevance

Functions power:

- API endpoints
- service layers
- middleware
- authentication
- validation
- database access
- dependency injection

Everything in FastAPI revolves around functions.

---

# Mini Exercises

## 1. What’s the output?

```python
def add(a, b):
    return a + b

print(add(2, 3))
```

---

## 2. What’s wrong here?

```python
def greet(name="Omkar", age):
    print(name, age)
```

---

## 3. What’s the output?

```python
def outer():
    message = "Hello"

    def inner():
        print(message)

    return inner

func = outer()

func()
```

---

# Important Takeaway

To become strong in Python backend development:

- understand functions deeply
- understand closures
- understand scope
- understand functions as objects

These concepts are foundational for advanced Python.
