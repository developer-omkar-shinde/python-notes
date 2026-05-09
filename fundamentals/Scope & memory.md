# Python Basics — Topic 4: Scope & Memory

This is one of the MOST IMPORTANT Python topics.

Understanding:
- scope
- references
- memory
- mutability

will make advanced Python MUCH easier.

These concepts are critical in:
- FastAPI
- async programming
- decorators
- caching
- ORMs
- dependency injection
- performance optimization

---

# Variables Store REFERENCES

In Python:
variables do NOT directly store values.

They store references to objects in memory.

Example:

```python
a = 10
```

Internally:

```text
a ---> object(10)
```

---

# Assignment Creates References

```python
a = [1, 2]
b = a
```

Both variables point to SAME object.

```text
a ---> [1, 2] <--- b
```

---

# Mutable Example

```python
a = [1, 2]
b = a

b.append(3)

print(a)
```

Output:

```python
[1, 2, 3]
```

Because:
- same object
- same memory reference

---

# Immutable Example

```python
a = 10
b = a

b = 20

print(a)
```

Output:

```python
10
```

Why?

Because integers are immutable.

When:

```python
b = 20
```

Python creates NEW object.

```text
a ---> 10

b ---> 20
```

---

# Identity vs Equality

VERY IMPORTANT.

---

## Equality ==

Checks value.

```python
a = [1, 2]
b = [1, 2]

print(a == b)
```

Output:

```python
True
```

---

## Identity is

Checks memory location.

```python
print(a is b)
```

Output:

```python
False
```

Because:
- different objects
- same values

---

# id()

Shows memory identity.

```python
a = [1, 2]

print(id(a))
```

Useful for understanding references.

---

# Scope

Scope decides:
where variable is accessible.

---

# Local Scope

Variables inside function.

```python
def test():
    name = "Omkar"

    print(name)
```

Accessible ONLY inside function.

---

# Global Scope

Variables outside functions.

```python
name = "Omkar"

def show():
    print(name)
```

Function can access global variable.

---

# LEGB Rule

Python searches variables in this order:

1. Local
2. Enclosing
3. Global
4. Built-in

VERY important for closures/decorators.

---

# Example of LEGB

```python
name = "Global"

def outer():
    name = "Outer"

    def inner():
        name = "Inner"

        print(name)

    inner()

outer()
```

Output:

```python
Inner
```

Python searches nearest scope first.

---

# Enclosing Scope

Nested function scope.

```python
def outer():
    message = "Hello"

    def inner():
        print(message)

    inner()
```

inner() accesses enclosing variable.

---

# global Keyword

Modify global variable.

```python
count = 0

def increment():
    global count

    count += 1
```

Avoid overusing global variables.

Bad for large backend systems.

---

# nonlocal Keyword

Modify enclosing scope variable.

```python
def outer():
    count = 0

    def inner():
        nonlocal count

        count += 1

    inner()

    print(count)

outer()
```

Output:

```python
1
```

Important for closures.

---

# Mutable Default Argument Trap

VERY IMPORTANT PYTHON INTERVIEW QUESTION.

BAD:

```python
def add_item(item, items=[]):
    items.append(item)

    return items
```

Problem:

```python
print(add_item(1))
print(add_item(2))
```

Output:

```python
[1]
[1, 2]
```

Because SAME list reused.

---

# Correct Approach

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)

    return items
```

This is standard Python practice.

---

# Garbage Collection

Python automatically manages memory.

Unused objects get cleaned.

You usually don't manually free memory.

---

# Reference Counting

Python tracks:
how many references point to object.

When references become 0:
object can be removed.

---

# Shallow Copy vs Deep Copy

VERY IMPORTANT.

---

# Shallow Copy

```python
import copy

a = [[1, 2]]
b = copy.copy(a)
```

Outer object copied.
Inner nested objects shared.

---

# Deep Copy

```python
b = copy.deepcopy(a)
```

Everything copied recursively.

Important for:
- API transformations
- nested data
- caching systems

---

# Stack vs Heap (Conceptually)

---

## Stack
Stores:
- function calls
- local references

---

## Heap
Stores:
- actual objects

---

# Backend Relevance

Understanding memory helps with:
- caching
- async behavior
- request handling
- mutable bugs
- ORM objects
- performance optimization

---

# Common Backend Bug Example

```python
DEFAULT_RESPONSE = {}

def handler():
    DEFAULT_RESPONSE["status"] = "ok"

    return DEFAULT_RESPONSE
```

This shared mutable object can create bugs.

---

# Mini Exercises

## 1. What’s the output?

```python
a = [1, 2]
b = a

b.append(3)

print(a)
```

---

## 2. What’s the output?

```python
a = [1, 2]
b = [1, 2]

print(a == b)
print(a is b)
```

---

## 3. What’s wrong here?

```python
def test(data=[]):
    data.append(1)

    return data
```

---

# Important Takeaways

You MUST deeply understand:
- references
- mutability
- scope
- closures
- identity vs equality

These concepts separate:
- beginner Python developers
from
- strong backend Python engineers.
