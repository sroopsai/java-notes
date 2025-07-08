# Chapter 13: Introducing Threads

## ✅ Summary

### What Is a Thread?
- A **thread** is the smallest unit of execution that the OS can schedule.
- A **process** is a group of threads sharing memory.
- **Single-threaded** process: only one thread.
- **Multithreaded** process: multiple threads in shared environment.

### Types of Threads in Java
- **Platform thread**: OS-level thread.
- **System thread**: Created by JVM (e.g., garbage collector).
- **User-defined thread**: Created by the developer.
- **Virtual thread**: Lightweight, managed by carrier threads.

### Tasks and Threads
- A **task** is one unit of work a thread executes.
- A thread executes only one task at a time.

### Virtual vs. Platform Threads
- **Platform threads** are resource-intensive, mapped one-to-one with OS threads.
- **Virtual threads** are more scalable, ideal for I/O-bound tasks.
- Managed by **carrier threads**, not tied to OS threads.
- JVM treats carrier threads as a special kind of platform thread.
- Virtual threads are always daemon threads.

### Thread Scheduling and Concurrency
- **Concurrency**: Multiple threads/processes running seemingly at the same time.
- **Scheduler**: Determines which thread runs when.
- Uses algorithms like **round-robin** or considers **priority**.
- **Context switch**: Saving/restoring a thread’s state when switching execution.

### 🔄 Thread Lifecycle and State Transitions

A thread in Java can exist in one of six states. You can check the current state using `getState()`.

#### ✅ Thread States

- **NEW**: The thread has been created but not yet started (`start()` has not been called).
- **RUNNABLE**: The thread is eligible to run but might be waiting for CPU time.
- **BLOCKED**: The thread is waiting to acquire a monitor lock to enter a synchronized block or method.
- **WAITING**: The thread is waiting indefinitely for another thread to perform a specific action.
- **TIMED_WAITING**: The thread is waiting for a specified amount of time (e.g., using `Thread.sleep()`).
- **TERMINATED**: The thread has completed execution or was terminated due to an exception.

#### 🔁 Common Transitions Between States

- `NEW` → `RUNNABLE`: When `start()` is called on the thread.
- `RUNNABLE` → `BLOCKED`: When trying to enter a synchronized block already locked by another thread.
- `RUNNABLE` → `WAITING`: When calling `join()` or `wait()` with no timeout.
- `RUNNABLE` → `TIMED_WAITING`: When calling `sleep()` or `join(timeout)`.
- `BLOCKED` / `WAITING` / `TIMED_WAITING` → `RUNNABLE`: When the thread is notified, interrupted, or timeout expires.
- Any state → `TERMINATED`: When the thread finishes execution or is stopped due to an error.

#### 🛠 Examples of Transitions

- `Thread.sleep(1000)` → moves to `TIMED_WAITING`, then back to `RUNNABLE`.
- `thread.join()` → moves to `WAITING` until the target thread finishes.
- `interrupt()` on a `WAITING` thread → moves it back to `RUNNABLE`.
- Thread completes `run()` → moves to `TERMINATED`.

> ⚠️ Trying to restart a terminated thread will cause an `IllegalThreadStateException`.

### Creating Threads (Java 21+)
- Use factories: `Thread.ofPlatform()` or `Thread.ofVirtual()`.
- `start()` begins execution. `unstarted()` defers it.
- Avoid calling `run()`—it runs synchronously in the current thread.
- Priorities (1–10) can be set on platform threads only.

### Daemon Threads
- **Daemon threads**: JVM exits if only these are left.
- **Virtual threads** are always daemons.
- **Platform threads** are non-daemon by default but can be set.

### Thread Utilities
- Use `join()` to wait for a thread to finish.
- Use `interrupt()` to signal a thread to stop waiting or sleeping.
- Use `Thread.interrupted()` to check if a thread was interrupted.

### Terminology Reference

| Term             | Description |
|------------------|-------------|
| **Carrier thread** | Runs virtual threads on OS thread |
| **Daemon thread**  | JVM can exit even if this is running |
| **Platform thread**| Mapped 1:1 to OS thread |
| **System thread**  | JVM-created (e.g., GC) |
| **User thread**    | Developer-created |
| **Virtual thread** | Lightweight, scalable |
| **Task**           | Unit of work |
| **Process**        | Group of threads sharing memory |

---

## ❓ Theoretical Questions

1. What is the difference between a process and a thread?
2. How do virtual threads improve scalability?
3. What role do carrier threads play in virtual threading?
4. What’s the consequence of calling `.run()` instead of `.start()` on a thread?
5. Explain the purpose of thread priorities and their limitations.
6. Describe each thread state and give an example of when it occurs.
7. What is a context switch and why is it expensive?
8. Under what conditions does the JVM shut down in a multithreaded program?
9. Why is `interrupt()` useful? How does it behave differently based on thread state?
10. How does the thread scheduler decide which thread to run?

---

## 💡 Practical Coding Questions

1. Create a program with a virtual thread and a platform thread. Print messages from both. What outputs are possible?
2. Write a program using `Thread.sleep()` in a virtual thread. Compare performance to a platform thread.
3. Try running 100,000 virtual threads vs platform threads. What happens?
4. Call `.run()` instead of `.start()` on a thread. What changes?
5. Use `join()` to wait for thread completion. What happens if you remove it?
6. Track thread states using `getState()` as they move from NEW to TERMINATED.
7. Simulate a context switch using `Thread.sleep()` in a loop. Observe interleaving outputs.
8. Implement a loop that checks for `Thread.interrupted()` and exits cleanly when interrupted.
9. Mark a platform thread as daemon. Does the JVM wait for it?
10. Create a platform thread and interrupt it while it’s sleeping. What’s the result?

---
