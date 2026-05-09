# Python Basics — Topic 11: Loops

Loops are fundamental in Python backend development.

You’ll use loops constantly for:
- API data processing
- database results
- validation
- transformations
- background jobs
- file processing
- async iteration

Python loops are cleaner and more expressive than many languages.

---

# for Loop

Most commonly used loop in Python.

Example:

```python
numbers = [1, 2, 3]

for number in numbers:
    print(number)
```

Output:

```python
1
2
3
```

---

# Pythonic Looping

Unlike JavaScript:

```js
for (let i = 0; i < arr.length; i++)
```

Python prefers direct iteration:

```python
for item in items:
```

Cleaner and more readable.

---

# Looping Through Strings

```python
for char in "Python":
    print(char)
```

---

# range()

Generate sequence of numbers.

---

# Basic range

```python
for i in range(5):
    print(i)
```

Output:

```python
0
1
2
3
4
```

---

# range(start, stop)

```python
for i in range(1, 5):
    print(i)
```

Output:

```python
1
2
3
4
```

---

# range(start, stop, step)

```python
for i in range(0, 10, 2):
    print(i)
```

Output:

```python
0
2
4
6
8
```

---

# while Loop

Runs until condition becomes False.

```python
count = 0

while count < 5:
    print(count)

    count += 1
```

---

# Infinite Loop

Be careful.

```python
while True:
    print("Running")
```

Used in:
- servers
- event loops
- background workers

Usually requires:
- break

---

# break

Stops loop immediately.

```python
for i in range(10):
    if i == 5:
        break

    print(i)
```

Output:

```python
0
1
2
3
4
```

---

# continue

Skip current iteration.

```python
for i in range(5):
    if i == 2:
        continue

    print(i)
```

Output:

```python
0
1
3
4
```

---

# pass

Placeholder statement.

```python
for i in range(5):
    pass
```

Useful during development.

---

# else with Loops

Python supports:
- loop else

VERY Pythonic.

```python
for i in range(3):
    print(i)
else:
    print("Finished")
```

Runs if loop finishes normally.

---

# break prevents else

```python
for i in range(5):
    if i == 3:
        break
else:
    print("Finished")
```

Else does NOT run.

---

# enumerate()

VERY IMPORTANT.

Get:
- index
- value

```python
users = ["Omkar", "John"]

for index, user in enumerate(users):
    print(index, user)
```

Output:

```python
0 Omkar
1 John
```

Preferred over manual indexing.

---

# zip()

Loop multiple iterables together.

```python
names = ["Omkar", "John"]
ages = [25, 30]

for name, age in zip(names, ages):
    print(name, age)
```

Output:

```python
Omkar 25
John 30
```

Very useful in backend processing.

---

# Nested Loops

```python
for i in range(3):
    for j in range(2):
        print(i, j)
```

---

# Looping Dictionaries

---

# Keys

```python
user = {
    "name": "Omkar",
    "age": 25
}

for key in user:
    print(key)
```

---

# Values

```python
for value in user.values():
    print(value)
```

---

# Key + Value

```python
for key, value in user.items():
    print(key, value)
```

Very common in APIs.

---

# List Comprehension

VERY IMPORTANT.

Pythonic transformation syntax.

---

# Traditional Loop

```python
numbers = [1, 2, 3]

squared = []

for num in numbers:
    squared.append(num * num)
```

---

# List Comprehension

```python
squared = [num * num for num in numbers]
```

Cleaner and preferred.

---

# Conditional Comprehension

```python
evens = [n for n in range(10) if n % 2 == 0]
```

---

# Dictionary Comprehension

```python
squares = {
    n: n * n
    for n in range(5)
}
```

---

# Set Comprehension

```python
unique = {
    n * 2
    for n in range(5)
}
```

---

# Generator Comprehension

Lazy evaluation.

```python
numbers = (
    n * 2
    for n in range(1000000)
)
```

Memory efficient.

---

# Iterating Files

```python
with open("data.txt") as file:
    for line in file:
        print(line)
```

Very memory efficient.

---

# Async Loops

Advanced topic.

```python
async for item in generator:
    print(item)
```

Used in:
- streaming
- WebSockets
- async APIs

---

# Common Backend Loop Use Cases

Loops are used for:
- transforming API responses
- DB processing
- validation
- batch jobs
- async processing
- pagination
- caching

---

# Pythonic Principles

Python prefers:
- readability
- direct iteration
- expressive loops

Instead of:
- manual indexing everywhere

---

# Common Beginner Mistakes

---

# Modifying List While Iterating

BAD:

```python
numbers = [1, 2, 3]

for n in numbers:
    numbers.remove(n)
```

Can create bugs.

---

# Forgetting Infinite Loop Exit

BAD:

```python
while True:
    print("Forever")
```

Without:
- break
- condition

---

# Backend Relevance

Loops power:
- API processing
- background jobs
- streaming
- data transformation
- validation pipelines

You will use loops constantly in FastAPI/backend development.

---

# Mini Exercises

## 1. What’s the output?

```python
for i in range(3):
    print(i)
```

---

## 2. What’s the output?

```python
for i in range(5):
    if i == 2:
        continue

    print(i)
```

---

## 3. Convert this into list comprehension

```python
result = []

for n in range(5):
    result.append(n * 2)
```

---

# Important Takeaways

You MUST deeply understand:
- for loops
- while loops
- range()
- break/continue
- enumerate()
- zip()
- comprehensions
- iteration patterns

These concepts are foundational for modern Python backend development.
