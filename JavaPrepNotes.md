1.Marker Interface in Java
A marker interface is an interface with no methods or fields, used to "mark" a class and signal that it has a specific property or behavior.
Examples of Marker Interfaces
•	Serializable: Marks a class for serialization (convert to byte stream).
•	Cloneable: Marks a class for cloning via the clone() method.
•	Remote: Marks a class for remote invocation in distributed systems.
Why We Need Marker Interfaces
1.	Tagging/Classification:
o	Categorizes classes without enforcing method implementation.
o	Example: Serializable informs the JVM a class can be serialized.
2.	Metadata Association:
o	Provides metadata for frameworks or APIs to apply special processing.
o	Example: ObjectOutputStream checks for Serializable before serializing.
3.	Type Safety:
o	Ensures compile-time enforcement of specific properties.
o	Example: Methods accepting Cloneable prevent non-cloneable objects.
2.Here’s a concise comparison of StringBuffer and StringBuilder:
Feature	StringBuffer	StringBuilder
Thread Safety	Thread-safe (synchronized)	Not thread-safe (no sync)
Performance	Slower due to synchronization	Faster due to no synchronization
Usage Scenario	Use in multi-threaded scenarios	Use in single-threaded scenarios
Why We Need Them:
•	Mutable Strings: Both allow modification of strings without creating new objects, improving performance when performing frequent string manipulations.
•	Difference from String: Unlike String, which is immutable, these provide mutable alternatives for better efficiency.
3.Transient Keyword in Java
The transient keyword is used to indicate that a field should not be serialized during the serialization process.
Key Points
1.	Purpose:
o	Prevent sensitive or non-essential data from being saved when an object is serialized (converted to a byte stream).
2.	Behavior:
o	Transient fields are skipped during serialization.
o	Their values are set to the default value (e.g., null for objects, 0 for integers) when deserialized.
3.	Use Cases:
o	Sensitive Data: Avoid saving sensitive information (e.g., passwords).
o	Non-Persistent Data: Exclude fields derived from other values or temporary computations.
o	External Systems: Fields irrelevant for saving or sharing with 3rd parties.
4.	Data Persistence:
o	Transient fields are not saved in databases or sent to external systems during serialization.
Example:
java
Copy code
class User implements Serializable {
    private String username;
    private transient String password; // Not serialized
}
When serialized, password is excluded from the byte stream.
Why We Need It:
•	Security: Prevent sensitive data (e.g., credentials) from being serialized and shared.
•	Optimization: Skip unnecessary or large fields during serialization.
•	Compliance: Ensure certain data isn’t shared with 3rd parties.
Interview Perspective:
•	Explain what, why, and how it is used.
•	Highlight real-world scenarios like skipping passwords or cache fields during serialization.
•	Mention that transient fields are irrelevant for databases or shared objects unless explicitly handled

4.Comparator vs Comparable in Java
Feature	Comparable	Comparator
Definition	Used to define a natural ordering for objects of a class.	Used to define custom orderings outside the class.
Interface	java.lang.Comparable	java.util.Comparator
Method	compareTo(Object o)	compare(Object o1, Object o2)
Location	Defined within the class being sorted.	Defined in a separate class or as a lambda.
Use Case	Single, default sorting logic.	Multiple, flexible sorting logics.
________________________________________
Comparable Example
java
Copy code
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
________________________________________
Comparator Example
java
Copy code
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
________________________________________
Java 8 Example with Streams
java
Copy code
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
________________________________________
When to Use
•	Use Comparable when a single natural order is sufficient.
•	Use Comparator for multiple or flexible sorting orders.
Both can be combined for robust sorting strategies.

5. Wrapper Classes in Java
Wrapper classes are used to convert primitive data types into their corresponding objects. They allow primitive data types to be treated as objects, which is necessary when working with collections (like ArrayList, which can only store objects).
Each primitive type in Java has a corresponding wrapper class:
Primitive Type	Wrapper Class
int	Integer
char	Character
boolean	Boolean
byte	Byte
short	Short
long	Long
float	Float
double	Double
Key Points:
1.	Auto-boxing: Automatically converts primitive values into wrapper class objects.
java
Copy code
int x = 10;
Integer xObj = x;  // Auto-boxing
2.	Unboxing: Automatically converts wrapper class objects back to primitive values.
java
Copy code
Integer xObj = 10;
int x = xObj;  // Unboxing
3.	Methods: Wrapper classes provide utility methods, like parseInt(), valueOf(), and toString().
java
Copy code
int num = Integer.parseInt("123");  // Converts string to integer
String str = Integer.toString(123); // Converts integer to string

