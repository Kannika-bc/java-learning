# Race Condition, Mutation & Thread Safety in Java

## 1. Mutation

Mutation means changing the state or value of an object after it has been created.

```java
class Counter {
    int count = 0;

    void increment() {
        count++;
    }
}
```

Here, `count++` changes the object's state. This is called mutation.

### Mutable vs Immutable

| Mutable | Immutable |
|---|---|
| State can be changed | State cannot be changed after creation |
| Example: ArrayList | Example: String |
| Example: StringBuilder | Changes require creating a new object |

**Why is mutation important in multithreading?**

When multiple threads modify the same shared mutable object, unexpected results can occur.

---

## 2. Race Condition

A race condition occurs when multiple threads access and modify shared data concurrently, and the final result depends on the order in which they execute.

### Example

```java
class Counter {
    int count = 0;

    void increment() {
        count++;
    }
}
```

Suppose two threads call `increment()` simultaneously.

Expected result:

```text
Initial count = 0

Thread 1 → increment()
Thread 2 → increment()

Expected count = 2
```

But `count++` is not an atomic operation. Internally, it involves:

1. Read the current value.
2. Increment the value.
3. Write the updated value.

### Possible execution

```text
Thread 1 reads count = 0
Thread 2 reads count = 0

Thread 1 increments to 1
Thread 2 increments to 1

Thread 1 writes 1
Thread 2 writes 1

Final count = 1
```

We expected 2 but got 1.

This is called a **race condition**.

---

## 3. Thread Safety

Thread safety means that code behaves correctly even when multiple threads access it simultaneously.

A thread-safe program protects shared resources from unexpected concurrent modifications.

For example, the previous Counter class is not thread-safe because multiple threads can modify `count` simultaneously.

We can use synchronization to make it thread-safe.

---

## 4. synchronized Keyword

Java provides the `synchronized` keyword to control access to shared resources.

It ensures that only one thread at a time can execute a synchronized section protected by the same object's monitor lock.

### Example

```java
class Counter {
    int count = 0;

    synchronized void increment() {
        count++;
    }
}
```

### How it works

```text
Thread 1 enters increment()
Thread 2 waits for the lock

Thread 1 completes → count = 1
Thread 1 releases the lock

Thread 2 acquires the lock
Thread 2 completes → count = 2
```

Now the counter is thread-safe when accessed through this synchronized method on the same Counter object.

### How synchronized works

- A thread must acquire the object's monitor lock before entering a synchronized instance method.
- Other threads trying to acquire the same lock must wait.
- When the synchronized method or block exits, the lock is released.
- Synchronization also provides memory visibility guarantees.

### Types of synchronization

**1. Synchronized Method**

```java
synchronized void display() {
    // Critical section
}
```

**2. Synchronized Block**

```java
void display() {
    synchronized (this) {
        // Critical section
    }
}
```

A synchronized block allows us to lock only the specific section of code that needs protection.

---

## 5. Important Terminology

| Term | Meaning |
|---|---|
| Mutation | Changing an object's state |
| Race Condition | Result depends on thread execution order |
| Thread Safety | Correct behavior under concurrent access |
| synchronized | Controls access using monitor locks |
| Critical Section | Code accessing shared resources |
| Shared Resource | Data accessed by multiple threads |
| Atomic Operation | An operation that happens as one indivisible unit |
| Monitor Lock | Lock used by Java synchronization |

---

## 6. How These Concepts Are Connected

1. **Mutation:** An object is modified.
2. **Shared Mutable Data:** Multiple threads access the same object.
3. **Race Condition:** Concurrent modifications can produce unexpected results.
4. **Synchronization:** Controls access using locks.
5. **Thread Safety:** Ensures correct behavior under concurrent access.

---

## 7. Important Interview Points

- `count++` is not an atomic operation.
- Multiple threads modifying shared mutable data can cause race conditions.
- `synchronized` provides mutual exclusion and memory visibility.
- Only one thread at a time can hold a particular monitor lock.
- Different objects have different instance monitor locks.
- A synchronized instance method locks the current object (`this`).
- A static synchronized method locks the class object.
- Synchronization can introduce performance overhead if overused.

### Important: synchronized vs volatile

| synchronized | volatile |
|---|---|
| Provides mutual exclusion | Does not provide mutual exclusion |
| Provides memory visibility | Provides memory visibility |
| Can protect compound operations | Does not make `count++` atomic |
| Uses monitor locks | Does not use monitor locks |

---

## 8. Quick Revision

Remember these definitions:

- **Mutation:** Changing an object's state.
- **Race Condition:** Multiple threads accessing shared data without proper coordination, causing unpredictable results.
- **Thread Safety:** Code works correctly under concurrent execution.
- **Synchronization:** Controls access to shared resources using locks.

### Next Topics to Learn

- [ ] volatile keyword
- [ ] AtomicInteger
- [ ] synchronized vs volatile
- [ ] Deadlock
- [ ] wait(), notify() and notifyAll()
- [ ] ExecutorService
- [ ] Concurrent Collections

---

**Revision Goal:** Understand how multiple threads interact with shared mutable data and how Java synchronization prevents race conditions.
