# COMPLETE JAVA NOTES

# Part 3 — Core Java

**Modern baseline:** Java SE 27  
**Purpose:** Learning • Revision • Interviews • Practical Development

---

# Table of Contents

1. Strings
2. Arrays
3. Wrapper Classes
4. Packages and Imports
5. Exception Handling
6. Enums
7. Nested and Inner Classes
8. Generics
9. Collections Framework
10. Important Relationships
11. Common Mistakes
12. Interview & Revision Points
13. Quick Revision
14. Practice Questions
15. References

---

# 1. Strings

## What is a String?

A `String` represents a sequence of characters.

In Java:

```java
String name = "Satyam";
```

`String` is a class in `java.lang`, so it is automatically available without an explicit import.

---

## 1.1 Strings are immutable

A `String` object cannot be changed after it is created.

Example:

```java
String name = "Java";

name.concat(" Programming");

System.out.println(name);
```

The original `String` remains unchanged because `concat()` returns a new string.

Correct approach:

```java
String name = "Java";

name = name.concat(" Programming");

System.out.println(name);
```

Think:

```text
"Java"
  │
  │ concat(...)
  ▼
"Java Programming"

Original object: unchanged
```

---

## 1.2 Why immutability matters

String immutability provides useful properties such as:

- safe sharing of string values
- predictable behavior
- easier reasoning about strings
- suitability for use as map keys
- support for string pooling

It also means repeated string modifications can create many temporary objects.

For heavy mutable text construction, use `StringBuilder` or an appropriate alternative.

---

# 2. String Literals and the String Pool

Example:

```java
String first = "Java";
String second = "Java";
```

The Java runtime can reuse the same interned string object for identical string literals.

Conceptually:

```text
first  ──┐
         ├──► "Java"
second ──┘
```

This is why:

```java
System.out.println(first == second);
```

can print:

```text
true
```

But do **not** conclude that `==` is the correct way to compare string contents.

---

# 3. `==` vs `equals()`

This is one of the most important Java fundamentals.

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

The important distinction is:

```text
==        → compares references
equals()  → compares logical content for String
```

The output is therefore:

```text
false
true
```

For content comparison:

```java
a.equals(b)
```

Use `==` when you intentionally want reference identity.

---

# 4. String Methods

Common methods include:

```java
length()
isEmpty()
isBlank()
charAt()
substring()
contains()
startsWith()
endsWith()
indexOf()
lastIndexOf()
replace()
toLowerCase()
toUpperCase()
trim()
strip()
split()
```

Example:

```java
String message = "  Hello Java  ";

System.out.println(message.strip());
System.out.println(message.toUpperCase());
```

---

## 4.1 `isEmpty()` vs `isBlank()`

```java
String a = "";
String b = "   ";
```

`isEmpty()` checks whether length is zero.

`isBlank()` checks whether the string is empty or contains only Unicode whitespace.

So:

```java
a.isEmpty()  // true
b.isEmpty()  // false
b.isBlank()  // true
```

`isBlank()` was introduced in Java 11.

---

# 5. String Concatenation

Example:

```java
String name = "Satyam";
String message = "Hello " + name;
```

The `+` operator can concatenate strings.

For more complex repeated mutation:

```java
StringBuilder builder = new StringBuilder();

builder.append("Java");
builder.append(" ");
builder.append("Programming");

String result = builder.toString();
```

---

# 6. `StringBuilder` vs `StringBuffer`

Both represent mutable character sequences.

| `StringBuilder` | `StringBuffer` |
|---|---|
| Mutable | Mutable |
| Generally preferred for single-threaded use | Synchronized methods |
| Usually better choice for ordinary local string building | Useful in legacy/thread-sharing scenarios where its synchronization semantics are specifically desired |

Do not automatically use `StringBuffer` simply because "it is thread-safe."

---

# 7. Arrays

Java arrays are objects that hold a fixed number of components of one array type.

The Java Language Specification explicitly defines arrays as dynamically created objects.

Example:

```java
int[] numbers = {10, 20, 30, 40};
```

Indexes begin at zero:

