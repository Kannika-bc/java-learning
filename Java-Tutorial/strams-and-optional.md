# Java Quick Revision – Stream API, Parallel Streams & Optional

## 1. Stream API

### What is Stream API?

- Introduced in Java 8.
- Used to process data from collections such as ArrayList, HashSet, etc.
- Makes data processing easier using operations like filtering, mapping, sorting and reducing.
- A Stream does not store data. It processes data from a source.
- It does not modify the original collection.

### Collection vs Stream

| Collection | Stream |
|---|---|
| Stores data | Processes data |
| Can be reused | Can be consumed only once |
| Supports adding/removing elements | Does not modify the source |
| Data structure | Data processing pipeline |

### Stream Pipeline

A Stream pipeline has three stages:

1. **Source:** Collection, Array, etc.
2. **Intermediate Operations:** filter(), map(), sorted().
3. **Terminal Operations:** collect(), forEach(), reduce().

Example:

```java
List<Integer> numbers = Arrays.asList(10, 20, 30, 40, 50);

List<Integer> result = numbers.stream()
        .filter(n -> n > 20)
        .map(n -> n * 2)
        .collect(Collectors.toList());

System.out.println(result);
```

Output:
```text
[60, 80, 100]
```

### Intermediate Operations

- Return another Stream.
- Can be chained.
- Are lazy: they execute only when a terminal operation is called.

| Method | Purpose |
|---|---|
| filter() | Selects elements based on a condition |
| map() | Transforms each element |
| sorted() | Sorts elements |
| distinct() | Removes duplicates |
| limit() | Limits the number of elements |
| skip() | Skips elements |
| peek() | Inspects elements during processing |

### Terminal Operations

- Trigger the execution of the Stream pipeline.
- Produce a result or perform an action.
- Consume the Stream.

| Method | Purpose |
|---|---|
| forEach() | Performs an action on each element |
| collect() | Collects elements into a collection |
| reduce() | Combines elements into one result |
| count() | Counts elements |
| min() | Finds minimum element |
| max() | Finds maximum element |
| findFirst() | Returns the first element |
| anyMatch() | Checks if any element matches |
| allMatch() | Checks if all elements match |

---

## 2. Important Stream Methods

### 2.1 filter()

Selects elements based on a condition.

**Syntax:**
```java
stream.filter(condition);
```

Example:
```java
List<Integer> numbers = Arrays.asList(10, 15, 20, 25, 30);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());

System.out.println(result);
```

Output:
```text
[10, 20, 30]
```

Remember: filter() selects elements; it does not transform them.

### 2.2 map()

Transforms each element into another value.

**Syntax:**
```java
stream.map(transformation);
```

Example:
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

List<Integer> squares = numbers.stream()
        .map(n -> n * n)
        .collect(Collectors.toList());

System.out.println(squares);
```

Output:
```text
[1, 4, 9, 16, 25]
```

Remember: map() transforms every element.

### filter() vs map()

| filter() | map() |
|---|---|
| Selects elements | Transforms elements |
| Uses a condition | Uses a transformation |
| Keeps or removes elements | Produces a mapped value |
| Returns a Stream | Returns a Stream |

Example using both:
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .collect(Collectors.toList());

System.out.println(result);
```

Output:
```text
[4, 16, 36]
```

### 2.3 sorted()

Sorts elements in a Stream.

**Ascending order:**
```java
List<Integer> numbers = Arrays.asList(50, 10, 40, 20, 30);

List<Integer> result = numbers.stream()
        .sorted()
        .collect(Collectors.toList());
```

Output:
```text
[10, 20, 30, 40, 50]
```

**Descending order:**
```java
List<Integer> result = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .collect(Collectors.toList());
```

Output:
```text
[50, 40, 30, 20, 10]
```

**Sorting objects using Comparator:**
```java
students.stream()
        .sorted(Comparator.comparing(s -> s.age))
        .forEach(s -> System.out.println(s.name));
```

Remember:
- sorted() uses natural ordering by default.
- Comparator can be used for custom sorting.
- sorted() does not modify the original collection.

### 2.4 reduce()

Combines all elements into a single result.

Common uses:
- Sum of numbers.
- Product of numbers.
- Combining values.

**Syntax:**
```java
stream.reduce(identity, accumulator);
```

- Identity: Initial value.
- Accumulator: Function that combines the current result with the next element.

**Example: Sum**
```java
List<Integer> numbers = Arrays.asList(10, 20, 30, 40);

int sum = numbers.stream()
        .reduce(0, (a, b) -> a + b);

System.out.println(sum);
```

Output:
```text
100
```

Working:
```text
0 + 10 = 10
10 + 20 = 30
30 + 30 = 60
60 + 40 = 100
```

**Example: Product**
```java
List<Integer> numbers = Arrays.asList(2, 3, 4);

int product = numbers.stream()
        .reduce(1, (a, b) -> a * b);

System.out.println(product);
```

Output:
```text
24
```

