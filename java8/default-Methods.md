
# Default Methods in Java Interfaces

## Problem Before Default Methods (Pre-Java 8)

### Old Interface Design
Imagine we have an interface `Printer` with one method:
```java
public interface Printer {
    void print();
}
```

### Existing Implementations
Two classes implement this interface:
```java
public class TextPrinter implements Printer {
    @Override
    public void print() {
        System.out.println("Printing text...");
    }
}

public class ImagePrinter implements Printer {
    @Override
    public void print() {
        System.out.println("Printing image...");
    }
}
```

### The Problem
Now, you want to add a new method `scan()` to the `Printer` interface. Before default methods, adding a method to the interface forced **all existing implementing classes** to implement the new method, even if they didn’t care about the `scan()` functionality:

```java
public interface Printer {
    void print();
    void scan(); // New method added
}
```

If you try to compile this code, it will fail because `TextPrinter` and `ImagePrinter` don’t implement the new `scan()` method:

```
Error: TextPrinter is not abstract and does not override abstract method scan() in Printer
```

### Result
- You have to modify every existing class (`TextPrinter`, `ImagePrinter`, etc.) to implement the `scan()` method, even if some implementations don’t need it.
- This breaks backward compatibility, causing maintenance issues.

---

## Solution with Default Methods (Java 8+)

With default methods, you can provide a **default implementation** for the new method in the interface itself. This ensures that adding a method does not break existing implementations.

### Updated Interface with Default Method
```java
public interface Printer {
    void print();

    // New method with default implementation
    default void scan() {
        System.out.println("Default scan operation");
    }
}
```

### Existing Implementations Work Without Changes
Since the `scan()` method now has a default implementation, `TextPrinter` and `ImagePrinter` don’t need to implement it unless they want to override the default behavior.

### Result
The existing classes (`TextPrinter` and `ImagePrinter`) work without any changes:

```java
public class TextPrinter implements Printer {
    @Override
    public void print() {
        System.out.println("Printing text...");
    }
}

public class ImagePrinter implements Printer {
    @Override
    public void print() {
        System.out.println("Printing image...");
    }
}
```

If desired, new classes can override the default behavior:

```java
public class AdvancedPrinter implements Printer {
    @Override
    public void print() {
        System.out.println("Printing advanced document...");
    }

    @Override
    public void scan() {
        System.out.println("Scanning advanced document...");
    }
}
```

---

## How It Helps Backward Compatibility

### Pre-Java 8
Adding a new method to an interface forces all implementing classes to change, breaking existing code.

### With Default Methods
Adding a new method with a default implementation allows existing classes to continue working unchanged, preserving backward compatibility.

---

## Summary

- **Without Default Methods:** Adding a new method breaks all existing implementations because they are required to implement the new method.
- **With Default Methods:** The interface provides a default behavior for the new method, so existing implementations don’t need to change unless they want to customize the new method's behavior.