6.null in Java
1.null in Java is a literal that represents the absence of a value or non-existence of an object. It is not a keyword, but a special value used to signify that a reference does not point to any object or is uninitialized.


Key Points:
5.	Represents Absence of Object Reference:
6.	When a reference variable is assigned null, it means the variable does not refer to any object.
7.	java
8.	Copy code
9.	String str = null;  // str doesn't refer to any object

7. What are object class methods in java

8. Final Keyword in java
9. Immutability
10. Why string is immtable
11. Why we need enums in java
12. How many ways we can create a string and what is string constant pool
13. Fail-Fast vs. Fail-Safe in Java
In Java, fail-fast and fail-safe are two different mechanisms used to handle concurrent modification of collections, or to detect errors early. They are primarily used in the context of iterating over collections like List, Set, etc.
1. Fail-Fast
•	Definition: A fail-fast collection throws a ConcurrentModificationException if it detects that the collection has been modified (except by the iterator itself) while it is being iterated over. This ensures that errors are detected as soon as they occur.
•	When it's used:
o	It is used when you want to ensure that any modification to the collection (like adding, removing, or updating elements) while iterating will result in an immediate exception, preventing unpredictable behavior.
•	Where it's used:
o	ArrayList, HashMap, HashSet, and other non-thread-safe collections in Java utilize fail-fast behavior.
•	Significance:
o	Early error detection: It helps in detecting concurrent modification errors immediately, ensuring that you don’t silently run into bugs.
o	Prevents inconsistent state: By throwing an exception when an unexpected change happens, fail-fast behavior prevents the collection from entering an inconsistent state during iteration.
•	Example of Fail-Fast:
java
Copy code
import java.util.*;

public class FailFastExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("One");
        list.add("Two");
        
        // Fail-fast iterator
        Iterator<String> iterator = list.iterator();
        
        while (iterator.hasNext()) {
            String value = iterator.next();
            if (value.equals("One")) {
                list.remove("Two");  // Concurrent modification occurs here
            }
        }
    }
}
// Throws ConcurrentModificationException
________________________________________
2. Fail-Safe
•	Definition: A fail-safe collection, on the other hand, allows modification of the collection during iteration without throwing an exception. Instead of throwing an exception, these collections may make a copy of the collection and iterate over the copy, which prevents ConcurrentModificationException.
•	When it's used:
o	It is used when you need to safely iterate over a collection while making changes (like adding or removing elements) without causing errors.
•	Where it's used:
o	CopyOnWriteArrayList, CopyOnWriteArraySet, and other thread-safe collections in Java.
•	Significance:
o	Concurrency safe: Fail-safe collections are used in multithreading environments, where the collection might be accessed or modified by multiple threads simultaneously.
o	No immediate error: Instead of throwing an exception, it lets the iteration continue without interruption, but this can lead to inconsistencies in data.
•	Example of Fail-Safe:
java
Copy code
import java.util.*;

public class FailSafeExample {
    public static void main(String[] args) {
        List<String> list = new CopyOnWriteArrayList<>();
        list.add("One");
        list.add("Two");
        
        // Fail-safe iterator
        Iterator<String> iterator = list.iterator();
        
        while (iterator.hasNext()) {
            String value = iterator.next();
            if (value.equals("One")) {
                list.remove("Two");  // Safe to modify during iteration
            }
        }
        System.out.println(list);  // List can be modified safely
    }
}
________________________________________
Key Differences Between Fail-Fast and Fail-Safe:
Feature	Fail-Fast	Fail-Safe
Behavior on Modification	Throws ConcurrentModificationException	Allows modification without exception
Used for	Single-threaded environments or cases where you want to detect errors immediately	Multi-threaded environments where you want to safely modify the collection during iteration
Performance Impact	Faster, but less tolerant to concurrent modifications	Slower due to internal copying mechanisms
Examples	ArrayList, HashMap, HashSet	CopyOnWriteArrayList, CopyOnWriteArraySet
Collection Type	Non-thread-safe collections	Thread-safe collections
Conclusion:
•	Fail-Fast is used when you want immediate detection of errors in single-threaded environments, where any modification during iteration should raise an exception.
•	Fail-Safe is used in multi-threaded environments, where modifications to the collection can safely happen without raising exceptions, but the performance may be slightly impacted due to internal copying.


