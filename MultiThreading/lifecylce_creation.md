# Java Multithreading — Day 1 & Day 2

## Day 1 — Multithreading Fundamentals

### 1. Process

A **process** is an independent program in execution.

Examples:

* Chrome
* IntelliJ IDEA
* VS Code

Characteristics:

* Has its own memory space.
* Independent from other processes.
* Communication between processes is relatively expensive.

---

### 2. Thread

A **thread** is the smallest unit of execution inside a process.

A process can contain multiple threads.

```text
Process
│
├── Thread 1
├── Thread 2
├── Thread 3
└── Thread 4
```

Threads within the same process share memory/resources.

---

### 3. Process vs Thread

| Process                                  | Thread                                  |
| ---------------------------------------- | --------------------------------------- |
| Independent program                      | Execution unit inside a process         |
| Separate memory                          | Shares process memory                   |
| Heavyweight                              | Lightweight                             |
| Expensive to create                      | Cheaper to create                       |
| Inter-process communication is expensive | Thread communication is relatively fast |

---

### 4. Why Multithreading?

Multithreading allows multiple tasks to make progress concurrently.

Benefits:

* Better CPU utilization
* Higher throughput
* Better application responsiveness
* Ability to handle multiple tasks/requests concurrently

Example backend server:

```text
Server
│
├── Thread → Request 1
├── Thread → Request 2
├── Thread → Request 3
└── Thread → Request 4
```

---

### 5. Main Thread

Every Java application starts with a **main thread**, which executes the `main()` method.

```java
public static void main(String[] args) {
    System.out.println(
        Thread.currentThread().getName()
    );
}
```

Output:

```text
main
```

---

### 6. Thread Lifecycle

Important Java thread states:

```text
NEW
 ↓
RUNNABLE
 ↓
RUNNING
 ↓
BLOCKED / WAITING / TIMED_WAITING
 ↓
TERMINATED
```

#### NEW

Thread object has been created but `start()` hasn't been called.

```java
Thread t = new Thread();
```

#### RUNNABLE

Thread has been started and is eligible to run.

```java
t.start();
```

Java's `RUNNABLE` state covers both **ready-to-run** and actually running from the JVM perspective.

#### BLOCKED

Thread is waiting to acquire a monitor lock.

#### WAITING

Thread is waiting indefinitely for another thread/action.

Examples:

```java
wait();
join();
```

#### TIMED_WAITING

Thread is waiting for a specified amount of time.

Example:

```java
Thread.sleep(1000);
```

#### TERMINATED

Thread has completed execution.

---

### 7. Context Switching

The CPU switches between threads so that multiple threads can make progress.

```text
Thread A → CPU
Thread B → CPU
Thread A → CPU
Thread C → CPU
```

This is called **context switching**.

Too much context switching can hurt performance because switching between threads has overhead.

---

### 8. User Thread vs Daemon Thread

#### User Thread

Performs normal application work.

The JVM generally waits for user threads to finish before shutting down.

#### Daemon Thread

A background/supporting thread.

The JVM does not keep running just because daemon threads are still alive.

Example:

```java
Thread t = new Thread(task);
t.setDaemon(true);
t.start();
```

Important: `setDaemon(true)` must be called **before** `start()`.

---

# Day 2 — Creating & Managing Threads

## 1. Extending `Thread`

Create a class that extends `Thread` and override `run()`.

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Thread running");
    }
}
```

Start it:

```java
MyThread t = new MyThread();
t.start();
```

### Limitation

Java supports single class inheritance, so if:

```java
class MyThread extends Thread
```

you cannot extend another class.

---

# 2. Implementing `Runnable` ⭐

`Runnable` is generally preferred because it separates the **task** from the **thread that executes it**.

```java
class MyTask implements Runnable {

    @Override
    public void run() {
        System.out.println("Task running");
    }
}
```

Usage:

```java
Runnable task = new MyTask();

Thread t = new Thread(task);

t.start();
```

### Advantages

* Can still extend another class.
* Better separation of task and execution mechanism.
* Works naturally with ExecutorService and thread pools.

---

# 3. Thread vs Runnable

| `Thread`                    | `Runnable`                 |
| --------------------------- | -------------------------- |
| Class                       | Interface                  |
| Uses inheritance            | Uses composition           |
| Cannot extend another class | Can extend another class   |
| Couples task with thread    | Separates task from thread |
| Less flexible               | Generally preferred        |

---

# 4. Callable

`Callable` is useful when a task needs to **return a result**.

```java
Callable<Integer> task = () -> {
    return 10 + 20;
};
```

A `Callable` is normally submitted to an `ExecutorService` and produces a `Future`.

```java
Future<Integer> result =
    executor.submit(task);