Remember:
- filter() → Select
- map() → Transform
- sorted() → Sort
- reduce() → Combine into one result
- collect() → Collect results into a container

---

## 3. Parallel Streams

### What is a Parallel Stream?

- Introduced in Java 8.
- Processes elements concurrently using multiple threads.
- Can divide work into smaller tasks.
- Commonly uses the ForkJoinPool common pool.
- Can improve performance for suitable large workloads.

### Sequential vs Parallel Stream

**Sequential Stream:**
```java
numbers.stream()
       .forEach(System.out::println);
```

Processes elements sequentially.

**Parallel Stream:**
```java
numbers.parallelStream()
       .forEach(System.out::println);
```

Processes elements concurrently. Output order is not guaranteed with forEach().

### Creating Parallel Streams

Two ways:

**Method 1:**
```java
numbers.parallelStream();
```

**Method 2:**
```java
numbers.stream().parallel();
```

### How Parallel Streams Work

1. Divides the workload into smaller tasks.
2. Processes tasks using multiple worker threads.
3. Combines the results.

Parallel Streams commonly use the ForkJoinPool common pool.

### Sequential vs Parallel

| Sequential | Parallel |
|---|---|
| Processes sequentially | Processes concurrently |
| Usually one processing thread | Uses multiple worker threads |
| Simpler execution | More complex execution |
| Predictable encounter order with forEach() on ordered streams | forEach() order is not guaranteed |
| Suitable for many ordinary tasks | Useful for suitable large workloads |

### Advantages

- Uses multiple CPU cores.
- Can reduce processing time.
- Requires less manual thread management.

### Disadvantages

- Thread management introduces overhead.
- Not always faster.
- Debugging can be difficult.
- Shared mutable data can cause race conditions.
- Execution order may not be guaranteed.

### Thread Safety in Parallel Streams

**Unsafe example:**
```java
List<Integer> result = new ArrayList<>();

numbers.parallelStream()
       .forEach(n -> result.add(n));
```

Why is this unsafe?

- Multiple threads access the same ArrayList.
- Multiple threads may modify it simultaneously.
- ArrayList is not thread-safe.
- Can cause race conditions and unpredictable results.

**Safer approach:**
```java
List<Integer> result = numbers.parallelStream()
        .map(n -> n * 2)
        .collect(Collectors.toList());
```

Use Stream collectors instead of manually modifying shared collections.

### Important Points

- Parallel Streams do not automatically make code thread-safe.
- Avoid shared mutable variables.
- Prefer stateless operations.
- Parallel Streams are not always faster.
- Use them when the workload benefits from parallel execution.

---

## 4. Optional Class

### What is Optional?

- Introduced in Java 8.
- Represents a value that may or may not be present.
- Helps handle missing values instead of directly relying on null.
- Provides methods to check, retrieve, transform or provide alternatives for a value.
- Helps avoid NullPointerException when used correctly.

### Why Do We Need Optional?

Without Optional:
```java
String name = null;

System.out.println(name.length());
```

Throws:
```text
NullPointerException
```

We can manually check:
```java
if (name != null) {
    System.out.println(name.length());
}
```

Optional provides a cleaner way to handle potentially missing values.

### Creating Optional Objects

**1. Optional.of()**

Used when the value is definitely not null.

```java
Optional<String> name = Optional.of("Java");
```

Passing null throws NullPointerException.

**2. Optional.ofNullable()**

Used when the value may be null.

```java
String value = null;

Optional<String> name = Optional.ofNullable(value);
```

If null is passed, it creates an empty Optional.

**3. Optional.empty()**

Creates an empty Optional.

```java
Optional<String> name = Optional.empty();
```

### Important Optional Methods

**1. isPresent()**

Checks whether a value is present.

```java
Optional<String> name = Optional.of("Java");

System.out.println(name.isPresent());
```

Output: `true`

**2. isEmpty()**

Checks whether the Optional is empty.

```java
Optional<String> name = Optional.empty();

System.out.println(name.isEmpty());
```

Output: `true`

Note: Introduced in Java 11.

**3. get()**

Returns the value inside Optional.

```java
Optional<String> name = Optional.of("Java");

System.out.println(name.get());
```

Output: `Java`

Calling get() on an empty Optional throws NoSuchElementException.

Avoid using get() without checking whether a value is present.

**4. orElse()**

Returns the value if present; otherwise returns a default value.

```java
Optional<String> name = Optional.empty();

String result = name.orElse("Unknown");

System.out.println(result);
```

Output: `Unknown`

**5. orElseGet()**

Uses a Supplier to provide a default value only when Optional is empty.

```java
Optional<String> name = Optional.empty();

String result = name.orElseGet(() -> "Unknown");
```

**orElse() vs orElseGet()**

| orElse() | orElseGet() |
|---|---|
| Takes a default value | Takes a Supplier |
| Default expression is evaluated even when value exists | Supplier executes only when value is absent |
| Useful for simple defaults | Useful for expensive default creation |

**6. ifPresent()**