```text
Index:   0    1    2    3
Value:  10   20   30   40
```

---

# 8. Creating Arrays

Declaration:

```java
int[] numbers;
```

Creation:

```java
numbers = new int[5];
```

Declaration and creation:

```java
int[] numbers = new int[5];
```

Initialization:

```java
int[] numbers = {10, 20, 30};
```

---

# 9. Array Length

Use:

```java
numbers.length
```

Notice that arrays have a **field**, not a `length()` method.

Correct:

```java
numbers.length
```

Incorrect:

```java
numbers.length()
```

---

# 10. Array Default Values

When an array is created, its components receive default values.

For example:

```java
int[] numbers = new int[3];
```

Initially:

```text
0 0 0
```

For:

```java
boolean[] flags = new boolean[3];
```

the values are:

```text
false false false
```

For reference arrays:

```java
String[] names = new String[3];
```

the components initially contain:

```text
null null null
```

---

# 11. Array Bounds

This:

```java
int[] values = {10, 20, 30};
```

has valid indexes:

```text
0
1
2
```

Accessing:

```java
values[3]
```

causes:

```text
ArrayIndexOutOfBoundsException
```

---

# 12. Multidimensional Arrays

Java supports arrays of arrays.

Example:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};
```

Conceptually:

```text
matrix
  │
  ├──► [1, 2, 3]
  │
  └──► [4, 5, 6]
```

Because Java's multidimensional arrays are arrays of arrays, rows can have different lengths:

```java
int[][] values = {
    {1, 2},
    {3, 4, 5}
};
```

This is valid.

---

# 13. Arrays and Reference Types

This is valid:

```java
Object value = new int[] {1, 2, 3};
```

Because arrays are objects and have `Object` as a superclass.

Arrays can also have covariant reference types:

```java
Number[] numbers = new Integer[3];
```

But this can cause:

```java
numbers[0] = Double.valueOf(10.5);
```

to throw:

```text
ArrayStoreException
```

This is an important difference between array covariance and generic type invariance.

---

# 14. Wrapper Classes

Primitive types are not classes.

Java provides wrapper classes for primitive values.

```text
byte      → Byte
short     → Short
int       → Integer
long      → Long
float     → Float
double    → Double
char      → Character
boolean   → Boolean
```

---

# 15. Why Wrapper Classes?

They are useful when an API requires objects rather than primitives.

For example, generics work with reference types:

```java
List<Integer> numbers = new ArrayList<>();
```

You cannot write:

```java
List<int> numbers;
```

---

# 16. Autoboxing

Autoboxing converts a primitive value to the corresponding wrapper type.

```java
int number = 10;

Integer boxed = number;
```

Conceptually:

```text
int
 ↓
Integer
```

---

# 17. Unboxing

Unboxing converts a wrapper object to its primitive value.

```java
Integer boxed = 10;

int number = boxed;
```

Conceptually:

```text
Integer
   ↓
 int
```

---

# 18. The `null` Trap with Unboxing

This is dangerous:

```java
Integer value = null;

int number = value;
```

The unboxing operation requires a non-null wrapper value and can result in:

```text
NullPointerException
```

Remember:

```text
int        → cannot be null
Integer    → can be null
```

---

# 19. Integer Caching

Java implementations maintain certain commonly used boxed integer values through caching semantics specified by the language/API.

Therefore:

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
```

may be `true` for values within the guaranteed cache range.

But never use `==` to compare wrapper contents.

Use:

```java
a.equals(b)
```

or appropriate value-comparison methods.

---

# 20. Packages

A package groups related types and provides a namespace.

Example:

```java
package com.example.app;
```

A package helps:

- organize code
- avoid naming conflicts
- control package-level access
- structure large applications

---

# 21. Package Declaration

Usually the package declaration appears near the beginning of the source file:

```java
package com.example.shop;

public class Product {
}
```

The package declaration determines the package of the compilation unit.

---

# 22. Imports

Suppose you want to use:

```java
ArrayList
```

You can write:

```java
import java.util.ArrayList;
```

Then:

```java
ArrayList<String> names = new ArrayList<>();
```

Without the import:

