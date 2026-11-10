# Java Thread Priority and Sleep — Interview Notes

## 1. Thread Priority

Thread priority is a scheduling hint that indicates the relative importance of a thread.

Java supports priorities from 1 to 10.

### Priority Constants

| Constant | Value |
|---|---:|
| Thread.MIN_PRIORITY | 1 |
| Thread.NORM_PRIORITY | 5 |
| Thread.MAX_PRIORITY | 10 |

Default priority of a newly created thread is inherited from its parent thread. The main thread normally has priority 5.

### Important Methods

```java
t1.setPriority(8);
int p = t1.getPriority();
```

- `setPriority()` changes priority.
- `getPriority()` retrieves priority.
- Invalid priorities throw IllegalArgumentException.
- Higher priority does not guarantee earlier execution.
- Actual scheduling depends on JVM and operating system.

## 2. Thread.sleep()

`sleep()` pauses the currently executing thread for a specified duration.

### Syntax

```java
Thread.sleep(1000);
```

1000 milliseconds = 1 second.

### Important Points

1. sleep() is a static method of Thread.
2. It pauses the currently executing thread.
3. It does not pause the entire application.
4. It does not release intrinsic locks.
5. It can throw InterruptedException.
6. After sleeping, a thread becomes eligible to run again but may not resume immediately.

### Example

```java
public class Main {

    public static void main(String[] args)
            throws InterruptedException {

        System.out.println("Start");

        Thread.sleep(2000);

        System.out.println("End");
    }
}
```

## 3. sleep() vs join()

| sleep() | join() |
|---|---|
| Static method | Instance method |
| Pauses current thread | Waits for target thread to terminate |
| Takes time duration | Waits for thread completion |
| Does not release intrinsic locks | Does not release intrinsic locks |

## 4. InterruptedException

- Thrown when a thread is interrupted while sleeping or joining.
- Allows interruption of blocking operations.
- The interrupted status is cleared when this exception is thrown.
- Can be propagated or handled.
- Restore the interrupt status when appropriate:

```java
catch (InterruptedException e) {
    Thread.currentThread().interrupt();
}
```

## 5. Common Interview Questions

**Q1. What is thread priority?**

A scheduling hint indicating relative importance of a thread.

**Q2. What is the priority range in Java?**

1 to 10.

**Q3. What is the default thread priority?**

A newly created thread inherits its parent's priority. The main thread normally has priority 5.

**Q4. Does higher priority guarantee faster execution?**

No. It is only a scheduling hint.

**Q5. What is sleep()?**

A static method that pauses the currently executing thread for a specified duration.

**Q6. Why is sleep() static?**

Because it operates on the currently executing thread rather than a particular Thread object.

**Q7. Does sleep() stop other threads?**

No. Only the calling thread sleeps.

**Q8. Does sleep() release locks?**

No. It retains intrinsic locks held by the sleeping thread.

**Q9. Can sleep() throw an exception?**

Yes, InterruptedException.

**Q10. Difference between sleep() and join()?**

sleep() pauses the current thread for a duration. join() makes the calling thread wait for another thread to terminate.

## 6. One-Minute Revision

- Priority range: 1–10.
- Minimum: 1.
- Normal: 5.
- Maximum: 10.
- setPriority(): Sets priority.
- getPriority(): Gets priority.
- sleep(): Pauses current thread.
- sleep() is static.
- sleep() does not release intrinsic locks.
- sleep() can throw InterruptedException.
- Higher priority does not guarantee earlier execution.
- join() waits for another thread to terminate.
