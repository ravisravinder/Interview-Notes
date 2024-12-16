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


# Java Concepts - README

## Table of Contents
1. [null in Java](#null-in-java)
2. [Object Class Methods](#object-class-methods)
3. [Final Keyword](#final-keyword)
4. [Immutability and String](#immutability-and-string)
5. [Enums in Java](#enums-in-java)
6. [String Creation and String Constant Pool](#string-creation-and-string-constant-pool)
7. [Fail-Fast vs. Fail-Safe](#fail-fast-vs-fail-safe)
8. [Dependency Injection Types](#dependency-injection-types)
9. [Dependency Injection vs Dependency Inversion Principle](#dependency-injection-vs-dependency-inversion-principle)

---

## null in Java
- `null` is a literal in Java that represents the absence of a value or object.
- It signifies that a reference variable does not point to any object or is uninitialized.

### Key Points:
- **Represents Absence of Object Reference:**
  ```java
  String str = null; // str doesn't refer to any object
  ```

---

## Object Class Methods
- The `Object` class is the root of the class hierarchy in Java. Every class implicitly extends it.

### Key Methods:
1. `equals(Object obj)` - Compares this object to another.
2. `hashCode()` - Returns a hash code for the object.
3. `toString()` - Returns a string representation of the object.
4. `clone()` - Creates and returns a copy of the object.
5. `finalize()` - Called before the object is garbage collected.

---

## Final Keyword
- Used to declare constants, restrict inheritance, or prevent method overriding.

### Key Uses:
1. **Final Variables:** Values cannot be changed once assigned.
2. **Final Methods:** Cannot be overridden by subclasses.
3. **Final Classes:** Cannot be extended.

---

## Immutability and String

### Why Strings are Immutable:
- Strings are stored in the String Constant Pool to save memory and prevent duplication.
- Immutability ensures thread safety and secure handling of sensitive data.

---

## Enums in Java
- Enums are special classes used to define a collection of constants.

### Key Benefits:
- Readability and maintainability.
- Type safety.
- Built-in methods like `values()`, `ordinal()`, and `valueOf()`.

---

## String Creation and String Constant Pool
- **Ways to Create Strings:**
  ```java
  // Using literals
  String str1 = "Hello";

  // Using the new keyword
  String str2 = new String("Hello");
  ```
- **String Constant Pool:**
  - A memory area in the heap where string literals are stored.
  - Ensures that duplicate strings are not created.

---

## Fail-Fast vs. Fail-Safe

### Fail-Fast:
- Throws `ConcurrentModificationException` when a collection is modified during iteration.
- Used in collections like `ArrayList`, `HashMap`, and `HashSet`.

**Example:**
```java
List<String> list = new ArrayList<>();
list.add("One");
list.add("Two");

Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    if (iterator.next().equals("One")) {
        list.add("Three"); // Throws ConcurrentModificationException
    }
}
```

### Fail-Safe:
- Allows safe modification of a collection during iteration by working on a copy.
- Used in `CopyOnWriteArrayList` and `ConcurrentHashMap`.

**Example:**
```java
List<String> list = new CopyOnWriteArrayList<>();
list.add("One");
list.add("Two");

for (String value : list) {
    if (value.equals("One")) {
        list.add("Three");
    }
}
```

---

## Dependency Injection Types
- Dependency injection is a design pattern used to achieve inversion of control.

### 1. Constructor Injection:
- **Use Case:** Mandatory dependencies.
- Promotes immutability and testability.

**Example:**
```java
public class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

### 2. Field Injection:
- **Use Case:** Optional dependencies.

**Example:**
```java
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

### 3. Setter Injection:
- **Use Case:** Post-construction or optional dependencies.

**Example:**
```java
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

---

## Dependency Injection vs Dependency Inversion Principle
- **Dependency Injection (DI):** A technique for supplying dependencies to a class.
- **Dependency Inversion Principle (DIP):** A design principle that states:
  1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
  2. Abstractions should not depend on details. Details should depend on abstractions.

**Summary:**
- DI is an implementation technique for achieving DIP.

  # Injection Types and Usage

## 14. Which Injection is Better & When Should Use What

### 1. Constructor Injection

**When to use:**
- **Required dependencies:** Use when dependencies are mandatory for the class to function correctly.
- **Immutability:** Promotes immutability by fully initializing the object at creation.
- **Best for Testability:** Easier to mock or inject dependencies in unit tests.
- **Enforces consistency:** Class cannot be instantiated without its required dependencies.

```java
public class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

2. Field Injection
When to use:

Simplicity: Most concise and easiest form.
Optional dependencies: Suitable when dependencies are optional or have default values.
Flexibility: Allows Spring to handle dependency injection internally.
java
Copy code
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
3. Setter Injection
When to use:

Optional dependencies: When dependencies are not essential and can be set after object creation.
Post-construction configuration: Suitable for modifying dependencies after object instantiation.
Lesser immutability: Allows changes to dependencies during the object's lifetime.
java
Copy code
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
Summary
Constructor Injection: Use when dependencies are mandatory, the object is immutable, and testing is required.
Field Injection: Quick and automatic but lacks immutability and makes testing harder.
Setter Injection: Use when dependencies are optional, or when post-construction modification is needed.
15. What is the difference between Dependency Injection and Dependency Inversion Principle
16. Are Wrapper Classes Immutable by Default?
Yes, wrapper classes in Java (such as Integer, Double, Character, Boolean, etc.) are immutable by design.

Why Wrapper Classes are Immutable
Final Fields: Fields are final, meaning their values cannot change after initialization.
No Setter Methods: No methods allow modification of the internal state.
No Method to Alter Value: Methods like intValue() return copies, not alter the object's state.
Example:

java
Copy code
Integer x = 10;
x = 20; // Creates a new Integer object; original 10 is unchanged
Characteristics of Immutable Objects
Final class
Final fields
No setter methods
Object state is set in constructor
Example of Immutable Wrapper Class:

java
Copy code
Integer i1 = Integer.valueOf(10);
Integer i2 = i1;
i1 = 15; // New Integer object created
System.out.println(i1); // 15
System.out.println(i2); // 10
17. Strategy Design Pattern
The Strategy Design Pattern is a behavioral design pattern that selects an algorithm's behavior at runtime.

Example: Mastercard Payment Strategies
Step 1: Define the Strategy Interface
java
Copy code
public interface PaymentStrategy {
    void processPayment(String cardNumber, double amount);
}
Step 2: Implement Concrete Strategies
Domestic Payment Strategy:
java
Copy code
public class DomesticPaymentStrategy implements PaymentStrategy {
    @Override
    public void processPayment(String cardNumber, double amount) {
        System.out.println("Processing domestic payment of $" + amount + " for card " + cardNumber);
    }
}
International Payment Strategy:
java
Copy code
public class InternationalPaymentStrategy implements PaymentStrategy {
    @Override
    public void processPayment(String cardNumber, double amount) {
        System.out.println("Processing international payment of $" + amount + " for card " + cardNumber);
    }
}
Contactless Payment Strategy:
java
Copy code
public class ContactlessPaymentStrategy implements PaymentStrategy {
    @Override
    public void processPayment(String cardNumber, double amount) {
        System.out.println("Processing contactless payment of $" + amount + " for card " + cardNumber);
    }
}
Step 3: Create the Context
java
Copy code
public class PaymentContext {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void executePayment(String cardNumber, double amount) {
        if (paymentStrategy == null) {
            throw new IllegalStateException("Payment strategy is not set!");
        }
        paymentStrategy.processPayment(cardNumber, amount);
    }
}
Step 4: Test the Strategy Pattern
java
Copy code
public class StrategyPatternDemo {
    public static void main(String[] args) {
        PaymentContext paymentContext = new PaymentContext();

        // Domestic Payment
        paymentContext.setPaymentStrategy(new DomesticPaymentStrategy());
        paymentContext.executePayment("1234-5678-9012-3456", 100.00);

        // International Payment
        paymentContext.setPaymentStrategy(new InternationalPaymentStrategy());
        paymentContext.executePayment("1234-5678-9012-3456", 200.00);

        // Contactless Payment
        paymentContext.setPaymentStrategy(new ContactlessPaymentStrategy());
        paymentContext.executePayment("1234-5678-9012-3456", 50.00);
    }
}
Output
bash
Copy code
Processing domestic payment of $100.0 for card 1234-5678-9012-3456
Processing international payment of $200.0 for card 1234-5678-9012-3456
Processing contactless payment of $50.0 for card 1234-5678-9012-3456
18. Can This Be Improved?
You can use a factory to dynamically select and instantiate strategies.

Improved Version: Strategy Selection with Factory
PaymentStrategyFactory
java
Copy code
public class PaymentStrategyFactory {
    public static PaymentStrategy getPaymentStrategy(String type) {
        switch (type.toLowerCase()) {
            case "domestic":
                return new DomesticPaymentStrategy();
            case "international":
                return new InternationalPaymentStrategy();
            case "contactless":
                return new ContactlessPaymentStrategy();
            default:
                throw new IllegalArgumentException("Unknown payment type: " + type);
        }
    }
}
Updated PaymentContext
java
Copy code
public class PaymentContext {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(String type) {
        this.paymentStrategy = PaymentStrategyFactory.getPaymentStrategy(type);
    }

    public void executePayment(String cardNumber, double amount) {
        if (paymentStrategy == null) {
            throw new IllegalStateException("Payment strategy is not set!");
        }
        paymentStrategy.processPayment(cardNumber, amount);
    }
}
Example Usage
java
Copy code
public class StrategyPatternDemo {
    public static void main(String[] args) {
        PaymentContext paymentContext = new PaymentContext();

        // Client only passes a string to select the strategy
        paymentContext.setPaymentStrategy("domestic");
        paymentContext.executePayment("1234-5678-9012-3456", 100.00);

        paymentContext.setPaymentStrategy("international");
        paymentContext.executePayment("1234-5678-9012-3456", 200.00);

        paymentContext.setPaymentStrategy("contactless");
        paymentContext.executePayment("1234-5678-9012-3456", 50.00);
    }
}
Output
bash
Copy code
Processing domestic payment of $100.0 for card 1234-5678-9012-3456
Processing international payment of $200.0 for card 1234-5678-9012-3456
Processing contactless payment of $50.0 for card 1234-5678-9012-3456
Benefits of Using a Factory
Simplifies client code
Encapsulates decision logic
Extensibility
19. Design Patterns
Creational:

Singleton: GFG
Factory: GFG, Versha
Abstract Factory: GFG
Builder: Versha, YouTube
Prototype: ChatGPT
Behavioral:

Observer: YouTube
State
Strategy: GFG, YouTube
Chain of Responsibility
Structural:

Decorator: YouTube
Facade: YouTube
Copy code


SOLIDL








