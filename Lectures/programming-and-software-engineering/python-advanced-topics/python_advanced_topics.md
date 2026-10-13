<!-- omit in toc -->
# Python Advanced Topics: Concurrency, Queues, Event-Driven Architectures & Testing

<!-- omit in toc -->
## Lecture Information

| **Lecture Title**   | Python Advanced Topics: Concurrency, Queues, Event-Driven Architectures & Testing|
|---------------------|----------------------------------------------------------------------------------|
| **Author**          | Prof. Marco Picone (marco.picone@unimore.it)                                     |
| **License**         | [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/) |

> **Prerequisites:** [Python Best Practices & Object Oriented Programming](../python-best-practices-and-oop/python_best_practices_and_oop.md)
> (type hints, logging, exceptions, classes, inheritance, abstract classes, the Observer pattern).
> **Runnable code:** [`python-advanced-topics-playground`](https://github.com/Marco-Picone-Teaching/python-advanced-topics-playground) repository.

The focus of this lecture is on **software engineering and coding in Python**: the concepts and the techniques are
general and apply to any kind of application. Every concept is built **step by step**, with many small examples
and a precise explanation of *how things work*. Only the **examples** are taken from the IoT domain, and they are
all set in the same running scenario: a **Smart Building IoT gateway** that reads sensors (temperature, humidity,
CO2, presence), drives actuators (HVAC, windows, lights, valves) and talks to the cloud. The same gateway is
rebuilt across the sections: first with threads, then with a thread pool, with asyncio, with queues, as an
event-driven system, and finally covered by automated tests.

Every code snippet is **complete, commented and runnable**, and is available in the
[`python-advanced-topics-playground`](https://github.com/Marco-Picone-Teaching/python-advanced-topics-playground) repository.
The outputs shown in the notes come from actually running the examples.

This lecture is split into **six standalone files**, each covering one macro-section of the lecture
(**Section A** to **Section F**), meant to be read in order. All numbering inside each file (headings, table of
contents, cross-references) is nested under its own macro-section: every heading in
`concurrency_and_threading.md` is numbered `A.x`, every heading in `thread_pools_and_futures.md` is numbered
`B.x`, and so on.

### Section A — [`concurrency_and_threading.md`](concurrency_and_threading.md) — *Concurrency Fundamentals & Threading*

- Why concurrency matters in software: the cost of waiting for I/O
- Processes vs. threads, concurrency vs. parallelism, I/O-bound vs. CPU-bound work
- The Global Interpreter Lock (GIL): what it is and what it means in practice
- The `threading` module: creating, starting, joining threads; daemon threads; `Thread` subclasses
- Race conditions, `Lock`, `RLock`, and deadlocks
- Coordination primitives: `Event`, `Condition`, `Semaphore`, `Timer`, `threading.local`
- Graceful shutdown of worker threads
- Free-threaded Python (PEP 703, Python 3.13/3.14)
- **Worked example** (IoT: a Smart Room gateway): putting it all together in a multithreaded application

### Section B — [`thread_pools_and_futures.md`](thread_pools_and_futures.md) — *Thread Pools & Futures*

- Why one thread per task does not scale, and the thread pool idea
- `ThreadPoolExecutor`, `submit()` and the `Future` object; callbacks and cancellation
- `executor.map()` vs. `as_completed()`, and `wait()` with deadlines
- Exceptions and timeouts in futures
- Executor lifecycle, `shutdown()`, and the default number of workers (Python 3.13 change)
- `ProcessPoolExecutor` for CPU-bound analytics
- Under the hood: a pool is a queue plus workers
- **Worked example** (IoT: polling a whole building): a batch of jobs with a deadline on a bounded pool

### Section C — [`asyncio.md`](asyncio.md) — *Asyncio*

- The event loop and cooperative scheduling
- Coroutines step by step: `async def`, `await`, `asyncio.run()`
- Running coroutines concurrently: `gather()`, tasks, cancellation
- Structured concurrency with `TaskGroup` and `except*` (Python 3.11+)
- Timeouts and drift-free periodic sampling
- The number one pitfall, blocking calls, and how to bridge blocking code (`to_thread`, `run_in_executor`)
- asyncio synchronization primitives
- Choosing between threads, processes and asyncio
- **Worked example** (IoT: a Smart Room gateway): a complete asyncio application

### Section D — [`queues.md`](queues.md) — *The `queue` Module*

- Why a thread-safe blocking queue: the producer-consumer pattern
- `Queue`, `LifoQueue`, `PriorityQueue`, `SimpleQueue`, with robust priority messages
- The API: blocking, timeouts, `*_nowait`, `task_done()` / `join()`
- Backpressure with bounded queues
- Clean shutdown: sentinels and `Queue.shutdown()` (Python 3.13+)
- `collections.deque`, `asyncio.Queue`, `multiprocessing.Queue`: which queue to use
- **Worked example** (IoT: a telemetry pipeline): a multi-stage pipeline with cascading shutdown

### Section E — [`event_driven_architecture.md`](event_driven_architecture.md) — *Event-Driven Architectures*

- From polling to events: producers, consumers, event bus, pub/sub, message brokers
- The limits of the Observer pattern
- Step by step: a synchronous bus, a thread-based bus (`EventBus`, `EventHandler`, `EventBusClient`),
  and an asyncio bus
- Robustness: wildcards, unsubscribe, error isolation, graceful stop, statistics
- Experiment: threads vs. asyncio on the same workload
- Limits of an in-process bus and when to move to an external broker
- **Worked example** (IoT: a Smart Room): an event-driven application (producers → rules → executors → observers)

### Section F — [`testing.md`](testing.md) — *Testing*

- Why test, test levels, the test pyramid, Arrange-Act-Assert
- `unittest`: `TestCase`, assertions, fixtures, `subTest`, test suites and runners
- `pytest`: plain `assert`, parametrization, `approx`, fixtures, `conftest.py`, configuration
- Test doubles: stubs, fakes, mocks; `unittest.mock`, `patch()`, `monkeypatch`; dependency injection
- **Testing concurrent code:** deterministic tests for threads, thread pools, asyncio (`pytest-asyncio`,
  `IsolatedAsyncioTestCase`, `AsyncMock`), queue pipelines and the event bus of the previous sections
- Coverage and flakiness checks