```java
java.util.ArrayList<String> names =
        new java.util.ArrayList<>();
```

---

# 23. `import` Does Not Copy Code

An important beginner misconception:

```java
import java.util.ArrayList;
```

does not copy the class into your source file.

It makes a type name available through a shorter name.

---

# 24. `java.lang`

Classes in `java.lang` are automatically available.

Examples:

```text
String
Object
System
Math
Integer
Exception
```

So you do not normally write:

```java
import java.lang.String;
```

---

# 25. Static Imports

Java also supports static imports.

Example:

```java
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;
```

Then:

```java
double circumference = 2 * PI * 5;
double root = sqrt(25);
```

Use static imports carefully; excessive use can make the source less obvious.

---

# 26. Exception Handling

An exception represents an abnormal condition or event that affects the normal flow of program execution.

Java's exception model is defined by both the language specification and the Java API.

A simplified hierarchy:

```text
Throwable
├── Error
└── Exception
    ├── RuntimeException
    └── other checked exceptions
```

---

# 27. `Error` vs `Exception`

`Error` generally represents serious problems from which ordinary application code is not normally expected to recover.

Examples include:

```text
OutOfMemoryError
StackOverflowError
```

`Exception` represents conditions that application code may need to handle or propagate.

---

# 28. Checked vs Unchecked Exceptions

A checked exception is an exception subject to compile-time checking under Java's exception rules.

Examples:

```text
IOException
SQLException
```

Unchecked exceptions include:

```text
RuntimeException
and its subclasses
```

Examples:

```text
NullPointerException
IllegalArgumentException
IndexOutOfBoundsException
```

---

# 29. `try-catch`

Example:

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException exception) {
    System.out.println("Cannot divide by zero");
}
```

Flow:

```text
try
 ↓
exception occurs
 ↓
matching catch
 ↓
catch executes
```

---

# 30. Multiple Catch Blocks

```java
try {
    // risky code
} catch (NumberFormatException exception) {
    // handle formatting problem
} catch (IllegalArgumentException exception) {
    // handle more general argument problem
}
```

Catch more specific types before broader applicable types.

This is invalid:

```java
try {
} catch (Exception exception) {
} catch (RuntimeException exception) {
}
```

because the second catch is unreachable.

---

# 31. Multi-Catch

Java supports multi-catch:

```java
try {
    // ...
} catch (IOException | SecurityException exception) {
    System.out.println("Operation failed");
}
```

Useful when different exception types require the same handling.

---

# 32. `finally`

`finally` is commonly used for cleanup that should occur regardless of whether the protected operation completes normally or abruptly.

Example:

```java
try {
    System.out.println("Working");
} finally {
    System.out.println("Cleanup");
}
```

However, for resources such as files and streams, **try-with-resources** is generally preferable.

---

# 33. Try-with-Resources

Example:

```java
try (BufferedReader reader =
         Files.newBufferedReader(Path.of("data.txt"))) {

    System.out.println(reader.readLine());

} catch (IOException exception) {
    exception.printStackTrace();
}
```

The resource is automatically closed.

The resource must implement `AutoCloseable`.

This is generally the preferred modern pattern for resource management.

---

# 34. `throw`

Use `throw` to explicitly throw an exception.

```java
if (amount <= 0) {
    throw new IllegalArgumentException("Amount must be positive");
}
```

---

# 35. `throws`

`throws` declares exceptions that a method may propagate.

Example:

```java
void readFile() throws IOException {
    // ...
}
```

Important:

```text
throw
 ↓
actually throws an exception object

throws
 ↓
declares possible propagated exceptions
```

---

# 36. Custom Exceptions

You can create domain-specific exceptions.

Example:

```java
class InsufficientBalanceException extends Exception {

    InsufficientBalanceException(String message) {
        super(message);
    }
}
```

Use:

```java
if (amount > balance) {
    throw new InsufficientBalanceException("Insufficient balance");
}
```

Don't create custom exception classes unnecessarily; use existing standard exceptions when they accurately represent the failure.

---

# 37. Exception Propagation

Suppose:

```java
methodA()
   ↓
