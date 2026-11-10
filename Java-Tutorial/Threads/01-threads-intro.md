# Java Threads — Complete Notes

## 1. What is a Thread?

A **thread is the smallest unit of execution within a process**. It represents an independent path of execution inside a program.

Java supports multithreading, which allows multiple threads to execute within a single process.

### Why do we need threads?

Consider a music application:

- One task plays music.
- Another loads album artwork.
- Another loads the playlist.
- Another checks notifications.

Without multithreading, tasks may have to wait for one another unnecessarily.

With multithreading, multiple tasks can make progress concurrently.

### Benefits of Multithreading

1. Better application responsiveness.
2. Efficient CPU utilization.
3. Concurrent execution of tasks.
4. Background processing.
5. Handling multiple client requests.

---

## 2. Process vs Thread

| Process | Thread |
|---|---|
| An executing program | Unit of execution inside a process |
| Has its own memory space | Shares process memory |
| More resource-intensive | Generally lightweight |
| Processes are isolated | Threads can communicate through shared memory |
| Has its own resources | Shares many process resources |

### Memory sharing

Threads within the same process share:
- Heap memory.
- Objects and instance data accessible through references.
- Process resources.

Each thread has its own:
- Stack memory.
- Local variables.
- Method call frames.
- Execution state.

**Interview point:** Threads share heap memory but have separate stacks.

---

## 3. How does a Java program execute?

Consider:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```

Execution steps:

1. JVM starts the Java application.
2. JVM creates the main thread.
3. Main thread executes the `main()` method.
4. The statement gets printed.
5. Main thread completes execution.

**Important:** The main() method is not a thread. It is a method executed by the main thread.

---

## 4. Java Thread Class

Java provides the built-in `Thread` class from the `java.lang` package.

It provides methods to create, start and manage threads.

### Creating a Thread Object

```java
Thread t = new Thread();
```

This creates a Thread object but does not start a new thread of execution.

To start it:

```java
t.start();
```

A task must be supplied to perform useful work.

---

## 5. Ways to Create Threads in Java

Two commonly taught approaches are:

### Approach 1: Extending the Thread Class

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Thread is running");
    }
}

public class Main {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.start();
    }
}
```

**Explanation:**

- `MyThread` inherits the Thread class.
- `run()` contains the task to execute.
- `start()` starts a new thread.
- The JVM arranges for `run()` to execute on that thread.

### Approach 2: Implementing Runnable Interface

```java
class MyTask implements Runnable {

    @Override
    public void run() {
        System.out.println("Runnable task running");
    }
}

public class Main {

    public static void main(String[] args) {

        MyTask task = new MyTask();

        Thread t = new Thread(task);

        t.start();
    }
}
```

**Explanation:**

- `Runnable` represents a task that can be executed by a thread.
- `MyTask` implements Runnable.
- `run()` defines the task.
- The task is passed to the Thread constructor.
- `start()` starts a new thread to execute the task.

### Runnable with Lambda Expression

Runnable is a functional interface because it has one abstract method: `run()`.

Therefore, we can use a lambda expression.

```java
public class Main {

    public static void main(String[] args) {

        Runnable task = () -> {
            System.out.println("Task running");
        };

        Thread t = new Thread(task);

        t.start();
    }
}
```

---

## 6. start() vs run()

One of the most important Java multithreading interview questions.

### start()

- Initiates a new thread of execution.
- JVM schedules the new thread.
- The new thread executes its `run()` method.
- Can be called only once on a particular Thread object.

### run()

- Contains the task executed by a thread.
- Calling it directly is like calling any normal method.
- Does not create a new thread when called directly.

Example:

```java
class MyThread extends Thread {

    public void run() {
        System.out.println(
            Thread.currentThread().getName()
        );
    }
}

public class Main {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.run();   // Normal method call

        t.start(); // Starts a new thread
    }
}
```

Possible output:

```text
main
Thread-0
```

### Comparison

