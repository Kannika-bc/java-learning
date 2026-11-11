# Java Optional Class – Quick Revision Notes

## 1. What is Optional?

- `Optional` is a class introduced in **Java 8** (`java.util.Optional`).
- It represents a value that may or may not be present.
- It provides a clear way to handle missing values instead of relying directly on `null`.
- It provides methods to check, retrieve, transform, or provide alternative values.

**Why do we need Optional?**

Using `null` to represent a missing value requires explicit null checks. If these checks are missed, accessing the value can result in a `NullPointerException`.

Optional makes the possibility of an absent value explicit and provides methods to handle both present and empty cases.

## 2. Example Without Optional

```java
public class OptionalDemo {
    public static void main(String[] args) {

        String[] words = new String[10];

        String word = words[5].toLowerCase();

        System.out.println(word);
    }
}
```

**Output:**
```text
Exception in thread "main" java.lang.NullPointerException
```

**Explanation:**
- An array of Strings is created with 10 positions.
- All positions initially contain `null`.
- `words[5]` returns `null`.
- Calling `toLowerCase()` on null causes a `NullPointerException`.

## 3. Example Using Optional

```java
import java.util.Optional;

public class OptionalDemo {
    public static void main(String[] args) {

        String[] words = new String[10];

        Optional<String> checkNull =
                Optional.ofNullable(words[5]);

        if (checkNull.isPresent()) {
            String word = checkNull.get().toLowerCase();
            System.out.println(word);
        } else {
            System.out.println("word is null");
        }
    }
}
```

**Output:**
```text
word is null
```

**Explanation:**
- `Optional.ofNullable()` wraps the value inside an Optional.
- `isPresent()` checks whether a value exists.
- If a value exists, `get()` retrieves it.
- Otherwise, the else block executes.

Note: Optional does not completely eliminate NullPointerException. It provides a safer way to handle potentially missing values.

## 4. Creating Optional Objects

Java provides three important factory methods:

| Method | Description |
|---|---|
| `Optional.empty()` | Creates an Optional with no value. |
| `Optional.of(value)` | Creates an Optional containing a non-null value. Throws NullPointerException if value is null. |
| `Optional.ofNullable(value)` | Creates an Optional containing the value if non-null; otherwise returns an empty Optional. |

### Example

```java
import java.util.Optional;

class Main {
    public static void main(String[] args) {

        String name = "Kanika";

        Optional<String> empty = Optional.empty();

        Optional<String> value = Optional.of(name);

        Optional<String> nullable = Optional.ofNullable(null);

        System.out.println(empty);
        System.out.println(value);
        System.out.println(nullable);
    }
}
```

**Output:**
```text
Optional.empty
Optional[Kanika]
Optional.empty
```

## 5. Important Optional Methods

| Method | Description |
|---|---|
| `empty()` | Creates an empty Optional. |
| `of()` | Creates Optional with a non-null value. |
| `ofNullable()` | Creates Optional from a possibly null value. |
| `get()` | Retrieves the value; throws NoSuchElementException if empty. |
| `isPresent()` | Returns true if a value exists. |
| `isEmpty()` | Returns true if no value exists (Java 11+). |
| `ifPresent()` | Executes an action if a value exists. |
| `orElse()` | Returns the value or a default value. |
| `orElseGet()` | Returns the value or gets a default from a Supplier. |
| `orElseThrow()` | Returns the value or throws an exception. |
| `filter()` | Keeps the value only if it satisfies a condition. |
| `map()` | Transforms the value if present. |
| `flatMap()` | Transforms a value using a function that already returns Optional. |
| `equals()` | Checks whether two Optional objects are equal. |
| `hashCode()` | Returns the hash code of the value, or 0 if empty. |
| `toString()` | Returns a String representation of Optional. |

## 6. Important Methods with Examples

### A. isPresent() and isEmpty()

```java
Optional<String> name = Optional.of("Kanika");

System.out.println(name.isPresent()); // true
System.out.println(name.isEmpty());   // false
```

### B. get()

Retrieves the actual value from Optional.

```java
Optional<String> name = Optional.of("Kanika");

System.out.println(name.get());
```

Output:
```text
Kanika
```

**Important:** Calling `get()` on an empty Optional throws `NoSuchElementException`. Avoid using it without checking whether a value exists.

### C. orElse()

Returns the value if present; otherwise, returns a default value.

```java
Optional<String> name = Optional.empty();

String result = name.orElse("Unknown");

System.out.println(result);
```

Output:
```text
Unknown
```

### D. orElseGet()