14. Which Injection is better & when should use what
choosing between constructor injection, field injection, and setter injection depends on various factors like immutability, flexibility, and testability. Here’s when to use each:
1. Constructor Injection:
When to use:
•	Required dependencies: Use constructor injection when the dependencies are mandatory for the class to function correctly. This ensures that all required dependencies are provided at the time of object creation, preventing the creation of an incomplete object.
•	Immutability: Constructor injection promotes immutability, which is a good practice, as the object is fully initialized once it is created.
•	Best for Testability: It makes testing easier because the dependencies are explicitly passed, making them more straightforward to mock or inject in unit tests.
•	Enforces consistency: The class cannot be instantiated without its required dependencies.
Example:
java
Copy code
public class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
2. Field Injection:
When to use:
•	Simplicity: Field injection is the easiest and most concise, where dependencies are injected directly into the fields of a class. It is suitable when you don't need to enforce the immutability of an object and the dependencies are not critical to the class.
•	When dependencies are optional: Use field injection when the dependencies are optional or have default values, meaning the object can function even if a dependency is not provided.
•	When flexibility is needed: It allows Spring (or other frameworks) to handle the injection internally, reducing the need for explicit constructor or setter methods.
•	Not recommended for immutability or testing: It’s generally not ideal for promoting immutability and can make unit testing harder, as it requires reflection.
Example:
java
Copy code
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
3. Setter Injection:
When to use:
•	Optional dependencies: Use setter injection when the dependency is optional, meaning the class can function without it. It allows dependencies to be set after the object is created, offering more flexibility.
•	Post-construction configuration: When you need to set or change dependencies after the object has been constructed, setter injection allows you to configure dependencies after object creation.
•	Lesser immutability: Unlike constructor injection, setter injection allows changing the dependency at any time, so it does not enforce immutability.
•	When lifecycle management is required: It is suitable when you need to perform initialization tasks after the object creation or allow for changing the dependency during the object's lifecycle.
Example:
java
Copy code
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
Summary:
•	Constructor Injection: Use when dependencies are required, the object should be immutable, and you want to ensure correct initialization. It is also test-friendly.
•	Field Injection: Use for simple cases or when you want automatic handling of dependencies, but it can lead to poor testability and lacks immutability.
•	Setter Injection: Use when dependencies are optional, or you want to allow modification of dependencies during the object's lifetime. It’s less strict and offers flexibility.
Generally, constructor injection is preferred for mandatory dependencies, while setter injection is useful for optional dependencies or when dependencies need to change post-construction. Field injection is quick but less ideal for testability and immutability.

15. What is the difference between Dependency Injection and Dependency Inversion Principle









