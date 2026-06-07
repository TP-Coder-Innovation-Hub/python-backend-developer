# Concurrency Model

`[Mid]`

Python gives you three main concurrency models. Choosing the right one is a critical backend engineering decision.

## The GIL (Global Interpreter Lock)

`[Mid]`

The GIL is a mutex that prevents multiple threads from executing Python bytecode simultaneously. Only one thread runs Python code at a time, even on a multi-core machine.

Why it exists: CPython's memory management uses reference counting, which is not thread-safe. The GIL is the simplest way to make reference counting safe.

| Scenario | GIL Impact |
|----------|-----------|
| CPU-bound Python code in threads | No speedup. Threads take turns on a single core. |
| I/O-bound code in threads (network, file, DB) | Minimal impact. GIL is released during I/O waits. |
| CPU-bound code via `multiprocessing` | Full multi-core utilization. Each process has its own GIL. |
| C extensions that release the GIL (NumPy) | Can run in parallel. The extension releases the GIL during computation. |

The GIL does not mean "Python is single-threaded." I/O operations release it. C extensions release it. It only blocks CPU-bound Python code in threads.

## Threading

Threads share memory and are managed by the OS. The GIL means only one thread runs Python bytecode at a time, but the GIL is released during I/O.

```python
import threading
import urllib.request

def fetch_url(url):
    return urllib.request.urlopen(url).read()

urls = ["https://example.com", "https://python.org"]
threads = []

for url in urls:
    t = threading.Thread(target=fetch_url, args=(url,))
    t.start()
    threads.append(t)

for t in threads:
    t.join()  # wait for all threads to finish
```

**Use when:** I/O-bound work with synchronous libraries, simple parallelism for a handful of concurrent operations.

**Don't use when:** CPU-bound work (no speedup) or high concurrency (use asyncio).

## Multiprocessing

Each process has its own Python interpreter and its own GIL. True parallelism for CPU-bound work.

```python
from multiprocessing import Pool

def process_data(item):
    return item * item  # CPU-bound work

with Pool(processes=4) as pool:
    results = pool.map(process_data, range(100))
```

**Use when:** CPU-bound workloads that need multi-core parallelism -- data processing, ML inference, image transformation.

**Don't use when:** You need shared memory or low overhead (spawning processes is expensive).

## Asyncio

Single-threaded, event-loop-based concurrency. Coroutines yield control at `await` points, letting the event loop handle other work while waiting for I/O.

```python
import asyncio

async def fetch_url(url):
    # simulate an HTTP request
    await asyncio.sleep(1)  # yields control for 1 second
    return f"Response from {url}"

async def main():
    # Run 3 requests concurrently -- total time ~1 second, not 3
    results = await asyncio.gather(
        fetch_url("https://example.com"),
        fetch_url("https://python.org"),
        fetch_url("https://github.com"),
    )
    print(results)

asyncio.run(main())
```

Step by step:
1. `asyncio.gather` starts all three coroutines.
2. The first `await asyncio.sleep(1)` suspends that coroutine and yields to the event loop.
3. The event loop starts the second coroutine, which also suspends at `await`.
4. Same for the third.
5. After 1 second, all three resume and return their results.

**Use when:** High-concurrency I/O-bound workloads, API servers handling thousands of connections.

**Don't use when:** Significant CPU-bound work in the event loop (blocks everything) or your dependencies are all synchronous.

## TaskGroup (Python 3.11+)

`[Senior]`

Structured concurrency -- all tasks complete or fail together:

```python
async def fetch_all(urls):
    results = {}

    async with asyncio.TaskGroup() as tg:
        for url in urls:
            tg.create_task(fetch_one(url, results))

    return results  # all tasks done by this point
```

If any task raises an exception, all other tasks are cancelled automatically. Safer than `asyncio.gather`.

## Decision Summary

| Workload | Concurrency Model |
|----------|-------------------|
| I/O-bound, low concurrency (<100) | Threading |
| I/O-bound, high concurrency (100+) | asyncio |
| CPU-bound | multiprocessing |
| Mixed I/O + CPU | asyncio + ProcessPoolExecutor |

The critical mistake: using threading for CPU-bound work. The GIL prevents any speedup. Use multiprocessing instead.