| start() | run() |
|---|---|
| Starts a new thread | Normal method call when invoked directly |
| Creates a new execution path | Does not create a new execution path |
| JVM schedules execution | Executes on the calling thread |
| Can be called once per Thread object | Can be called directly multiple times |

**Remember:** Calling `start()` twice on the same Thread object throws `IllegalThreadStateException`.

---

## 7. Main Thread

The main thread is the first application thread that executes the `main()` method.

Example:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println(
            Thread.currentThread().getName()
        );
    }
}
```

Output:

```text
main
```

### Important Methods

**currentThread()**

Returns the currently executing thread.

```java
Thread.currentThread();
```

**getName()**

Returns the name of the thread.

```java
Thread.currentThread().getName();
```

**setName()**

Changes the thread name.

```java
Thread.currentThread().setName("MainWorker");
```

---

## 8. Important Thread Methods

| Method | Description |
|---|---|
| `start()` | Starts a new thread |
| `run()` | Defines the task |
| `sleep()` | Pauses the current thread |
| `join()` | Waits for another thread to terminate |
| `currentThread()` | Returns the currently executing thread |
| `getName()` | Returns thread name |
| `setName()` | Changes thread name |
| `getPriority()` | Gets thread priority |
| `setPriority()` | Sets thread priority |
| `isAlive()` | Checks whether a thread has started and not yet terminated |
| `interrupt()` | Interrupts a thread |

---

## 9. Thread Naming

We can assign meaningful names to threads.

```java
public class Main {

    public static void main(String[] args) {

        Thread t = new Thread(() -> {
            System.out.println("Downloading file");
        });

        t.setName("DownloadThread");

        t.start();
    }
}
```

Output:

```text
Downloading file
```

To print the thread name:

```java
System.out.println(
    Thread.currentThread().getName()
);
```

Meaningful thread names make debugging easier.

---

## 10. Multiple Threads

A Java process can have multiple threads executing concurrently.

Example:

```java
public class Main {