16. is wrapper classes are immutable by default
ChatGPT said:
ChatGPT
Yes, wrapper classes in Java (such as Integer, Double, Character, Boolean, etc.) are immutable by design.
Why are Wrapper Classes Immutable?
1.	Final Fields: All wrapper class fields are declared as final. Once these fields are initialized, their values cannot be changed, which is a key characteristic of immutability.
2.	No Setter Methods: Wrapper classes do not provide any setter methods. This means you cannot modify the internal state of a wrapper object after it has been created.
3.	No Method to Alter Value: Methods in wrapper classes that return values, such as intValue() or doubleValue(), do not modify the internal state of the object but return a copy of the value.
Example:
java
Copy code
Integer x = 10;
x = 20;  // This creates a new Integer object; the original Integer with value 10 is unchanged.
In the above example, when x is reassigned to 20, a new Integer object is created. The original Integer object with the value 10 is not modified.
Characteristics of Immutable Objects:
•	Final class: The class itself is final, preventing subclassing.
•	Final fields: All fields are marked as final.
•	No setter methods: No methods modify the object’s state.
•	Object state is set in constructor: The state is set when the object is created and cannot be changed afterward.
Example of an Immutable Wrapper Class (Integer):
The Integer class in Java is immutable. When you call Integer.valueOf(10) or new Integer(10), a new Integer object is created, and its value cannot be modified.
java
Copy code
Integer i1 = Integer.valueOf(10);
Integer i2 = i1; // i2 references the same object as i1
i1 = 15; // This creates a new Integer object with the value 15, i2 still points to the original object
System.out.println(i1); // Prints: 15
System.out.println(i2); // Prints: 10
In this example, reassigning i1 creates a new Integer object. The object i2 remains unchanged.
Thus, wrapper classes like Integer, Double, Character, etc., are immutable by design, ensuring that their state cannot be altered once they are created.
4o mini
17. Strategy Design pattern
The Strategy Design Pattern is a behavioral design pattern that enables selecting an algorithm's behavior at runtime. In the context of Mastercard card payments, you can use the Strategy Pattern to handle different types of payment processing mechanisms (e.g., credit card, debit card, prepaid card, etc.) or support varying payment gateways, fraud detection, or currency conversion strategies.
Here's an example implementation in Java:
________________________________________
Scenario: Mastercard Payment Strategies
You want to handle different types of payments (e.g., domestic, international, EMV, contactless) in a clean and extensible way.
________________________________________
Step 1: Define the Strategy Interface
public interface PaymentStrategy {
    void processPayment(String cardNumber, double amount);
}
________________________________________
Step 2: Implement Concrete Strategies
Domestic Payment Strategy
public class DomesticPaymentStrategy implements PaymentStrategy {
    @Override
    public void processPayment(String cardNumber, double amount) {
        System.out.println("Processing domestic payment of $" + amount + " for card " + cardNumber);
    }
}
International Payment Strategy
java
Copy code
public class InternationalPaymentStrategy implements PaymentStrategy {
    @Override
    public void processPayment(String cardNumber, double amount) {
        System.out.println("Processing international payment of $" + amount + " for card " + cardNumber);
    }
}
Contactless Payment Strategy
java
Copy code
public class ContactlessPaymentStrategy implements PaymentStrategy {
    @Override
    public void processPayment(String cardNumber, double amount) {
        System.out.println("Processing contactless payment of $" + amount + " for card " + cardNumber);
    }
}
________________________________________
Step 3: Create the Context
The context will use the strategy dynamically.
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
________________________________________
Step 4: Test the Strategy Pattern
Example Usage
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
________________________________________
Output
plaintext
Copy code
Processing domestic payment of $100.0 for card 1234-5678-9012-3456
Processing international payment of $200.0 for card 1234-5678-9012-3456
Processing contactless payment of $50.0 for card 1234-5678-9012-3456

18.Can This Be Improved?
If you want to avoid burdening the client with the responsibility of deciding and instantiating the strategy, you can introduce a factory or configuration mechanism to determine the strategy dynamically based on some input or conditions.
Improved Version: Strategy Selection with Factory
You can use a factory to encapsulate the creation of the correct strategy based on input:
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
________________________________________
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
________________________________________
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
________________________________________
Output
plaintext
Copy code
Processing domestic payment of $100.0 for card 1234-5678-9012-3456
Processing international payment of $200.0 for card 1234-5678-9012-3456
Processing contactless payment of $50.0 for card 1234-5678-9012-3456
________________________________________
Benefits of Using a Factory
Simplifies the client code: The client no longer needs to know about specific strategy classes or create their instances.
Encapsulates decision logic: The logic for choosing the appropriate strategy is centralized in the factory.
Extensibility: Adding a new strategy only requires modifying the factory, without changing client code or context.
This way, the client simply provides input (e.g., "domestic", "international") instead of directly working with strategy objects.


19.DPs. 
Creational: 
Singleton: GFG
Factory: GFG, Versha
Abstract Factory: GFG
 Builder: Versha, https://www.youtube.com/watch?v=qOLRxN5eVC0&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=26
Prototype: ChatGPT
Behavioral:
 Observer: https://www.youtube.com/watch?v=Ep9_Zcgst3U&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=5
State
Strategy: GFG, https://www.youtube.com/watch?v=u8DttUrXtEw&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=4
Chain of Responsibility
Strucutural: 
Decorator: https://www.youtube.com/watch?v=w6a9MXUwcfY&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=6
 Facade: https://www.youtube.com/watch?v=GYaBXK54eLo&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=29
Proxy: https://www.youtube.com/watch?v=9MxHKlVc6ZM&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=15
Adaptor: https://www.youtube.com/watch?v=8wrg7IsILhQ&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=23
Composite:
Bridge: https://www.youtube.com/watch?v=SOw1_W0taBg&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=30


20.SOLID:
21. Completable Feature
2 videos of java teche https://www.youtube.com/watch?v=oFRtBuRviHM

22. What is JFR toold:
Java Application Flight Recorder introduced in java 11 Version which is used for collecting diagnostic profiling data about application running performance.
23. SYSTEM Design Prep Documentation
https://levelup.gitconnected.com/system-design-master-template-how-to-answer-any-system-design-interview-question-ee5dc332acd5
Design Gup Co Founder
23. Choosing Between SQL Vs NOSQL:






