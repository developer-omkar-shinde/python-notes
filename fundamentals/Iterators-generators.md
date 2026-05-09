# Python Basics — Topic 7: Iterators & Generators

This is one of the most important advanced Python topics.

Understanding iterators and generators helps you deeply understand:

- loops
- memory efficiency
- lazy evaluation
- async programming
- FastAPI internals
- streaming APIs
- large datasets

Generators are heavily used in:

- backend systems
- pipelines
- async frameworks
- data streaming
- middleware
- background processing

---

# What is Iteration?

Iteration means:
processing items one-by-one.

Example:

```python
numbers = [1, 2, 3]

for num in numbers:
    print(num)
```

Python internally uses:

- iterators

---

# Iterable vs Iterator

VERY IMPORTANT.

---

# Iterable

Object that can produce iterator.

Examples:

- list
- tuple
- string
- set
- dictionary

Example:

```python
numbers = [1, 2, 3]
```

List is iterable.

---

# Iterator

Object that remembers current state during iteration.

Uses:

- **iter**()
- **next**()

---

# iter()

Convert iterable → iterator.

```python
numbers = [1, 2, 3]

iterator = iter(numbers)
```

---

# next()

Get next value.

```python
print(next(iterator))
print(next(iterator))
print(next(iterator))
```

Output:

```python
1
2
3
```

---

# StopIteration

After values finish:

```python
next(iterator)
```

Raises:

```python
StopIteration
```

---

# How for-loop Works Internally

This:

```python
for item in numbers:
    print(item)
```

Internally similar to:

```python
iterator = iter(numbers)

while True:
    try:
        item = next(iterator)
        print(item)
    except StopIteration:
        break
```

VERY important concept.

---

# Creating Custom Iterator

```python
class Counter:
    def __init__(self, max):
        self.current = 0
        self.max = max

    def __iter__(self):
        return self

    def __next__(self):
        if self.current >= self.max:
            raise StopIteration

        self.current += 1

        return self.current
```

Usage:

```python
counter = Counter(3)

for num in counter:
    print(num)
```

Output:

```python
1
2
3
```

---

# Problem with Iterators

Complex to write manually.

Generators solve this elegantly.

---

# What is Generator?

Generator is simpler way to create iterators.

Uses:

- yield

instead of:

- return

---

# Basic Generator Example

```python
def count():
    yield 1
    yield 2
    yield 3
```

Usage:

```python
gen = count()

print(next(gen))
print(next(gen))
```

Output:

```python
1
2
```

---

# Important Difference: yield vs return

---

## return

Ends function completely.

---

## yield

Pauses function and remembers state.

Function resumes later.

This is HUGE.

---

# Generator State Preservation

```python
def counter():
    print("Start")

    yield 1

    print("Middle")

    yield 2

    print("End")
```

Usage:

```python
gen = counter()

print(next(gen))
print(next(gen))
```

Output:

```python
Start
1
Middle
2
```

Function pauses between yields.

---

# Generator Object

Calling generator function does NOT execute immediately.

```python
gen = counter()
```

Creates generator object.

Execution starts only when:

- next()
- loop

---

# Looping Through Generator

```python
def numbers():
    yield 1
    yield 2
    yield 3

for num in numbers():
    print(num)
```

---

# Why Generators Matter

Generators are:

- memory efficient
- lazy
- scalable

They produce values only when needed.

---

# Memory Example

BAD:

```python
numbers = [x for x in range(1000000)]
```

Creates HUGE list in memory.

---

# Better Generator

```python
numbers = (x for x in range(1000000))
```

Lazy generation.

Much more efficient.

---

# Generator Expression

Like list comprehension.

---

## List Comprehension

```python
nums = [x * 2 for x in range(5)]
```

Creates full list.

---

## Generator Expression

```python
nums = (x * 2 for x in range(5))
```

Lazy iterator.

---

# yield from

Delegate iteration.

```python
def sub():
    yield 1
    yield 2

def main():
    yield from sub()
    yield 3
```

---

# Infinite Generators

```python
def infinite():
    num = 0

    while True:
        yield num
        num += 1
```

Used carefully.

---

# Generators vs Lists

---

## Lists

- store everything in memory
- faster repeated access

---

## Generators

- lazy
- memory efficient
- one-time iteration

---

# Common Backend Use Cases

Generators are used in:

- streaming responses
- large DB queries
- file processing
- pipelines
- background jobs
- async systems

---

# FastAPI Example Conceptually

Streaming large responses:

```python
def stream_data():
    for i in range(1000000):
        yield f"Data {i}"
```

Without loading everything into memory.

---

# Generator Pipeline Example

```python
def numbers():
    for i in range(5):
        yield i

def squared(nums):
    for num in nums:
        yield num * num

result = squared(numbers())

for value in result:
    print(value)
```

Very scalable design pattern.

---

# send() Method

Advanced feature.

Can send values INTO generator.

```python
def generator():
    value = yield

    print(value)
```

Advanced topic.
Useful later in async internals.

---

# close() Method

Stop generator manually.

```python
gen.close()
```

---

# Generator Exhaustion

Generators can be consumed once.

Example:

```python
gen = (x for x in range(3))

print(list(gen))
print(list(gen))
```

Output:

```python
[0, 1, 2]
[]
```

---

# Backend Relevance

Generators help with:

- performance
- memory optimization
- streaming
- async systems
- scalable APIs
- data pipelines

Modern Python backend systems use generators heavily.

---

# Mini Exercises

## 1. What’s the output?

```python
def test():
    yield 1
    yield 2

gen = test()

print(next(gen))
```

---

## 2. Why is generator more memory efficient?

```python
(x for x in range(1000000))
```

vs

```python
[x for x in range(1000000)]
```

---

## 3. What happens here?

```python
gen = (x for x in range(3))

print(list(gen))
print(list(gen))
```

---

# Important Takeaways

You MUST deeply understand:

- iterable vs iterator
- next()
- yield
- lazy evaluation
- generator expressions
- memory efficiency

These concepts are foundational for advanced Python backend engineering and async systems.
