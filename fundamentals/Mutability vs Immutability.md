# Python Basics — Topic 5: Mutability vs Immutability

This is one of the MOST IMPORTANT concepts in Python.

If you deeply understand:
- mutability
- references
- object identity

you will avoid MANY backend bugs.

This topic is critical for:
- FastAPI
- async programming
- caching
- concurrency
- ORM behavior
- API request handling

---

# What is Mutability?

## Mutable Object
Can be changed AFTER creation.

Examples:
- list
- dict
- set

---

## Immutable Object
Cannot be changed AFTER creation.

Examples:
- int
- float
- bool
- string
- tuple

---

# Mutable Example

```python
numbers = [1, 2]

numbers.append(3)

print(numbers)
```

Output:

```python
[1, 2, 3]
```

List changed in-place.

---

# Immutable Example

```python
name = "Omkar"

name.upper()

print(name)
```

Output:

```python
Omkar
```

Because:
- strings are immutable
- upper() creates NEW string

Correct:

```python
name = name.upper()
```

---

# Why This Matters

In Python:
variables store references.

Mutable objects can change through ANY reference.

---

# Shared Reference Problem

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

# Immutable Reassignment

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

Because integers are immutable.

Python created NEW object for:
- b = 20

---

# Checking Identity

```python
a = [1, 2]
b = a

print(a is b)
```

Output:

```python
True
```

Both point to same object.

---

# Equality vs Identity

---

## Equality

```python
a == b
```

Checks:
- values

---

## Identity

```python
a is b
```

Checks:
- memory location

---

# Immutable Types in Detail

---

## Integers

```python
x = 10

x = x + 1
```

New integer object created.

---

## Strings

```python
name = "Omkar"

name += " Patil"
```

Creates NEW string.

---

## Tuples

```python
coordinates = (10, 20)
```

Cannot modify:

```python
coordinates[0] = 100
```

Error.

---

# Mutable Types in Detail

---

## Lists

```python
items = [1, 2]
```

Can modify:

```python
items.append(3)
```

---

## Dictionaries

```python
user = {"name": "Omkar"}

user["age"] = 25
```

---

## Sets

```python
tags = {"python"}

tags.add("fastapi")
```

---

# Important Backend Bug

Mutable shared state.

BAD:

```python
DEFAULT_CONFIG = {}

def update():
    DEFAULT_CONFIG["debug"] = True
```

Shared mutable state can create:
- race conditions
- unexpected API behavior
- threading issues

---

# Mutable Default Argument Problem

VERY IMPORTANT.

BAD:

```python
def add_item(item, items=[]):
    items.append(item)

    return items
```

Usage:

```python
print(add_item(1))
print(add_item(2))
```

Output:

```python
[1]
[1, 2]
```

Same list reused.

---

# Correct Solution

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)

    return items
```

This is standard Python practice.

---

# Copying Mutable Objects

---

# Assignment DOES NOT Copy

```python
a = [1, 2]
b = a
```

Both share same object.

---

# Shallow Copy

```python
b = a.copy()
```

OR:

```python
import copy

b = copy.copy(a)
```

Top-level copied.

Nested objects still shared.

---

# Deep Copy

```python
import copy

b = copy.deepcopy(a)
```

Everything copied recursively.

---

# Tuple Immutability Confusion

Tuple itself immutable.

But mutable objects INSIDE tuple can change.

Example:

```python
data = ([1, 2], [3, 4])

data[0].append(5)

print(data)
```

Output:

```python
([1, 2, 5], [3, 4])
```

Tuple structure fixed.
Inner list mutable.

---

# String Interning

Python optimizes immutable strings sometimes.

Example:

```python
a = "hello"
b = "hello"

print(a is b)
```

May return:

```python
True
```

Because Python may reuse immutable objects.

Do NOT rely on this behavior.

---

# Why Immutability is Valuable

Immutable objects are:
- safer
- predictable
- thread-safe
- hashable

Very important in:
- caching
- concurrent systems
- dictionaries/sets keys

---

# Hashability

Dictionary keys MUST be immutable/hashable.

Valid:

```python
user = {
    "name": "Omkar"
}
```

Invalid:

```python
data = {
    [1, 2]: "value"
}
```

Lists cannot be dictionary keys.

---

# Backend Relevance

Mutability affects:
- API state
- request data
- caching
- async tasks
- database models
- thread safety
- performance

Many production Python bugs come from:
- shared mutable state

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
name = "Omkar"

name.upper()

print(name)
```

---

## 3. Why is this dangerous?

```python
def test(data=[]):
    data.append(1)

    return data
```

---

# Important Takeaways

You MUST deeply understand:
- mutable vs immutable
- references
- shared state
- copying behavior
- identity vs equality

These concepts are foundational for advanced Python and backend engineering.