Returns the value if present; otherwise, executes a Supplier to generate a default value.

```java
Optional<String> name = Optional.empty();

String result = name.orElseGet(() -> "Unknown");
```

**Difference between orElse() and orElseGet():**

- `orElse()` evaluates its default argument even when the Optional contains a value.
- `orElseGet()` calls the Supplier only when the Optional is empty.

### E. orElseThrow()

Throws an exception if the value is missing.

```java
Optional<String> name = Optional.empty();

String result = name.orElseThrow(
    () -> new RuntimeException("Name not found")
);
```

### F. ifPresent()

Executes an action only if the value exists.

```java
Optional<String> name = Optional.of("Kanika");

name.ifPresent(n -> System.out.println(n));
```

Output:
```text
Kanika
```

### G. filter()

Checks whether the value satisfies a condition.

```java
Optional<String> name = Optional.of("Kanika");

Optional<String> result =
        name.filter(n -> n.startsWith("K"));

System.out.println(result);
```

Output:
```text
Optional[Kanika]
```

If the condition is false, it returns `Optional.empty()`.

### H. map()

Transforms the value inside Optional if present.

```java
Optional<String> name = Optional.of("Kanika");

Optional<Integer> length = name.map(n -> n.length());

System.out.println(length);
```

Output:
```text
Optional[6]
```

### I. flatMap()

Similar to map(), but the mapping function itself returns an Optional. It avoids creating nested Optional objects.

```java
Optional<String> name = Optional.of("Kanika");

Optional<String> result =
        name.flatMap(n -> Optional.of(n.toUpperCase()));

System.out.println(result);
```

Output:
```text
Optional[KANIKA]
```

## 7. Optional with Java Streams

One important use of Optional is with Stream operations like `findFirst()` and `findAny()`.

```java
import java.util.Arrays;
import java.util.List;

public class OptionalEx {
    public static void main(String[] args) {

        List<String> names =
                Arrays.asList("Navin", "Laxmi", "John", "Kishor");

        String name = names.stream()
                .filter(str -> str.contains("x"))
                .findFirst()
                .orElse("Not Found");

        System.out.println(name);
    }
}
```

**Output:**
```text
Laxmi
```

**Execution flow:**

1. `stream()` – Converts the List into a Stream.
2. `filter()` – Keeps names containing the letter "x".
3. `findFirst()` – Returns the first matching element as `Optional<String>`.
4. `orElse()` – Returns the actual String if present, otherwise "Not Found".

**Important return types:**

| Method | Return type |
|---|---|
| `stream()` | `Stream<String>` |
| `filter()` | `Stream<String>` |
| `findFirst()` | `Optional<String>` |
| `orElse()` | `String` |

**Why does findFirst() return Optional?**

Because there might be no matching element. Instead of returning null, it returns `Optional.empty()`.

## 8. Important Interview Points

1. Optional was introduced in Java 8.
2. Optional belongs to `java.util` package.
3. Optional is a container object that may or may not contain a non-null value.
4. `Optional.of(null)` throws NullPointerException.
5. `Optional.ofNullable(null)` returns Optional.empty().
6. `Optional.empty()` creates an empty Optional.
7. `get()` on an empty Optional throws NoSuchElementException.
8. `findFirst()` and `findAny()` return Optional.
9. Optional is primarily intended for method return types, not as a replacement for every null reference.
10. Optional does not guarantee that your program will never throw NullPointerException.

## 9. Quick Revision – Remember These

| Situation | Method |
|---|---|
| Create empty Optional | `Optional.empty()` |
| Create with non-null value | `Optional.of(value)` |
| Value might be null | `Optional.ofNullable(value)` |
| Check if value exists | `isPresent()` |
| Retrieve value | `get()` |
| Get value or default | `orElse()` |
| Generate default if empty | `orElseGet()` |
| Throw exception if empty | `orElseThrow()` |
| Perform action if present | `ifPresent()` |
| Apply condition | `filter()` |
| Transform value | `map()` |
| Transform into another Optional | `flatMap()` |

**Remember:** Optional makes missing values explicit and gives us methods to handle them safely instead of directly working with null.

---

## 10. Quick Interview Quiz

1. What is the main purpose of Optional?
2. What is the difference between `of()` and `ofNullable()`?
3. What happens when `get()` is called on an empty Optional?
4. What is the difference between `orElse()` and `orElseGet()`?
5. Why does `findFirst()` return Optional?
6. What is the difference between `map()` and `flatMap()`?
7. Can Optional completely prevent NullPointerException?
