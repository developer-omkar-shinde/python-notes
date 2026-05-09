# Python Basics — Topic 2: Lists, Tuples, Sets, Dictionaries

These are the most used data structures in Python backend development.

You’ll use them constantly in:
- FastAPI
- JSON APIs
- database responses
- request payloads
- caching
- authentication systems

---

# 1. Lists

Lists are:
- ordered
- mutable
- allow duplicates

Equivalent to JavaScript arrays.

```python
users = ["Omkar", "John", "Alice"]
```

---

## Accessing Items

```python
print(users[0])
```

Output:

```python
Omkar
```

Negative indexing:

```python
print(users[-1])
```

Output:

```python
Alice
```

---

## Updating Lists

```python
users.append("Bob")
```

```python
users.remove("John")
```

```python
users.insert(1, "Sam")
```

---

## Looping Through Lists

```python
for user in users:
    print(user)
```

---

## List Slicing

```python
numbers = [1, 2, 3, 4, 5]

print(numbers[1:4])
```

Output:

```python
[2, 3, 4]
```

---

## List Comprehension (VERY IMPORTANT)

Very Pythonic.

```python
numbers = [1, 2, 3, 4]

squared = [n * n for n in numbers]

print(squared)
```

Output:

```python
[1, 4, 9, 16]
```

Used heavily in backend code.

---

# 2. Tuples

Tuples are:
- ordered
- immutable
- allow duplicates

```python
coordinates = (10, 20)
```

---

## Why Tuples Matter

Useful when data should not change.

Examples:
- database rows
- coordinates
- configuration values

---

## Tuple Unpacking

```python
x, y = coordinates

print(x)
print(y)
```

Very common in Python.

---

# 3. Sets

Sets are:
- unordered
- mutable
- NO duplicates

```python
tags = {"python", "fastapi", "backend"}
```

---

## Duplicate Removal

```python
numbers = {1, 2, 2, 3}

print(numbers)
```

Output:

```python
{1, 2, 3}
```

---

## Set Operations

### Union

```python
a = {1, 2}
b = {2, 3}

print(a | b)
```

Output:

```python
{1, 2, 3}
```

---

## Intersection

```python
print(a & b)
```

Output:

```python
{2}
```

---

## Why Sets Matter in Backend

Great for:
- unique IDs
- permissions
- tags
- fast lookup operations

Sets are much faster than lists for lookups.

---

# 4. Dictionaries (MOST IMPORTANT)

Dictionaries are:
- key-value pairs
- mutable
- ordered (Python 3.7+)

Equivalent to JavaScript objects.

```python
user = {
    "name": "Omkar",
    "age": 25,
    "is_admin": True
}
```

---

## Access Values

```python
print(user["name"])
```

---

## Safer Access with get()

```python
print(user.get("email"))
```

Returns:

```python
None
```

instead of crashing.

Very important in APIs.

---

## Updating Dictionary

```python
user["age"] = 26
```

---

## Adding New Keys

```python
user["city"] = "Pune"
```

---

## Looping Through Dictionary

```python
for key, value in user.items():
    print(key, value)
```

---

# Nested Dictionaries

Very common in API responses.

```python
response = {
    "user": {
        "name": "Omkar",
        "skills": ["React", "Python"]
    }
}
```

Access:

```python
print(response["user"]["skills"])
```

---

# JSON vs Dictionary

IMPORTANT:

JSON is basically a string representation of a dictionary.

Example JSON:

```json
{
  "name": "Omkar"
}
```

Python dictionary:

```python
{
    "name": "Omkar"
}
```

FastAPI constantly converts:
- JSON → Python dict
- Python dict → JSON

---

# Mutability (VERY IMPORTANT)

## Mutable
Can change:
- list
- dict
- set

---

## Immutable
Cannot change:
- tuple
- string
- int
- float
- bool

This concept is CRITICAL in Python.

---

# Backend Relevance

## Lists
Used for:
- API arrays
- query results
- collections

---

## Dictionaries
Used for:
- request payloads
- JSON responses
- configurations
- ORM data

---

## Sets
Used for:
- permissions
- unique values
- optimized lookups

---

## Tuples
Used for:
- fixed data
- immutable structures

---

# Mini Exercises

## 1. What’s the output?

```python
numbers = [1, 2, 3]
numbers.append(4)

print(numbers)
```

---

## 2. Why does this fail?

```python
coordinates = (10, 20)

coordinates[0] = 100
```

---

## 3. What’s the output?

```python
data = {
    "name": "Omkar"
}

print(data.get("age"))
```

---

# Important Takeaway

If you master:
- lists
- dictionaries
- mutability

you can already understand a large amount of Python backend code.