methodB()
   ↓
methodC()
```

If `methodC()` throws an exception and does not handle it, it can propagate to its caller.

Conceptually:

```text
methodC
   │
   │ exception
   ▼
methodB
   │
   ▼
methodA
```

The exception can continue upward until matching handling is found or execution terminates.

---

# 38. Exception Design Principles

Good exception handling should:

- handle an exception where meaningful recovery is possible
- preserve useful diagnostic information
- avoid swallowing exceptions silently
- avoid using exceptions for ordinary control flow
- choose exception types that accurately communicate the problem
- use try-with-resources for closable resources

Bad:

```java
try {
    doSomething();
} catch (Exception exception) {
}
```

The exception disappears and debugging becomes difficult.

---

# 39. Enums

An enum defines a fixed set of named constants.

Example:

```java
enum OrderStatus {
    CREATED,
    PAID,
    SHIPPED,
    DELIVERED
}
```

Use:

```java
OrderStatus status = OrderStatus.PAID;
```

Enums are types, not simply integer constants.

---

# 40. Enum Methods

Useful methods include:

```java
values()
valueOf(String)
name()
ordinal()
```

Example:

```java
for (OrderStatus status : OrderStatus.values()) {
    System.out.println(status);
}
```

---

# 41. Enum Fields and Methods

Enums can contain state and behavior.

```java
enum Priority {

    LOW(1),
    MEDIUM(2),
    HIGH(3);

    private final int level;

    Priority(int level) {
        this.level = level;
    }

    public int level() {
        return level;
    }
}
```

Usage:

```java
System.out.println(Priority.HIGH.level());
```

---

# 42. Don't Persist Enum Ordinals

Avoid using:

```java
status.ordinal()
```

as a database identifier or long-term external representation.

Changing the enum declaration order can change ordinals.

Use an explicit stable value instead:

```java
enum Status {

    CREATED("C"),
    PAID("P"),
    SHIPPED("S");

    private final String code;

    Status(String code) {
        this.code = code;
    }

    public String code() {
        return code;
    }
}
```

---

# 43. Nested and Inner Classes

Java allows a class to be declared inside another class.

```text
Outer class
├── static nested class
└── inner class
```

The term **nested class** is the general category.

An **inner class** is a non-static nested class.

---

# 44. Static Nested Class

Example:

```java
class Outer {

    static class Nested {
        void show() {
            System.out.println("Nested");
        }
    }
}
```

Usage:

```java
Outer.Nested nested = new Outer.Nested();
nested.show();
```

A static nested class does not require an enclosing `Outer` instance merely to create the nested object.

---

# 45. Inner Class

Example:

```java
class Outer {

    private int value = 100;

    class Inner {

        void show() {
            System.out.println(value);
        }
    }
}
```

Create:

```java
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
```

An inner class is associated with an enclosing instance and can access members of that enclosing instance according to Java's access rules.

---

# 46. Local Classes

A class can be declared inside a method or block.

```java
void process() {

    class Helper {
        void run() {
            System.out.println("Running");
        }
    }

    Helper helper = new Helper();
    helper.run();
}
```

This is called a local class.

---

# 47. Anonymous Classes

An anonymous class creates an unnamed class and an instance at the same time.

Example:

```java
Runnable task = new Runnable() {

    @Override
    public void run() {
        System.out.println("Running");
    }
};
```

Anonymous classes remain useful for certain cases, although lambdas are preferable when the target is a functional interface.

---

# 48. Generics

Generics allow classes, interfaces and methods to operate with types while providing compile-time type safety.

Example:

```java
List<String> names = new ArrayList<>();

names.add("Satyam");
names.add("Sonam");
```

Now the compiler knows the list is intended to contain `String` values.

---

# 49. Why Generics?

Without generics, older collection code often required casts:

```java
List names = new ArrayList();
names.add("Java");

String name = (String) names.get(0);
```

With generics:

```java
List<String> names = new ArrayList<>();

String name = names.get(0);
```

Benefits include:

- stronger compile-time checking
- fewer explicit casts
- clearer APIs
- better type safety

---

# 50. Generic Class

Example:

```java
class Box<T> {

