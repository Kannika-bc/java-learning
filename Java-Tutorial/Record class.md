# Java Record Class – Quick Revision Notes

## 1. What is a Record Class?

A Record Class in Java is a special type of class used to store data.

It was introduced as a preview feature in Java 14 and became a permanent feature in Java 16.

Normally, when creating a class just to store data, we need to write:

- Fields
- Constructor
- Getter methods
- `equals()`
- `hashCode()`
- `toString()`

A Record automatically generates these for us.

In simple words: A Record is a shorter way to create a class whose main purpose is to hold data.

## 2. Normal Class vs Record Class

Using a normal class:

```
class Student {
    private final String name;
    private final int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

Using a Record:

```
record Student(String name, int age) {
}
```

That's it! Java automatically generates the constructor, accessors, `equals()`, `hashCode()` and `toString()`.

## 3. How to Create and Use a Record

Syntax:

```
record RecordName(DataType variable1, DataType variable2) {
}
```

Example:

```
record Student(String name, int age) {
}

public class Main {
    public static void main(String[] args) {

        Student s1 = new Student("John", 22);

        System.out.println(s1.name());
        System.out.println(s1.age());
        System.out.println(s1);
    }
}
```

Output:

```
John
22
Student[name=John, age=22]
```

Important: Record accessor methods use the component names, not the traditional `getName()` format.

For example:

- `name()` instead of `getName()`
- `age()` instead of `getAge()`

## 4. Features of Record Classes

### 1. Automatically Generated Constructor

Java automatically creates a canonical constructor containing all record components.

```
record Employee(String name, int salary) {
}
```

You can create objects directly:

```
Employee e1 = new Employee("John", 50000);
```

### 2. Automatically Generated Accessor Methods

Java creates methods to access each component.

```
record Employee(String name, int salary) {
}

Employee e1 = new Employee("John", 50000);

System.out.println(e1.name());
System.out.println(e1.salary());
```

### 3. Automatically Generated toString()

Provides a readable String representation of the object.

```
record Employee(String name, int salary) {
}

Employee e1 = new Employee("John", 50000);

System.out.println(e1);
```

Output:

```
Employee[name=John, salary=50000]
```

### 4. Automatically Generated equals() and hashCode()

Two records with the same component values are considered equal.

```
record Student(String name, int age) {
}

Student s1 = new Student("John", 22);
Student s2 = new Student("John", 22);

System.out.println(s1.equals(s2));
```

Output:

```
true
```

Unlike a normal class that does not override `equals()`, records automatically compare their component values.

## 5. Are Record Objects Mutable?

No. Records are shallowly immutable.

This means:

- Record components are implicitly `private final`.
- You cannot reassign their values after object creation.
- Records do not provide setter methods.

Example:

```
record Student(String name, int age) {
}

Student s1 = new Student("John", 22);

// Not allowed
// s1.age = 25;
```

However, remember that records provide only shallow immutability.

For example:

```
record Student(List<String> subjects) {
}
```

You cannot assign another List to `subjects`, but you can still modify the original List:

```
List<String> subjects = new ArrayList<>();
subjects.add("Java");

Student s1 = new Student(subjects);

s1.subjects().add("Python");
```

The List inside the record is mutable.

## 6. Can We Add Methods to Records?

Yes! Records can contain custom methods.

```
record Rectangle(int length, int width) {

    int area() {
        return length * width;
    }
}

public class Main {
    public static void main(String[] args) {

        Rectangle r1 = new Rectangle(10, 5);

        System.out.println(r1.area());
    }
}
```

Output:

```
50
```

## 7. Can We Create a Custom Constructor?

Yes. Records support custom constructors.

Compact constructor:

Used when we want to validate values without explicitly writing field assignments.

```
record Student(String name, int age) {

    Student {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
}
```

Java automatically assigns the constructor parameters to the record components after the compact constructor body completes successfully.

## 8. Important Rules of Record Classes

1. Records are implicitly `final`.
2. Records cannot extend another class.
3. Records can implement interfaces.
4. Record components are implicitly `private final`.
5. Records cannot declare additional instance fields.
6. Records can contain static fields and methods.
7. Records can have custom constructors and methods.
8. Records automatically generate `equals()`, `hashCode()` and `toString()`.

Example of implementing an interface:

```
interface Printable {
    void print();
}

record Student(String name) implements Printable {

    public void print() {
        System.out.println(name);
    }
}
```

## 9. Normal Class vs Record Class

| Normal Class                                               | Record Class                                 |
| ---------------------------------------------------------- | -------------------------------------------- |
| Used for general-purpose objects                           | Mainly used to represent data                |
| Can be mutable or immutable                                | Shallowly immutable                          |
| Can extend another class                                   | Cannot extend another class                  |
| Can have setters                                           | No automatically generated setters           |
| Need to implement equals() if value comparison is required | Automatically generated value-based equals() |
| Can have additional instance fields                        | Cannot declare additional instance fields    |
| More boilerplate code                                      | Less boilerplate code                        |

## 10. When Should We Use Records?

Use records when:

- You need to store data without much additional behaviour.
- You want immutable data objects.
- You want to reduce boilerplate code.
- You are creating DTOs (Data Transfer Objects).
- You need to return multiple related values from a method.

Avoid records when:

- You need mutable objects.
- You need a class with complex inheritance.
- Your object requires extensive state changes.

## 11. Quick Interview Revision

| Question                               | Answer                                             |
| -------------------------------------- | -------------------------------------------------- |
| When were records introduced?          | Preview in Java 14, permanent in Java 16           |
| What is a record?                      | Special class for holding data                     |
| Are records immutable?                 | Shallowly immutable                                |
| Can records extend classes?            | No                                                 |
| Can records implement interfaces?      | Yes                                                |
| Can records have methods?              | Yes                                                |
| Can records have constructors?         | Yes                                                |
| Can records have setters?              | Not automatically; components cannot be reassigned |
| Are records final?                     | Yes                                                |
| Are equals() and hashCode() generated? | Yes                                                |
| Can records have static fields?        | Yes                                                |

Remember for interviews:

- `record` reduces boilerplate code.
- Record components are `private final`.
- Accessors use component names, such as `name()`.
- Records are implicitly final.
- Records cannot extend classes but can implement interfaces.
- Records provide shallow immutability, not deep immutability.

One-line revision: A Record Class is a special Java class introduced in Java 16 that simplifies creating immutable data-carrier objects by automatically generating constructors, accessors, equals(), hashCode() and toString().