Executes an action only when a value exists.

```java
Optional<String> name = Optional.of("Java");

name.ifPresent(value -> System.out.println(value));
```

Output: `Java`

**7. orElseThrow()**

Returns the value if present; otherwise throws an exception.

```java
Optional<String> name = Optional.empty();

String result = name.orElseThrow(
        () -> new RuntimeException("Name not found")
);
```

### map() with Optional

Transforms the value if present.

```java
Optional<String> name = Optional.of("java");

Optional<String> result = name.map(String::toUpperCase);

System.out.println(result.get());
```

Output:
```text
JAVA
```

If the Optional is empty, map() returns an empty Optional.

### filter() with Optional

Checks whether the value satisfies a condition.

```java
Optional<Integer> age = Optional.of(22);

Optional<Integer> result = age.filter(a -> a >= 18);

System.out.println(result);
```

Output:
```text
Optional[22]
```

If the condition is false, returns Optional.empty().

### Real-world Example

```java
class User {
    String name;

    User(String name) {
        this.name = name;
    }
}

public static Optional<User> findUser(int id) {
    if (id == 1) {
        return Optional.of(new User("John"));
    }

    return Optional.empty();
}

// Usage
Optional<User> user = findUser(1);

String name = user
        .map(u -> u.name)
        .orElse("User not found");

System.out.println(name);
```

Output:
```text
John
```

Optional is especially useful as a return type when a method may not find or produce a value.

---

## 5. Important Differences

### Stream API vs Optional

| Stream API | Optional |
|---|---|
| Processes multiple elements | Represents zero or one value |
| Used for data processing | Used for handling missing values |
| Supports filter, map, reduce, sorted | Supports filter, map, orElse, ifPresent |

### Stream map() vs Optional map()

**Stream map():** Transforms every element.

```java
List<Integer> result = numbers.stream()
        .map(n -> n * 2)
        .collect(Collectors.toList());
```

**Optional map():** Transforms the value only if present.

```java
Optional<String> result = name.map(String::toUpperCase);
```

---

## 6. Quick Revision Cheat Sheet

| Concept | Remember |
|---|---|
| Stream | Processes data |
| stream() | Creates sequential Stream |
| parallelStream() | Creates parallel Stream |
| filter() | Selects elements |
| map() | Transforms elements |
| sorted() | Sorts elements |
| reduce() | Combines elements |
| collect() | Collects results |
| Intermediate | Returns another Stream |
| Terminal | Triggers execution |
| Lazy evaluation | Starts execution at terminal operation |
| Parallel Stream | Processes work concurrently |
| ForkJoinPool | Common execution pool for parallel streams |
| Optional | Represents a possibly missing value |
| of() | Requires non-null value |
| ofNullable() | Accepts possibly null value |
| empty() | Creates empty Optional |
| isPresent() | Checks if value exists |
| get() | Retrieves value |
| orElse() | Provides default value |
| orElseGet() | Lazily provides default value |
| ifPresent() | Executes action if value exists |
| orElseThrow() | Throws exception if value is absent |

---

## 7. Important Interview Questions

1. What is Stream API in Java?
2. Difference between Collection and Stream?
3. Difference between intermediate and terminal operations?
4. Why are Stream intermediate operations lazy?
5. Difference between map() and filter()?
6. Difference between reduce() and collect()?
7. Can we reuse a Stream after a terminal operation?
8. What is a Parallel Stream?
9. How does Parallel Stream work internally?
10. Are Parallel Streams always faster?
11. What is a race condition in Parallel Streams?
12. What is the purpose of Optional?
13. Difference between of() and ofNullable()?
14. Difference between orElse() and orElseGet()?
15. What happens when get() is called on an empty Optional?
16. Difference between isPresent() and isEmpty()?

---

## 8. Practice Checklist

- [ ] Find even numbers using filter().
- [ ] Find squares using map().
- [ ] Sort numbers in ascending and descending order.
- [ ] Find sum using reduce().
- [ ] Find product using reduce().
- [ ] Find names starting with A.
- [ ] Convert names to uppercase.
- [ ] Find maximum number using max().
- [ ] Calculate sum using Parallel Stream.
- [ ] Create Optional using ofNullable().
- [ ] Use orElse() for missing values.
- [ ] Use map() and filter() with Optional.
- [ ] Find a user and return "User not found" if absent.

---

## 9. Key Takeaways

- Stream API makes collection processing easier and more readable.
- filter() selects, map() transforms, sorted() sorts and reduce() combines.
- Intermediate operations are lazy; terminal operations trigger execution.
- Parallel Streams use multiple threads but are not always faster.
- Avoid shared mutable data in parallel processing.
- Optional provides a clear way to represent potentially missing values.
- Prefer orElse(), orElseGet() and ifPresent() over blindly calling get().

**Java Version:** Java 8+ (Some methods, such as Optional.isEmpty(), require Java 11+).

**Topics:** Stream API | Lambda Expressions | Functional Programming | Parallel Processing | Optional Class