    private T value;

    Box(T value) {
        this.value = value;
    }

    T getValue() {
        return value;
    }
}
```

Usage:

```java
Box<String> textBox = new Box<>("Java");
Box<Integer> numberBox = new Box<>(100);
```

Here `T` is a type parameter.

---

# 51. Generic Method

A method can declare its own type parameter.

```java
static <T> void print(T value) {
    System.out.println(value);
}
```

Call:

```java
print("Java");
print(100);
```

---

# 52. Bounded Type Parameters

Suppose a method should accept only numeric types:

```java
static <T extends Number> double doubleValue(T value) {
    return value.doubleValue();
}
```

`T extends Number` means the type argument must be `Number` or a subtype.

---

# 53. Wildcards

A wildcard represents an unknown type.

```java
List<?> values;
```

This means:

> a list of some unknown type.

---

# 54. Upper-Bounded Wildcard

```java
List<? extends Number>
```

This means a list whose element type is `Number` or some subtype.

It is useful when you primarily want to **read** values as `Number`.

---

# 55. Lower-Bounded Wildcard

```java
List<? super Integer>
```

This means a list whose type is `Integer` or a supertype of `Integer`.

This is useful when you want to safely add `Integer` values.

---

# 56. PECS

A useful generics rule is:

```text
Producer Extends
Consumer Super
```

### Producer

If you primarily read from a generic structure:

```java
List<? extends Number>
```

### Consumer

If you primarily put values into it:

```java
List<? super Integer>
```

PECS is a guideline for understanding wildcard variance, not a replacement for understanding the actual type rules.

---

# 57. Type Erasure

Java generics are implemented primarily through **type erasure**.

Generic type information is used heavily by the compiler for type checking, while most ordinary generic type arguments are not represented as distinct runtime types.

For example:

```java
List<String>
List<Integer>
```

do not become two fundamentally different generic runtime classes.

This is why:

```java
new T()
```

is generally not possible for an unconstrained type parameter.

---

# 58. Collections Framework

The Collections Framework provides interfaces and implementations for storing groups of objects.

A simplified model:

```text
Collection
├── List
├── Set
└── Queue / Deque

Map
```

Important:

> `Map` is part of the Collections Framework but is **not** a subtype of `Collection`.

---

# 59. `List`

A `List` is ordered and typically permits duplicates.

Examples:

```java
List<String> names = new ArrayList<>();
```

```java
List<String> names = List.of("A", "B", "C");
```

The Java SE API specifies `List` as an ordered collection with positional access and generally allows duplicate elements.

---

# 60. `ArrayList`

`ArrayList` is a resizable-array implementation of `List`.

Example:

```java
List<String> names = new ArrayList<>();

names.add("Java");
names.add("Python");
names.add("Go");
```

It provides fast indexed access in typical usage and is the default list implementation for many cases.

---

# 61. `LinkedList`

`LinkedList` is a doubly linked implementation that also implements `Deque`.

It can be useful when operations at the ends are important, but don't assume it is automatically faster than `ArrayList`.

For many ordinary list workloads:

```text
ArrayList
```

is the natural starting point.

---

# 62. `Set`

A `Set` represents a collection that does not permit duplicate elements according to its contract.

Examples:

```text
HashSet
LinkedHashSet
TreeSet
EnumSet
```

The API defines `Set` around the mathematical set abstraction of no duplicate elements.

---

# 63. `HashSet`

Example:

```java
Set<String> names = new HashSet<>();

names.add("Java");
names.add("Java");
names.add("Python");

System.out.println(names);
```

The duplicate `"Java"` is not stored twice.

`HashSet` does not provide a general sorted-order guarantee.

---

# 64. `LinkedHashSet`

`LinkedHashSet` maintains a predictable encounter/insertion-linked iteration order.

Use it when you want set semantics while preserving insertion order.

---

# 65. `TreeSet`

`TreeSet` maintains elements according to its ordering rules.

Example:

```java
Set<Integer> numbers = new TreeSet<>();

