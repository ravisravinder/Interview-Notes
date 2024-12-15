# Marker Interface in Java

A **marker interface** is an interface with no methods or fields, used to "mark" a class and signal that it has a specific property or behavior.

## Examples of Marker Interfaces

- **Serializable**: Marks a class for serialization (convert to byte stream).
- **Cloneable**: Marks a class for cloning via the `clone()` method.
- **Remote**: Marks a class for remote invocation in distributed systems.

## Why We Need Marker Interfaces

1. **Tagging/Classification**:
   - Categorizes classes without enforcing method implementation.
   - Example: `Serializable` informs the JVM a class can be serialized.

2. **Metadata Association**:
   - Provides metadata for frameworks or APIs to apply special processing.
   - Example: `ObjectOutputStream` checks for `Serializable` before serializing.

3. **Type Safety**:
   - Ensures compile-time enforcement of specific properties.
   - Example: Methods accepting `Cloneable` prevent non-cloneable objects.

---

# StringBuffer vs StringBuilder

| Feature        | StringBuffer            | StringBuilder             |
|----------------|-------------------------|---------------------------|
| **Thread Safety** | Thread-safe (synchronized) | Not thread-safe (no sync) |
| **Performance**   | Slower due to synchronization | Faster due to no synchronization |
| **Usage Scenario**| Use in multi-threaded scenarios | Use in single-threaded scenarios |

### Why We Need Them

- **Mutable Strings**: Both allow modification of strings without creating new objects, improving performance when performing frequent string manipulations.
- **Difference from String**: Unlike `String`, which is immutable, these provide mutable alternatives for better efficiency.

---

# Transient Keyword in Java

The `transient` keyword is used to indicate that a field should not be serialized during the serialization process.

## Key Points

1. **Purpose**:
   - Prevent sensitive or non-essential data from being saved when an object is serialized (converted to a byte stream).

2. **Behavior**:
   - Transient fields are skipped during serialization.
   - Their values are set to the default value (e.g., `null` for objects, `0` for integers) when deserialized.

3. **Use Cases**:
   - **Sensitive Data**: Avoid saving sensitive information (e.g., passwords).
   - **Non-Persistent Data**: Exclude fields derived from other values or temporary computations.
   - **External Systems**: Fields irrelevant for saving or sharing with third parties.

4. **Data Persistence**:
   - Transient fields are not saved in databases or sent to external systems during serialization.

### Example:

```java
class User implements Serializable {
    private String username;
    private transient String password; // Not serialized
}
```

When serialized, `password` is excluded from the byte stream.

### Why We Need It

- **Security**: Prevent sensitive data (e.g., credentials) from being serialized and shared.
- **Optimization**: Skip unnecessary or large fields during serialization.
- **Compliance**: Ensure certain data isn’t shared with third parties.

---

# Comparator vs Comparable in Java

| Feature       | Comparable                       | Comparator                         |
|---------------|----------------------------------|-------------------------------------|
| **Definition** | Used to define a natural ordering for objects of a class. | Used to define custom orderings outside the class. |
| **Interface**  | `java.lang.Comparable`          | `java.util.Comparator`             |
| **Method**     | `compareTo(Object o)`           | `compare(Object o1, Object o2)`    |
| **Location**   | Defined within the class being sorted. | Defined in a separate class or as a lambda. |
| **Use Case**   | Single, default sorting logic.  | Multiple, flexible sorting logics. |

### Comparable Example

```java
import java.util.*;

class Student implements Comparable<Student> {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.age, other.age); // Natural ordering by age
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }

    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
            new Student("Alice", 22),
            new Student("Bob", 20),
            new Student("Charlie", 25)
        );

        Collections.sort(students); // Uses Comparable
        System.out.println(students); // Output: [Bob (20), Alice (22), Charlie (25)]
    }
}
```

### Comparator Example

```java
import java.util.*;

class Student {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }

    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
            new Student("Alice", 22),
            new Student("Bob", 20),
            new Student("Charlie", 25)
        );

        // Sorting by name using Comparator
        students.sort((s1, s2) -> s1.name.compareTo(s2.name));
        System.out.println("Sorted by name: " + students);

        // Sorting by age using Comparator
        students.sort((s1, s2) -> Integer.compare(s1.age, s2.age));
        System.out.println("Sorted by age: " + students);
    }
}
```

### Java 8 Example with Streams

```java
import java.util.*;
import java.util.stream.Collectors;

class Student {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }

    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
            new Student("Alice", 22),
            new Student("Bob", 20),
            new Student("Charlie", 25)
        );

        // Sorting by name
        List<Student> sortedByName = students.stream()
                .sorted(Comparator.comparing(s -> s.name))
                .collect(Collectors.toList());
        System.out.println("Sorted by name: " + sortedByName);

        // Sorting by age
        List<Student> sortedByAge = students.stream()
                .sorted(Comparator.comparingInt(s -> s.age))
                .collect(Collectors.toList());
        System.out.println("Sorted by age: " + sortedByAge);
    }
}
```

### When to Use

- Use **Comparable** when a single natural order is sufficient.
- Use **Comparator** for multiple or flexible sorting orders.

---

# Wrapper Classes in Java

Wrapper classes are used to convert primitive data types into their corresponding objects. They allow primitive data types to be treated as objects, which is necessary when working with collections (like `ArrayList`, which can only store objects).

| Primitive Type | Wrapper Class |
|----------------|---------------|
| `int`          | `Integer`     |
| `char`         | `Character`   |
| `boolean`      | `Boolean`     |
| `byte`         | `Byte`        |
| `short`        | `Short`       |
| `long`         | `Long`        |
| `float`        | `Float`       |
| `double`       | `Double`      |

### Key Points

1. **Auto-boxing**: Automatically converts primitive values into wrapper class objects.

```java
int x = 10;
Integer xObj = x;  // Auto-boxing
```

2. **Unboxing**: Automatically converts wrapper class objects back to primitive values.

```java
Integer xObj = 10;
int x = xObj;  // Unboxing
```

3. **Methods**: Wrapper classes provide utility methods, like `parseInt()`, `valueOf()`, and `toString()`.

```java
int num = Integer.parseInt("123");  // Converts string to integer
String str = Integer.toString(123); // Converts integer to string