```

Later:

```java
Integer value = result.get();
```

### Runnable vs Callable

| Runnable                                       | Callable                              |
| ---------------------------------------------- | ------------------------------------- |
| `run()`                                        | `call()`                              |
| No return value                                | Returns a value                       |
| Cannot declare checked exceptions from `run()` | `call()` can throw checked exceptions |
| Common for tasks without results               | Useful when a result is needed        |

---

# 5. `start()` vs `run()` ⭐⭐⭐⭐⭐

This is a common interview question.

### `start()`

```java
Thread t = new Thread(task);
t.start();
```

`start()` asks the JVM to start a **new thread**, which will execute `run()`.

### `run()`

```java
t.run();
```

This is just a **normal method call**.

It does NOT create a new thread.

---

### Comparison

```text
t.start()
   ↓
New thread
   ↓
run()
```

Whereas:

```text
t.run()
   ↓
Current thread executes run()
```

**Remember:**

> `start()` starts a new thread; `run()` is just the task method.

---

# 6. `sleep()`

Pauses the **currently executing thread** for a specified duration.

```java
Thread.sleep(2000);
```

The thread enters:

```text
TIMED_WAITING
```

for approximately 2 seconds.

Important:

* `sleep()` is a static method of `Thread`.
* It does **not release a lock/monitor** that the thread already holds.
* It can throw `InterruptedException`.

---

# 7. `join()`

Makes the current thread wait for another thread to finish.

```java
Thread t = new Thread(task);

t.start();

t.join();
```

If `main` executes `t.join()`:

```text
main
 │
 ├── starts Thread t
 │
 ├── waits at join()
 │
 │       Thread t runs
 │       Thread t finishes
 │
 └── main continues
```

Useful when one task must finish before another task continues.

---

# 8. `yield()`

```java
Thread.yield();
```

Provides a **hint to the scheduler** that the current thread is willing to give other runnable threads an opportunity to execute.

Important:

> `yield()` is only a hint. The JVM scheduler may ignore it.

Do not use it for synchronization.

---

# 9. `interrupt()`

`interrupt()` is a way to **request that a thread stop what it is doing or respond to interruption**.

```java
Thread t = new Thread(task);

t.start();

t.interrupt();
```

If the thread is sleeping/waiting/joining, it can receive:

```text
InterruptedException
```

Example:

```java
try {
    Thread.sleep(5000);
} catch (InterruptedException e) {
    Thread.currentThread().interrupt();
}
```

Best practice is usually to **preserve the interrupt status** when catching `InterruptedException` unless you intentionally handle the interruption completely.

---

# 10. Important Thread Methods

| Method        | Purpose                  |
| ------------- | ------------------------ |
| `start()`     | Starts a new thread      |
| `run()`       | Contains the task logic  |
| `sleep()`     | Pauses current thread    |
| `join()`      | Waits for another thread |
| `yield()`     | Gives scheduler a hint   |
| `interrupt()` | Requests interruption    |

---

# Common Interview Questions

### Can we call `start()` twice?

No.

```java
t.start();
t.start(); // ❌ IllegalThreadStateException
```

A thread can only be started once.

---

### Does `run()` create a new thread?

No.

```java
t.run();
```

is a normal method call.

---

### Does `sleep()` release a lock?

No.

`sleep()` pauses the thread but does not release monitors it owns.

---

### Does `join()` create a new thread?

No.

It makes the current thread wait for another thread.

---

### Can `yield()` guarantee another thread will run?

No.

It is only a scheduling hint.

---

### When should I use Runnable instead of extending Thread?

Usually when you want to represent a task independently from the thread executing it. This also fits naturally with `ExecutorService` and thread pools.

---

# Day 1 + Day 2 — Quick Memory Sheet

```text
PROCESS
↓
Independent program

THREAD
↓
Smallest execution unit inside a process

MULTITHREADING
↓
Multiple threads making progress concurrently

LIFECYCLE
↓
NEW
→ RUNNABLE
→ BLOCKED / WAITING / TIMED_WAITING
→ TERMINATED

THREAD CREATION
↓
Thread
Runnable ⭐
Callable → result

start()
↓
Creates/starts a new thread

run()
↓
Normal method call

sleep()
↓
Pause current thread
Does NOT release monitor

join()
↓
Wait for another thread

yield()
↓
Scheduler hint

interrupt()
↓
Request interruption

USER THREAD
↓
Keeps JVM alive

DAEMON THREAD
↓
Background thread
JVM doesn't wait for it
```

## Most Important Interview Points

1. **`start()` vs `run()`**
2. **`Runnable` vs `Thread`**
3. **`Runnable` vs `Callable`**
4. **`sleep()` vs `wait()`** — especially that `sleep()` doesn't release a monitor while `wait()` does.
5. **Thread lifecycle**
6. **User vs daemon threads**
7. **`join()` and `interrupt()`**
8. **Why manually creating threads doesn't scale** → leads to `ExecutorService` and thread pools.