numbers.add(30);
numbers.add(10);
numbers.add(20);
```

Iteration produces sorted order according to the set's ordering.

---

# 66. `Map`

A `Map` stores key-value mappings:

```text
key → value
```

Example:

```java
Map<String, Integer> ages = new HashMap<>();

ages.put("Satyam", 27);
ages.put("Sonam", 25);
```

Keys are used to locate associated values.

---

# 67. `HashMap`

`HashMap` is a hash-table-based `Map` implementation.

Example:

```java
Map<String, Integer> scores = new HashMap<>();

scores.put("Java", 90);
scores.put("SQL", 85);

System.out.println(scores.get("Java"));
```

Do not rely on ordinary `HashMap` iteration order as a stable order guarantee.

---

# 68. `LinkedHashMap`

`LinkedHashMap` maintains a linked iteration order.

This is useful when predictable encounter order matters.

---

# 69. `TreeMap`

`TreeMap` keeps keys ordered according to its ordering rules.

```java
Map<String, Integer> scores = new TreeMap<>();

scores.put("Java", 90);
scores.put("C", 70);
scores.put("Python", 85);
```

Iteration is key-ordered.

---

# 70. Queue and Deque

A queue represents elements awaiting processing.

Example:

```java
Queue<String> queue = new ArrayDeque<>();

queue.offer("A");
queue.offer("B");

System.out.println(queue.poll());
```

`Deque` means double-ended queue:

```java
Deque<String> deque = new ArrayDeque<>();
```

Elements can be added/removed at both ends.

---

# 71. `ArrayDeque`

`ArrayDeque` is a general-purpose deque implementation.

It is commonly useful for:

- queue behavior
- stack-like behavior
- double-ended operations

Example stack-like use:

```java
Deque<Integer> stack = new ArrayDeque<>();

stack.push(10);
stack.push(20);

System.out.println(stack.pop());
```

For new code, prefer `Deque`/`ArrayDeque` over the legacy `Stack` class for stack behavior.

---

# 72. `Collection` vs `Collections`

Do not confuse:

```text
Collection
```

with:

```text
Collections
```

`Collection` is an interface.

`Collections` is a utility class containing static methods for working with collections.

Example:

```java
Collections.sort(names);
```

Modern code often uses:

```java
names.sort(...)
```

or immutable factory methods where appropriate.

---

# 73. Immutable / Unmodifiable Collections

Java provides convenient factories:

```java
List<String> names = List.of("A", "B", "C");
Set<Integer> numbers = Set.of(1, 2, 3);
Map<String, Integer> values = Map.of(
    "A", 1,
    "B", 2
);
```

These produce unmodifiable collections.

For example:

```java
names.add("D");
```

throws:

```text
UnsupportedOperationException
```

They also reject null elements/keys/values as specified by their respective factory APIs.

---

# 74. Iteration

A collection can be traversed using an enhanced `for` loop:

```java
for (String name : names) {
    System.out.println(name);
}
```

Or:

```java
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

For modern stream-oriented processing, the Stream API will be covered in Part 4.

---

# 75. `Comparable` and Natural Ordering

A class can implement `Comparable<T>` to define its natural ordering.

Example:

```java
class Product implements Comparable<Product> {

    private final String name;

    Product(String name) {
        this.name = name;
    }

    @Override
    public int compareTo(Product other) {
        return name.compareTo(other.name);
    }
}
```

---

# 76. `Comparator`

A `Comparator` provides an external ordering strategy.

Example:

```java
Comparator<Product> byName =
        Comparator.comparing(Product::getName);
```

You can then sort using:

```java
products.sort(byName);
```

This is especially useful when the same type needs several possible orderings.

---

# 77. `equals()` and Collections

Collections frequently depend on equality and hash-code contracts.

For hash-based collections:

```text
equals()
   +
hashCode()
   ↓
correct behavior in HashMap / HashSet
```

For example, if you store custom objects in a `HashSet`, implement equality consistently when logical value equality is intended.

This directly connects Part 2's `Object.equals()` and `hashCode()` concepts with Part 3 collections.

---

# 78. Generics + Collections

This is one of the most important combinations in Java.

Instead of:

```java
List list = new ArrayList();
```

prefer:

```java
List<String> names = new ArrayList<>();
```

Now the compiler can reject:

```java
names.add(100);
```

This gives you:

```text
Generics
   ↓
Compile-time type safety
   ↓
Collections become safer
```

---

# 79. Core Java Concept Map

```text
String
  │
  ├── immutable
  ├── equals()
  └── StringBuilder

Array
  │
  ├── fixed length
  ├── indexed
  └── object

Wrapper
  │
  ├── boxing
  └── unboxing

Package
  │
  ├── namespace
  └── access organization

Exception
  │
  ├── checked
  ├── unchecked
  ├── throw
  └── try/catch/resources

Enum
  │
  └── fixed set of constants

Nested class
  │
  ├── static nested
  ├── inner
  ├── local
  └── anonymous

Generics
  │
  ├── type safety
  ├── bounds
  └── wildcards

Collections
  │
  ├── List
  ├── Set
  ├── Queue / Deque
  └── Map
```

---

# 80. Common Mistakes

## String mistakes

```java
a == b
```

is not the normal way to compare string contents.

Use:

```java
a.equals(b)
```

---

## Array mistakes

```java
array.length()
```

is wrong.

Use:

```java
array.length
```

---

## Wrapper mistakes

Be careful:

```java
Integer number = null;
int value = number;
```

Unboxing can throw `NullPointerException`.

---

## Exception mistakes

Avoid:

```java
catch (Exception exception) {
}
```

without a deliberate reason.

---

## Enum mistakes

Do not use:

```java
enumValue.ordinal()
```

as a long-term stable database or API identifier.

---

## Generics mistakes

Avoid raw types:

```java
List list;
```

Prefer:

```java
List<String> list;
```

---

## Collection mistakes

Do not assume:

```text
HashMap → sorted
HashSet → sorted
```

They are not.

Use:

```text
TreeMap → ordered keys
TreeSet → ordered elements
```

when sorted ordering is required.

---

# 81. Interview & Revision Points

### Strings

1. Why is `String` immutable?
2. Difference between `==` and `equals()`?
3. What is the string pool?
4. Difference between `String`, `StringBuilder` and `StringBuffer`?
5. Difference between `isEmpty()` and `isBlank()`?

### Arrays

6. Are arrays objects in Java?
7. Why does an array use `length` rather than `length()`?
8. What happens on invalid array access?
9. Can Java have jagged multidimensional arrays?
10. Why can array covariance cause `ArrayStoreException`?

### Wrappers

11. What is autoboxing?
12. What is unboxing?
13. Why can unboxing `null` cause an exception?
14. Why should wrapper contents not normally be compared using `==`?

### Packages

15. What does a package provide?
16. What does `import` do?
17. Why are `java.lang` classes available without imports?
18. What is static import?

### Exceptions

19. Checked vs unchecked exceptions?
20. Difference between `throw` and `throws`?
21. What is exception propagation?
22. Why is try-with-resources preferred for closeable resources?
23. What is multi-catch?
24. Why should exceptions not be silently swallowed?

### Enums

25. Why are enums preferable to many integer constants?
26. What is `ordinal()`?
27. Why should ordinal values not be used as persistent identifiers?
28. Can enums contain methods and fields?

### Nested classes

29. Difference between nested and inner class?
30. What is a static nested class?
31. What is an inner class?
32. What is an anonymous class?

### Generics

33. Why do we use generics?
34. What is a bounded type parameter?
35. What is `? extends`?
36. What is `? super`?
37. Explain PECS.
38. What is type erasure?
39. Why can't generics use primitive type arguments directly?

### Collections

40. List vs Set vs Map?
41. Why is Map not a subtype of Collection?
42. ArrayList vs LinkedList?
43. HashSet vs LinkedHashSet vs TreeSet?
44. HashMap vs LinkedHashMap vs TreeMap?
45. Why is `ArrayDeque` commonly preferred over legacy `Stack`?
46. Why do `equals()` and `hashCode()` matter for hash collections?
47. What is the difference between `Comparable` and `Comparator`?

---

