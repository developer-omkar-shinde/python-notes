# Python Basics — Topic 9: Asyncio

Asyncio is one of the MOST IMPORTANT topics for modern Python backend development.

FastAPI heavily relies on:
- async programming
- event loops
- coroutines
- non-blocking I/O

Understanding asyncio deeply is ESSENTIAL for:
- scalable APIs
- concurrency
- performance
- WebSockets
- streaming
- background tasks

If you already know:
- JavaScript async/await
- event loop concepts

you will learn asyncio much faster.

---

# What Problem Does Asyncio Solve?

Traditional synchronous code blocks execution.

Example:

```python
import time

def task():
    time.sleep(3)

    print("Done")

task()
```

Program waits entire 3 seconds.

This blocks:
- server
- request handling
- scalability

Bad for backend APIs.

---

# Concurrency vs Parallelism

VERY IMPORTANT.

---

# Concurrency

Handling multiple tasks efficiently.

Tasks take turns.

Single thread can manage many I/O tasks.

Asyncio focuses on:
- concurrency

---

# Parallelism

Tasks truly run simultaneously.

Usually:
- multiple CPU cores
- multiprocessing

Different concept.

---

# Why Async Helps Backend APIs

Backend servers spend huge time waiting for:
- database
- network
- file I/O
- external APIs

Instead of blocking:
Python can switch to other tasks.

This improves scalability massively.

---

# Coroutine

Coroutine is async function.

Defined using:

```python
async def
```

Example:

```python
async def greet():
    print("Hello")
```

Calling coroutine does NOT execute immediately.

---

# Await

Use:
- await

to pause coroutine until async task finishes.

Example:

```python
import asyncio

async def greet():
    await asyncio.sleep(1)

    print("Hello")
```

---

# Running Async Code

Need event loop.

Example:

```python
asyncio.run(greet())
```

Output after 1 second:

```python
Hello
```

---

# asyncio.sleep vs time.sleep

VERY IMPORTANT.

---

# time.sleep()

Blocks thread completely.

---

# asyncio.sleep()

Non-blocking.

Allows other tasks to run.

Critical difference.

---

# Multiple Async Tasks

```python
import asyncio

async def task1():
    await asyncio.sleep(2)

    print("Task 1 done")

async def task2():
    await asyncio.sleep(1)

    print("Task 2 done")
```

---

# Concurrent Execution

```python
async def main():
    await asyncio.gather(
        task1(),
        task2()
    )

asyncio.run(main())
```

Output:

```python
Task 2 done
Task 1 done
```

Both tasks run concurrently.

---

# Event Loop

Heart of asyncio.

Event loop:
- manages tasks
- schedules coroutines
- switches execution

Similar to:
- JavaScript event loop

---

# Important Mental Model

When async task waits:

```python
await asyncio.sleep(1)
```

Event loop says:

> "While waiting, run something else."

This is core async concept.

---

# Blocking vs Non-Blocking

---

# Blocking Example

```python
import time

time.sleep(5)
```

Everything stops.

---

# Non-Blocking Example

```python
await asyncio.sleep(5)
```

Other tasks continue running.

---

# Async Functions Return Coroutine Objects

```python
async def test():
    return 1

result = test()

print(result)
```

Output:

```python
<coroutine object ...>
```

Need:
- await
or
- asyncio.run()

---

# Awaiting Coroutines

```python
async def test():
    return 1

async def main():
    value = await test()

    print(value)

asyncio.run(main())
```

---

# create_task()

Schedules task independently.

```python
async def worker():
    await asyncio.sleep(2)

    print("Done")
```

```python
async def main():
    task = asyncio.create_task(worker())

    print("Working...")

    await task
```

---

# gather()

Run multiple coroutines concurrently.

```python
await asyncio.gather(
    task1(),
    task2(),
    task3()
)
```

Very common in backend APIs.

---

# Real Backend Example

Instead of:

```python
user = get_user()
posts = get_posts()
comments = get_comments()
```

Sequential.

Use:

```python
await asyncio.gather(
    get_user(),
    get_posts(),
    get_comments()
)
```

Much faster.

---

# CPU-Bound vs I/O-Bound

VERY IMPORTANT.

---

# I/O-Bound

Waiting tasks:
- APIs
- DB
- files
- network

Asyncio GREAT for this.

---

# CPU-Bound

Heavy calculations:
- image processing
- ML
- video encoding

Asyncio NOT ideal.

Use:
- multiprocessing
- worker queues

---

# Async Iterators

Advanced topic.

```python
class AsyncCounter:
    async def __aiter__(self):
        return self
```

Used in:
- streaming
- WebSockets

---

# Async Generators

```python
async def generator():
    yield 1
```

Very useful for streaming responses.

---

# Async Context Managers

```python
async with session:
    pass
```

Used heavily in:
- DB connections
- HTTP clients

---

# Common Async Libraries

Backend ecosystem uses:
- FastAPI
- aiohttp
- httpx
- asyncpg
- aioredis

All built around asyncio.

---

# Important FastAPI Concept

FastAPI route handlers often use:

```python
@app.get("/")
async def home():
    return {"message": "Hello"}
```

This enables:
- high concurrency
- scalable APIs

Without blocking server.

---

# When NOT to Use Async

Do NOT use async for:
- simple scripts
- CPU-heavy work
- unnecessary complexity

Async is mainly useful for:
- I/O-heavy backend systems

---

# Common Beginner Mistake

FORGETTING await.

BAD:

```python
result = fetch_data()
```

Correct:

```python
result = await fetch_data()
```

---

# Common Asyncio Terms

---

## Coroutine
Async function.

---

## Event Loop
Schedules async tasks.

---

## Task
Running coroutine.

---

## Future
Placeholder for future result.

---

## Awaitable
Object usable with await.

---

# Backend Relevance

Asyncio powers:
- FastAPI
- WebSockets
- streaming APIs
- concurrent requests
- background jobs
- scalable microservices

Modern Python backend development heavily relies on asyncio.

---

# Mini Exercises

## 1. What’s the difference?

```python
time.sleep(2)
```

vs

```python
await asyncio.sleep(2)
```

---

## 2. What’s the output order?

```python
async def a():
    await asyncio.sleep(2)

    print("A")

async def b():
    await asyncio.sleep(1)

    print("B")
```

Using:

```python
await asyncio.gather(a(), b())
```

---

## 3. Why is async useful for APIs?

Think about:
- waiting time
- concurrency
- scalability

---

# Important Takeaways

You MUST deeply understand:
- async/await
- event loop
- coroutines
- non-blocking I/O
- gather()
- create_task()
- concurrency vs parallelism

These concepts are foundational for modern Python backend engineering and FastAPI.
