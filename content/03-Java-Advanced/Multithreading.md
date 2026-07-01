---
type: concept
tags:
  - java/advanced
  - java/concurrency
difficulty: hard
pattern: ""
related:
  - "[[static-Keyword]]"
  - "[[JVM-and-Memory]]"
  - "[[final-Keyword]]"
  - "[[Object-Class]]"
aliases:
  - Threads
  - Concurrency
  - ExecutorService
---

# Multithreading

> [!note] Definition
> A **thread** is an independent execution path sharing the same heap. Java threads map to OS threads (and virtual threads since Java 21). Concurrency shares state; you must **synchronize** to keep data consistent.

## Creating a thread
```java
// 1. Runnable
Thread t = new Thread(() -> System.out.println("run " + Thread.currentThread()));
t.start();          // schedules; don't call run() directly
t.join();           // wait for completion

// 2. subclass (less flexible)
class W extends Thread { public void run(){ /*...*/ } }
new W().start();
```

## The hazard: shared mutable state
```java
int counter = 0;
Runnable inc = () -> { for (int i=0;i<10000;i++) counter++; };
// two threads running inc -> counter < 20000 (lost updates; ++ is not atomic)
```

## Synchronization primitives
- `synchronized` block/method — mutual exclusion on a monitor:
```java
synchronized (lock) { counter++; }
public synchronized void m(){ ... }   // locks `this`
```
- `volatile` — guarantees reads/writes go to main memory + prevents reordering; **not** atomicity. Good for a status flag, not `count++`.
- `java.util.concurrent.atomic` — lock-free atomics: `AtomicInteger`, `AtomicLong`, `AtomicReference`.
```java
AtomicInteger ai = new AtomicInteger();
ai.incrementAndGet(); ai.compareAndSet(0, 1);
```

## High-level: `ExecutorService` (preferred)
```java
ExecutorService pool = Executors.newFixedThreadPool(4);
Future<Integer> f = pool.submit(() -> compute());
int result = f.get();              // blocks
pool.shutdown();
```
Don't hand-manage threads in production — use executors.

## Concurrency utilities (`java.util.concurrent`)
- `ReentrantLock` / `ReadWriteLock` — finer-grained than `synchronized`, supports `tryLock`, timed lock.
- `Semaphore`, `CountDownLatch`, `CyclicBarrier`, `Phaser` — coordination.
- `ConcurrentHashMap` — thread-safe, highly concurrent map (lock per bucket).
- `BlockingQueue` (`LinkedBlockingQueue`, `ArrayBlockingQueue`) — producer/consumer.
- `CompletableFuture` — composable async pipelines.

## Memory visibility (JMM)
Without synchronization, one thread's writes may be invisible to another (caches, reordering). `volatile`, `synchronized`, `final` field initialization, and concurrent collections all create **happens-before** guarantees.

> [!tip] DSA rarely needs threads, but concurrency fundamentals matter for:
> - Real systems & interviews about design.
> - `ConcurrentHashMap` for thread-safe frequency maps.
> - "Producer/consumer" and "readers/writers" are classic interview problems solved with `BlockingQueue`/`Semaphore`.

> [!warning] Pitfalls
> - Calling `run()` instead of `start()` runs on the current thread (no concurrency).
> - `synchronized` on a non-`final` field that gets reassigned → you lock different objects → no protection.
> - Deadlock: lock A then B in one thread, B then A in another. Always acquire locks in a fixed global order.
> - Holding a lock during a long/blocking call kills throughput.
> - `Thread.stop`/`suspend`/`resume` are deprecated and unsafe — interrupt cooperatively: `t.interrupt();` check `Thread.interrupted()`.

## Related
- [[static-Keyword]] · [[JVM-and-Memory]] · [[final-Keyword]] (safe publication) · [[Object-Class]] (`wait/notify`)
