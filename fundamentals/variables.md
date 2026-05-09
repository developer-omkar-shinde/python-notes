# Python Basics — Topic 1: Variables & Data Types

## Variables in Python

Python is dynamically typed.

```python
name = "Omkar"
age = 25
is_dev = True
```

Unlike TypeScript:

```ts
let name: string = "Omkar";
```

Python does NOT require explicit type declarations.

---

## Important Python Difference

Variables are references to objects.

This becomes VERY important later for:
- mutability
- memory
- FastAPI request handling
- ORM behavior

Example:

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

Because both variables reference the same object.

This is one of the most important Python concepts.

---

# Primitive/Common Data Types

## String

```python
name = "Omkar"
```

---

## Integer

```python
age = 25
```

---

## Float

```python
price = 99.99
```

---

## Boolean

```python
is_active = True
```

Python uses:
- `True`
- `False`

(not lowercase like JS)

---

## None

Equivalent to:
- `null` in JS

```python
user = None
```

Very common in backend APIs.

---

## Type Checking

```python
print(type(name))
```

Output:

```python
<class 'str'>
```

Everything in Python is an object.

Even:

```python
type(5)
```

returns:

```python
<class 'int'>
```

---

## Type Conversion

```python
age = "25"

converted_age = int(age)
```

Other conversions:

```python
str()
float()
bool()
list()
dict()
```

---

## Python Naming Convention

Use:
- `snake_case`

```python
first_name = "Omkar"
```

NOT:
- `camelCase`

---

## Constants

Python has no real constants.

Convention:

```python
MAX_USERS = 100
```

---

## Multiple Assignment

Very Pythonic:

```python
x, y = 10, 20
```

Swap values:

```python
x, y = y, x
```

This is heavily loved in Python.

---

## F-Strings (VERY IMPORTANT)

Equivalent to template literals in JS.

```python
name = "Omkar"

print(f"Hello {name}")
```

Used everywhere in backend development.

---

# Backend Relevance

These basics matter for:
- request payloads
- JSON handling
- DB models
- API responses
- environment configs
- serialization

---

# Mini Exercises

## 1. What’s the output?

```python
a = 10
b = a

b = 20

print(a)
```

---

## 2. Why is the output different?

```python
a = [1, 2]
b = a

b.append(3)

print(a)
```

---

# Next Topics

1. Lists/Tuples/Sets/Dictionaries
2. Functions
3. Scope & memory
4. Mutability vs immutability
5. Classes & objects
6. Iterators/generators
7. Decorators
8. Asyncio