# 82. Quick Revision

## String

```text
String → immutable
==     → reference identity
equals → logical equality
```

## Array

```text
fixed size
zero-based index
array.length
arrays are objects
```

## Wrapper

```text
int ↔ Integer
double ↔ Double
char ↔ Character
boolean ↔ Boolean
```

Autoboxing:

```text
primitive → wrapper
```

Unboxing:

```text
wrapper → primitive
```

---

## Exceptions

```text
Throwable
├── Error
└── Exception
    └── RuntimeException
```

Remember:

```text
throw  → throws an exception
throws → declares propagated exceptions
```

---

## Generics

```text
List<String>
List<Integer>
```

Think:

```text
Generics
   ↓
Compile-time type safety
```

Wildcards:

```text
? extends → producer
? super   → consumer
```

---

## Collections

```text
List
 └── ordered, duplicates typically allowed

Set
 └── no duplicates

Map
 └── key → value

Queue
 └── processing order

Deque
 └── both ends
```

Common implementations:

```text
List   → ArrayList
Set    → HashSet
Map    → HashMap
Deque  → ArrayDeque
```

Use ordered/sorted variants when their guarantees are required:

```text
LinkedHashSet
LinkedHashMap
TreeSet
TreeMap
```

---

# 83. Practice Questions

## Conceptual

1. Explain why `String` is immutable.
2. Explain string pooling and why `==` can appear to work with literals.
3. Explain array covariance with an example of `ArrayStoreException`.
4. Explain autoboxing and unboxing.
5. Explain checked vs unchecked exceptions.
6. Explain try-with-resources.
7. Explain enum vs integer constants.
8. Explain static nested class vs inner class.
9. Explain PECS with an example.
10. Explain why `Map` is not a `Collection`.
11. Explain `ArrayList` vs `LinkedList`.
12. Explain `HashMap` vs `TreeMap`.
13. Explain `Comparable` vs `Comparator`.

---

## Predict the output

### Question 1

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
System.out.println(a.equals(b));
```

### Question 2

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

### Question 3

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
System.out.println(a.equals(b));
```

Explain why identity and value comparison are different.

---

### Question 4

```java
List<String> names = List.of("A", "B");

names.add("C");
```

What happens?

---

### Question 5

```java
Map<String, Integer> scores = new HashMap<>();

scores.put("Java", 90);
scores.put("Java", 95);

System.out.println(scores.get("Java"));
```

---

# 84. Coding Exercises

1. Write a method that reverses a `String` without using a library reverse method.
2. Write a method that checks whether a string is a palindrome.
3. Find the largest element in an integer array.
4. Count duplicate values in an array.
5. Create an enum for order status and use it in a small program.
6. Create a custom checked exception for an invalid transaction.
7. Write a generic `Box<T>` class.
8. Write a generic method that returns the first element of a list.
9. Create a `List`, `Set` and `Map` example using a small employee domain.
10. Sort a collection using both `Comparable` and `Comparator`.
11. Write a program that removes duplicates while preserving insertion order.
12. Build a simple word-frequency counter using `Map<String, Integer>`.

**Try the exercises yourself before checking solutions.**

---

# 85. Key Takeaway

Part 3 gives you the Java APIs and language features that appear constantly in everyday programming.

The most important progression is:

```text
String / Array
      ↓
Objects and wrappers
      ↓
Packages
      ↓
Exceptions
      ↓
Enums / nested types
      ↓
Generics
      ↓
Collections
```

The most important real-world combination is:

```text
Generic types
      +
Collections
      +
equals/hashCode
      +
Exception handling
      ↓
Reliable Java code
```

And this prepares you for Part 4:

```text
Collections
    ↓
Lambda expressions
    ↓
Method references
    ↓
Streams
    ↓
Collectors
```

That is where modern Java becomes much more expressive for data processing.

---

# References

- Java SE 27 & JDK 27 Specifications — Oracle.
- Java Language Specification, Java SE 27.
- JLS Chapter 10 — Arrays.
- Java SE 27 API — `List`.
- Java SE 27 API — `Set`.
- Java SE 27 API — `Map`.