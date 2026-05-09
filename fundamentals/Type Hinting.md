# Python Basics — Topic 10: Type Hinting

Type hinting is one of the MOST IMPORTANT topics in modern Python backend development.

FastAPI heavily relies on:
- type hints
- validation
- editor tooling
- autocomplete
- schema generation

Even though Python is dynamically typed,
modern Python uses type hints extensively.

If you know TypeScript,
this topic will feel very familiar.

---

# What is Type Hinting?

Type hints describe expected data types.

Example:

```python
def greet(name: str) -> str:
    return f"Hello {name}"
```

---

# Important Clarification

Python type hints are NOT enforced at runtime by default.

This still works:

```python
greet(123)
```

No runtime error automatically.

Type hints mainly help:
- IDEs
- linters
- static analysis
- readability
- frameworks like FastAPI

---

# Basic Type Hints

---

# String

```python
name: str = "Omkar"
```

---

# Integer

```python
age: int = 25
```

---

# Float

```python
price: float = 99.99
```

---

# Boolean

```python
is_admin: bool = True
```

---

# Function Return Types

```python
def add(a: int, b: int) -> int:
    return a + b
```

---

# None Return

Equivalent to:
- void in TypeScript

```python
def log(message: str) -> None:
    print(message)
```

---

# List Type Hints

Modern syntax (Python 3.9+):

```python
numbers: list[int] = [1, 2, 3]
```

Older syntax:

```python
from typing import List

numbers: List[int]
```

---

# Dictionary Type Hints

```python
user: dict[str, str] = {
    "name": "Omkar"
}
```

---

# Tuple Type Hints

```python
coordinates: tuple[int, int] = (10, 20)
```

---

# Set Type Hints

```python
tags: set[str] = {"python", "fastapi"}
```

---

# Optional

Value can be:
- specific type
OR
- None

```python
from typing import Optional

name: Optional[str] = None
```

Equivalent to:

```python
str | None
```

Modern syntax (Python 3.10+):

```python
name: str | None = None
```

Very common in backend APIs.

---

# Union

Multiple possible types.

```python
from typing import Union

value: Union[str, int]
```

Modern syntax:

```python
value: str | int
```

---

# Any

Accepts anything.

```python
from typing import Any

data: Any
```

Avoid excessive use.

Loses type safety.

---

# Typed Dictionaries

Useful for structured dictionary objects.

```python
from typing import TypedDict

class User(TypedDict):
    name: str
    age: int
```

Usage:

```python
user: User = {
    "name": "Omkar",
    "age": 25
}
```

Very useful for API payloads.

---

# Literal Types

Restrict exact values.

```python
from typing import Literal

status: Literal["success", "error"]
```

Very useful in APIs.

---

# Function Type Hints

Functions as arguments.

```python
from typing import Callable

def process(func: Callable[[int], int]):
    return func(10)
```

---

# Type Aliases

Create reusable types.

```python
UserId = int

user_id: UserId = 10
```

Improves readability.

---

# Generic Types

Advanced topic.

```python
from typing import TypeVar

T = TypeVar("T")
```

Used for reusable type-safe code.

Important later for advanced backend architecture.

---

# Type Hinting Classes

```python
class User:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age
```

Very common.

---

# Dataclasses + Type Hints

```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    age: int
```

Modern Python backend style.

---

# FastAPI Example

FastAPI heavily uses type hints.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    return {"user_id": user_id}
```

FastAPI automatically:
- validates input
- generates docs
- parses types

using type hints.

This is HUGE.

---

# Pydantic Uses Type Hints

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
```

Validation happens automatically.

---

# Static Type Checking

Python tools:
- mypy
- pyright
- pylance

can analyze type errors before runtime.

Example:

```python
def add(a: int, b: int) -> int:
    return a + b

add("1", "2")
```

Type checker warns:
- wrong argument types

---

# Duck Typing vs Type Hinting

Python still supports:
- duck typing

Type hints improve:
- clarity
- tooling
- maintainability

Modern Python combines both.

---

# Protocols (Advanced)

Structural typing.

```python
from typing import Protocol

class Reader(Protocol):
    def read(self) -> str:
        pass
```

Very powerful for backend architecture.

---

# Runtime Validation vs Type Hints

Type hints alone DO NOT validate runtime data.

Frameworks like:
- FastAPI
- Pydantic

use type hints for runtime validation.

---

# Common Backend Uses

Type hints improve:
- API validation
- autocomplete
- documentation
- maintainability
- refactoring
- schema generation

Modern backend Python code heavily relies on type hints.

---

# Common Beginner Mistake

Thinking type hints enforce runtime validation automatically.

This still runs:

```python
def greet(name: str):
    print(name)

greet(123)
```

Python itself does NOT stop it.

---

# Mini Exercises

## 1. What does this mean?

```python
def add(a: int, b: int) -> int:
```

---

## 2. What does Optional[str] mean?

Think about:
- None values

---

## 3. Why are type hints valuable in FastAPI?

Think about:
- validation
- docs
- autocomplete

---

# Important Takeaways

You MUST deeply understand:
- function type hints
- Optional
- Union
- list/dict typing
- TypedDict
- dataclasses
- static analysis

Type hints are foundational for modern Python backend engineering and FastAPI.
