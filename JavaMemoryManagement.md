## Basics of Java Memory Management

Java Memory Model
Java uses an automatic memory management system known as Garbage Collection (GC). The JVM manages memory allocation and deallocation, so developers don’t need to explicitly free up memory.


## Types of Memory in Java
 Heap Memory:

Used to store objects and class instances.
Managed by the Garbage Collector (GC).
Divided into generations for optimization.
Stack Memory:

Stores method calls, local variables, and reference variables.
Each thread has its own stack.
Memory is allocated/deallocated in a Last In, First Out (LIFO) manner.
Metaspace:

Replaced PermGen in Java 8+.
Stores class metadata, static variables, and method definitions.
Grows dynamically and is managed by the JVM.
Program Counter (PC) Register:

Holds the address of the current instruction being executed.
Native Method Stack:

Used for native method calls (e.g., code written in C/C++ and invoked via JNI).
2. Java Memory Regions
Heap Memory Structure (Generational Model)
The Java heap is divided into 3 generations:

Young Generation (Eden + Survivor Spaces):

New objects are allocated here.
Divided into:
Eden Space: New objects are first allocated here.
Survivor Spaces (S0, S1): Objects that survive garbage collection in Eden are moved here.
Old Generation (Tenured Space):

Objects that survive multiple garbage collections (long-lived objects) are promoted to this space.
Permanent Generation (Deprecated) → Metaspace:

Stores class metadata and static fields.
In Java 8+, Metaspace replaced PermGen for better flexibility and memory management.
Non-Heap Memory:
Includes Metaspace, Thread stacks, PC Register, and native memory.
3. Memory Allocation in Java
Object Allocation
Objects are allocated in the heap.
JVM determines whether the object is short-lived or long-lived to place it in the appropriate generation.
Primitive Data Types and References
Primitive variables (int, char, boolean, etc.) are stored in stack memory.
References to objects are stored in the stack, while the actual objects are in the heap.
4. Garbage Collection (GC)
What is Garbage Collection?
GC automatically removes objects that are no longer referenced to free up heap memory.
The JVM manages GC using different algorithms.
Garbage Collector Algorithms
Mark-and-Sweep:

Objects that are reachable are marked.
Unreachable objects are swept (deleted).
Stop-the-World (STW):

All application threads are paused during GC.
Minor GC:

Collects garbage from the Young Generation.
Short-lived objects are removed quickly.
Major GC (Full GC):

Collects garbage from the Old Generation and Young Generation.
Concurrent GC:

Works concurrently with the application threads to minimize pause times.
5. JVM Garbage Collectors
The JVM offers multiple GC implementations:

Serial GC:

Uses a single thread for GC.
Suitable for single-threaded applications.
Parallel GC (Throughput Collector):

Uses multiple threads to speed up GC.
Optimized for high throughput.
Concurrent Mark-Sweep (CMS) GC:

Reduces pause times by running concurrently with the application.
G1 Garbage Collector:

Breaks the heap into regions and collects garbage incrementally.
Optimized for low-latency applications.
Z Garbage Collector (ZGC):

Designed for very large heaps.
Provides very low pause times.
Shenandoah GC:

Similar to ZGC but targets consistent pause times.
How to Select a Garbage Collector?
Default: G1GC in Java 9+.
Low Pause Time: CMS or G1GC.
High Throughput: Parallel GC.
Large Heap: ZGC or Shenandoah.
6. Advanced Memory Management Concepts
Object Reachability
Objects have different states of reachability:

Strong Reference:

Prevents GC from collecting the object.
Weak Reference:

Collected in the next GC cycle.
Soft Reference:

Cleared only when the JVM runs low on memory.
Phantom Reference:

Used for post-mortem cleanup.
Escape Analysis
JVM optimization technique to analyze if an object can be allocated on the stack instead of the heap.
Memory Leaks in Java
Memory leaks occur when objects are no longer needed but still referenced.

Causes:
Unclosed streams.
Static references to objects.
Improper use of collections.
Tools to detect memory leaks:
VisualVM, Eclipse MAT, YourKit, JProfiler.
7. Performance Tuning and Optimization
JVM Options for GC Tuning
You can configure garbage collectors and memory sizes using JVM options:

Heap Size:

-Xms: Initial heap size.
-Xmx: Maximum heap size.
GC Options:

-XX:+UseG1GC: Use G1 Garbage Collector.
-XX:+UseParallelGC: Use Parallel GC.
-XX:+UseZGC: Use ZGC.
Logging:

-XX:+PrintGCDetails: Print detailed GC logs.
-Xlog:gc: Unified GC logging in Java 9+.
8. Tools for Memory Management
Monitoring Tools
JConsole: Monitor heap, threads, and GC activity.
VisualVM: Profile JVM memory and detect memory leaks.
Eclipse MAT (Memory Analyzer Tool): Analyze heap dumps for leaks.
JProfiler and YourKit: Advanced profiling tools.
Heap Dump Analysis
A heap dump is a snapshot of the heap at a point in time.
Used to analyze memory usage and locate memory leaks.
Conclusion
Java’s automatic memory management simplifies application development but requires a solid understanding of:

Heap and stack allocation.
Garbage collection algorithms.
JVM tuning and performance optimization.
By leveraging tools and JVM options, developers can optimize their application's memory usage and minimize GC pauses effectively.





In Java's Garbage Collection (GC) process, the phrase "objects that are reachable are marked" refers to identifying objects in the heap memory that are still accessible (reachable) by the program. Objects that cannot be reached are considered garbage and will be eligible for collection (deletion).