    public static void main(String[] args) {

        Thread t1 = new Thread(() ->
            System.out.println("Task 1")
        );

        Thread t2 = new Thread(() ->
            System.out.println("Task 2")
        );

        Thread t3 = new Thread(() ->
            System.out.println("Task 3")
        );

        t1.start();
        t2.start();
        t3.start();
    }
}
```

### Important: Output order

The output order is not guaranteed.

Possible output:

```text
Task 2
Task 1
Task 3
```

Or:

```text
Task 3
Task 2
Task 1
```

This happens because thread execution is controlled by the thread scheduler.

---

## 11. Thread Scheduler

The thread scheduler determines which eligible thread gets CPU execution time.

Important points:

- Multiple threads can be ready to execute.
- Scheduler determines which thread gets CPU time.
- Execution order is not guaranteed.
- A thread started first does not necessarily finish first.
- Actual scheduling depends on the JVM and underlying operating system.

---

## 12. Concurrency vs Parallelism

| Concurrency | Parallelism |
|---|---|
| Multiple tasks make progress during overlapping periods | Multiple tasks execute simultaneously |
| Tasks may take turns using CPU time | Tasks execute on multiple processing units |
| Can happen on one CPU core | Requires multiple execution resources |
| Focuses on managing multiple tasks | Focuses on simultaneous execution |

Example:

- Concurrency: Two tasks take turns using one CPU core.
- Parallelism: Two tasks execute simultaneously on two CPU cores.

Java supports concurrent execution through multithreading. Actual parallel execution depends on available hardware and scheduling.

---

## 13. join() Method

The `join()` method makes the calling thread wait until the specified thread terminates.

### Why do we need join()?

Suppose three threads perform three tasks.

We want to print:

`All tasks completed`

Only after all three threads finish.

We can use `join()`.

### Example

```java
public class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Thread t1 = new Thread(() -> {
            System.out.println("Task 1 completed");
        });

        Thread t2 = new Thread(() -> {
            System.out.println("Task 2 completed");
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("All tasks completed");
    }
}
```

### Execution

1. Main thread starts t1.
2. Main thread starts t2.
3. Main thread calls t1.join().
4. Main waits until t1 terminates.
5. Main waits for t2 to terminate.
6. Main prints the final message.

### Different join() methods

| Method | Description |
|---|---|
| `join()` | Waits until thread terminates |
| `join(long millis)` | Waits up to specified milliseconds |
| `join(long millis, int nanos)` | Waits up to specified milliseconds and nanoseconds |

`join()` methods can throw `InterruptedException`.

**Important:** Calling join() does not stop the target thread. It makes the calling thread wait.

---

## 14. Common Interview Questions

**Q1. What is a thread?**

A thread is the smallest unit of execution within a process. It represents an independent execution path.

**Q2. Why do we need multithreading?**

To execute multiple tasks concurrently, improve application responsiveness and make better use of CPU resources.

**Q3. What is the difference between a process and a thread?**

A process is an executing program with its own memory space. A thread is an execution unit within a process. Threads share process memory but have separate stacks.

**Q4. What is the main thread?**

The thread that executes the main() method when a Java application starts.

**Q5. What is the difference between start() and run()?**

start() initiates a new thread. Directly calling run() executes it as a normal method on the current thread.

**Q6. Can we call start() twice?**

No. Calling start() twice on the same Thread object throws IllegalThreadStateException.

**Q7. Can multiple threads execute the same method?**

Yes. Multiple threads can execute the same method. However, accessing shared mutable data may require synchronization.

**Q8. Do threads share memory?**

Yes. Threads in the same process share heap memory but maintain separate stacks.

**Q9. What is a thread scheduler?**

It determines which eligible thread gets CPU execution time.

**Q10. Is thread execution order guaranteed?**

No. The execution order depends on thread scheduling.

**Q11. What is join()?**

join() makes the calling thread wait until another thread terminates.

**Q12. What happens if the main thread completes before child threads?**

Normal non-daemon child threads can continue running. The JVM does not terminate while non-daemon threads are still alive.

**Q13. What is concurrency?**

Concurrency means multiple tasks make progress during overlapping periods.

**Q14. What is parallelism?**

Parallelism means multiple tasks execute at exactly the same time on separate processing resources.

---

## 15. Common Mistakes

| Mistake | Correct understanding |
|---|---|
| Calling run() to create a thread | Use start() |
| Calling start() twice | A Thread object can be started only once |
| Assuming start order equals execution order | Execution order is not guaranteed |
| Assuming join() stops a thread | join() makes the calling thread wait |
| Assuming all threads have separate heap memory | Threads share process heap memory |
| Assuming main() is a thread | main() is a method executed by the main thread |
| Assuming multithreading always improves performance | Thread management has overhead |

---

## 16. Quick Revision — One-Minute Summary

- Thread = Independent path of execution inside a process.
- Process = Executing program.
- Main thread = Executes main().
- Thread class = Built-in Java class for thread management.
- Runnable = Interface representing a task.
- start() = Initiates a new thread.
- run() = Contains the task; direct invocation is a normal method call.
- join() = Makes the calling thread wait.
- sleep() = Pauses the current thread.
- Thread scheduler = Determines CPU execution scheduling.
- Concurrency = Multiple tasks make progress over overlapping periods.
- Parallelism = Multiple tasks execute simultaneously.
- Heap = Shared among threads of the same process.
- Stack = Separate for each thread.
- start() called twice = IllegalThreadStateException.

---

## 17. Important Coding Practice

1. Create a thread by extending Thread.
2. Create a thread by implementing Runnable.
3. Create a thread using a lambda expression.
4. Create three threads and observe output order.
5. Print the current thread name.
6. Change a thread's name.
7. Use join() to wait for multiple threads.
8. Compare direct run() invocation with start().

## 18. Topics to Connect Next

- Thread Priority and Sleep
- Runnable vs Thread
- Race Conditions
- Synchronization and Thread Safety
- Thread Lifecycle and States
