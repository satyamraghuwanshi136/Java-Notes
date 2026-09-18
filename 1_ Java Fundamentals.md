# COMPLETE JAVA NOTES

# Part 1 — Java Fundamentals

**Modern baseline:** Java SE 27  
**Purpose:** Learning • Revision • Interviews • Practical Development

---

# Table of Contents

1. Introduction to Java
2. Features of Java
3. Java Platform: JDK, JVM and Runtime
4. Installing and Setting Up Java
5. Your First Java Program
6. Compilation and Execution
7. Variables
8. Data Types
9. Operators
10. Type Conversion and Casting
11. Control Flow
12. Interview & Revision Points
13. Quick Revision
14. Practice Questions
15. References

---

# 1. Introduction to Java

## What is Java?

Java is a **general-purpose, strongly typed, class-based programming language** designed to allow developers to write programs that can run on different platforms through the Java runtime environment.

A Java program is generally compiled into **bytecode**, represented by `.class` files, which can then be executed by a Java Virtual Machine (JVM).

A simplified model is:

```text
Java Source Code
      │
      │ javac
      ▼
   Bytecode
   (.class)
      │
      │ JVM
      ▼
Program Execution
```

The Java platform includes much more than the language itself. It includes the JVM, standard libraries/APIs, development tools, specifications, and runtime components.

---

## Why was Java designed this way?

A major goal was to allow developers to compile Java programs into an intermediate representation that can be executed by implementations of the JVM on different operating systems.

This leads to the familiar idea:

> **Write once, run on compatible Java implementations/platforms.**

This should not be interpreted as meaning that every Java program is automatically guaranteed to behave identically everywhere. The operating system, native libraries, filesystem behavior, architecture, environment configuration, and JVM implementation can still matter.

---

# 2. Features of Java

Java is commonly described using several characteristics.

## 2.1 Object-oriented

Java supports object-oriented programming through:

- classes
- objects
- inheritance
- encapsulation
- polymorphism
- abstraction
- interfaces

We will study these in detail in Part 2.

---

## 2.2 Strongly typed

Java associates variables and expressions with types.

```java
int age = 27;
String name = "Satyam";
```

The compiler checks many type-related errors before the program runs.

For example:

```java
int age = "27";   // Compilation error
```

The string `"27"` is not an `int`.

---

## 2.3 Statically typed

Java is primarily **statically typed**.

The type of a variable is normally known at compile time:

```java
int number = 10;
```

This differs from dynamically typed languages where types are generally determined differently at runtime.

Important distinction:

```text
Static typing
    ↓
Many type constraints are checked at compile time.

Strong typing
    ↓
The language enforces type rules and does not freely treat
unrelated values as interchangeable.
```

Do not treat "static typing" and "strong typing" as exactly the same concept.

---

## 2.4 Platform independent bytecode

Java source code is compiled into JVM class files.

```text
Source
  │
  ▼
javac
  │
  ▼
.class bytecode
  │
  ▼
JVM implementation
  │
  ▼
Operating system / hardware
```

The JVM specification defines the abstract machine; individual JVM implementations provide the actual execution environment.

---

## 2.5 Automatic memory management

Java provides automatic memory management through garbage collection.

Developers normally do not explicitly free ordinary objects using a `free()` or `delete` operation.

Example:

```java
Customer customer = new Customer();
```

When an object is no longer reachable by a live reference, it can eventually become eligible for garbage collection.

Important:

> **Eligible for garbage collection does not mean "immediately deleted."**

Garbage collection is discussed more deeply in Part 5.

---

## 2.6 Multithreading and concurrency support

Java provides APIs and language mechanisms for concurrent programming, including:

- `Thread`
- synchronization
- locks
- executors
- concurrent collections
- `CompletableFuture`
- virtual threads

Concurrency will be covered later.

---

## 2.7 Rich standard library

Java provides APIs for areas such as:

- collections
- strings
- mathematics
- dates and times
- I/O
- networking
- concurrency
- security
- reflection
- regular expressions

The Java SE API documentation is the authoritative reference for standard Java APIs.

---

# 3. JDK, JVM and Java Runtime Concepts

One of the most important foundational distinctions in Java is:

```text
JDK
├── Development tools
├── Java compiler
├── Runtime components
└── Java APIs

JVM
└── Executes Java class files

Java runtime environment
└── The components needed to execute Java applications
```

## 3.1 JVM — Java Virtual Machine

The JVM is the abstract machine specified for executing Java bytecode.

Its responsibilities include things such as:

- loading classes
- verifying class files
- executing bytecode
- managing runtime memory
- providing garbage collection facilities
- interacting with the underlying platform

A JVM implementation is specific to a platform/environment, but Java bytecode is designed to be portable across compliant JVM implementations.

---

## 3.2 JDK — Java Development Kit

The JDK is the development kit used to develop Java applications.

It provides tools such as:

```text
javac   → Java compiler
java    → Java application launcher
javadoc → API documentation generator
javap   → Class-file disassembler
jshell  → Java interactive shell
jar     → JAR archive tool
```

The `javac` command reads Java source code and compiles it into class files that run on the JVM.

---

## 3.3 What about JRE?

Older Java documentation frequently presents:

```text
JDK
 └── JRE
      └── JVM
```

This is useful historical terminology, but it can be misleading when discussing modern JDK distributions.

Modern Java distributions should generally be understood in terms of:

- JDK
- runtime image/components
- JVM
- Java SE APIs

rather than assuming that every modern JDK installation contains a separately packaged `jre/` directory.

**Interview note:** If someone asks about JDK vs JRE vs JVM, explain both the traditional model and the modern packaging reality.

---

## 3.4 Simple comparison

| Term | Main purpose |
|---|---|
| JVM | Executes Java bytecode |
| JDK | Develops, compiles, runs and debugs Java applications |
| Java SE | Java platform specification/API platform |
| JRE | Historical/runtime terminology; don't assume a separate modern JRE installation |

---

# 4. Installing and Setting Up Java

For current development, install a suitable JDK rather than trying to install only a runtime.

Oracle provides installation instructions for Windows, macOS and Linux.

After installation, verify the Java version:

```bash
java -version
```

Verify the compiler:

```bash
javac -version
```

Both commands should point to the Java installation you intend to use.

---

## 4.1 PATH and JAVA_HOME

### PATH

`PATH` allows your operating system shell to locate commands such as:

```text
java
javac
jshell
```

### JAVA_HOME

`JAVA_HOME` is a commonly used environment variable that points to the JDK installation directory.

Example conceptually:

```text
JAVA_HOME
   │
   ▼
/path/to/jdk
   │
   └── bin/
        ├── java
        ├── javac
        └── jshell
```

The exact configuration depends on the operating system and shell.

---

# 5. Your First Java Program

Create a file named:

```text
HelloWorld.java
```

Example:

```java
public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("Hello, Java!");
    }
}
```

Output:

```text
Hello, Java!
```

---

# 6. Understanding the First Program

## `public class HelloWorld`

Defines a class named `HelloWorld`.

```java
public class HelloWorld {
}
```

The class is the basic structural unit used to define Java types.

---

## `main`

A conventional application entry point is:

```java
public static void main(String[] args)
```

For ordinary class-based launching, the Java launcher looks for a suitable `main` method.

Break it down:

```text
public
  → accessible to the launcher

static
  → belongs to the class rather than requiring an instance

void
  → returns no value

main
  → method name recognized as an application entry point

String[] args
  → command-line arguments
```

---

## `System.out.println`

```java
System.out.println("Hello, Java!");
```

This prints text followed by a line terminator.

Conceptually:

```text
System
  └── out
       └── println(...)
```

`System.out` is a standard output stream.

---

# 7. Compilation and Execution

Suppose we have:

```text
HelloWorld.java
```

Compile:

```bash
javac HelloWorld.java
```

This produces a class file:

```text
HelloWorld.class
```

Run:

```bash
java HelloWorld
```

Notice:

```bash
java HelloWorld
```

not:

```bash
java HelloWorld.class
```

The launcher identifies the class to launch rather than treating the argument as a source filename.

---

## Complete flow

```text
HelloWorld.java
      │
      │ javac
      ▼
HelloWorld.class
      │
      │ java
      ▼
JVM
      │
      ▼
Program execution
```

Oracle's `javac` documentation specifies that Java source files are compiled into class files intended to run on the JVM.

---

## Compiling for a specific Java release

Modern `javac` provides `--release`.

Example:

```bash
javac --release 17 HelloWorld.java
```

This is useful when developing for a particular Java SE release rather than simply compiling against the currently installed JDK.

`--release` controls the source language rules and API/platform target for the selected release.

---

# 8. Variables

A variable is a named storage location associated with a type.

Example:

```java
int age = 27;
```

This can be understood as:

```text
int    age    =    27;
│       │          │
type    name       value
```

---

## 8.1 Declaration

```java
int age;
```

A variable is declared with a type and name.

---

## 8.2 Initialization

```java
age = 27;
```

A value is assigned to the variable.

---

## 8.3 Declaration + initialization

```java
int age = 27;
```

---

## 8.4 Local variables

Example:

```java
public static void main(String[] args) {
    int age = 27;
}
```

`age` is a local variable.

Local variables do not receive the same automatic default initialization as fields.

For example:

```java
public static void main(String[] args) {
    int age;
    System.out.println(age); // Compilation error
}
```

The local variable must be definitely assigned before use.

---

# 9. Types of Variables

At the introductory level, Java variables can be discussed as:

### Local variables

Declared inside methods, constructors or blocks.

### Instance variables

Fields belonging to an object.

### Static variables

Fields associated with the class rather than an individual object.

Example:

```java
class Counter {
    int instanceCount;
    static int totalCount;
}
```

We will explore instance vs static state in Part 2.

---

# 10. Data Types

Java types can broadly be divided into:

```text
Java Types
├── Primitive types
└── Reference types
```

---

# 11. Primitive Data Types

Java has eight primitive types:

| Type | Typical size | Example |
|---|---:|---|
| `byte` | 8 bits | `100` |
| `short` | 16 bits | `1000` |
| `int` | 32 bits | `100000` |
| `long` | 64 bits | `100000L` |
| `float` | 32-bit floating point | `10.5f` |
| `double` | 64-bit floating point | `10.5` |
| `char` | 16-bit UTF-16 code unit | `'A'` |
| `boolean` | language-defined representation | `true` |

Important:

The JVM specification and Java language specification distinguish Java's primitive types from reference types. Do not assume that every implementation must represent a `boolean` using a particular physical number of bits.

---

## 11.1 Integral types

```text
byte
short
int
long
```

Example:

```java
int population = 1_500_000;
long distance = 9_000_000_000L;
```

Underscores can improve readability in numeric literals:

```java
int amount = 1_000_000;
```

---

## 11.2 Floating-point types

```java
float temperature = 36.5f;
double price = 999.99;
```

A decimal literal such as:

```java
999.99
```

is a `double` by default.

For `float`, use:

```java
999.99f
```

### Important

Floating-point numbers should not normally be used for exact monetary calculations.

For financial calculations, `BigDecimal` is often more appropriate.

---

# 12. `char`

`char` represents a UTF-16 code unit.

Example:

```java
char grade = 'A';
```

Single quotes are used:

```java
'A'
```

Double quotes create a `String`:

```java
"A"
```

These are different types.

```java
char letter = 'A';
String word = "A";
```

A `char` is not necessarily a complete Unicode code point because some Unicode characters require a surrogate pair.

This becomes important when dealing deeply with Unicode and text processing.

---

# 13. `boolean`

A boolean has two values:

```java
true
false
```

Example:

```java
boolean loggedIn = true;
```

Unlike some languages, Java does not treat arbitrary integers as booleans.

This is invalid:

```java
int value = 1;

// Not valid Java:
// if (value) { }
```

Use an actual boolean expression:

```java
if (value == 1) {
    // ...
}
```

---

# 14. Reference Types

Reference types include types such as:

```java
String
int[] 
Customer
ArrayList<String>
```

Example:

```java
String name = "Satyam";
```

The variable does not contain the `String` object in the same conceptual way that an `int` variable directly represents a primitive value.

A useful beginner model is:

```text
name
 │
 ▼
String object
" Satyam "
```

Be careful: references, objects, stack/heap implementation details, and memory layout become more nuanced when we study the JVM.

---

# 15. `null`

A reference variable can contain `null`.

```java
String name = null;
```

`null` means the reference does not currently refer to an object.

This is different from:

```java
String name = "";
```

because an empty string is still a real `String` object.

---

# 16. Operators

Java operators can be grouped into several categories.

## Arithmetic

```text
+
-
*
/
%
```

Example:

```java
int total = 10 + 5;
int remainder = 10 % 3;
```

---

## Relational

```text
==
!=
>
<
>=
<=
```

Example:

```java
int age = 27;

boolean adult = age >= 18;
```

---

## Logical

```text
&&
||
!
```

Example:

```java
boolean hasAccount = true;
boolean active = true;

if (hasAccount && active) {
    // ...
}
```

### Short-circuit evaluation

For:

```java
a && b
```

if `a` is `false`, Java does not need to evaluate `b`.

For:

```java
a || b
```

if `a` is `true`, Java does not need to evaluate `b`.

This behavior is important when the second expression has side effects or could cause an error.

---

# 17. Assignment Operators

Basic assignment:

```java
int count = 10;
```

Compound assignment:

```java
count += 5;
count -= 2;
count *= 3;
count /= 2;
```

For example:

```java
count += 5;
```

is conceptually similar to:

```java
count = count + 5;
```

but compound assignment has specific Java conversion rules, so they are not universally interchangeable in every expression.

---

# 18. Increment and Decrement

```java
count++;
count--;
```

Prefix:

```java
++count;
--count;
```

Postfix:

```java
count++;
count--;
```

The distinction matters when the expression's value is used.

Example:

```java
int x = 5;
int y = x++;
```

Here, the old value is used for the assignment before `x` is incremented.

For readability, avoid unnecessarily complicated expressions containing multiple increments/decrements.

---

# 19. Ternary Operator

Syntax:

```java
condition ? valueIfTrue : valueIfFalse
```

Example:

```java
int age = 27;

String status = age >= 18 ? "Adult" : "Minor";
```

Think of it as a compact conditional expression.

Do not use deeply nested ternary expressions when they make the code harder to understand.

---

# 20. Operator Precedence

Java defines precedence rules that determine how expressions are grouped.

For example:

```java
int result = 2 + 3 * 4;
```

Multiplication is evaluated before addition, so the expression is grouped conceptually as:

```java
2 + (3 * 4)
```

When clarity matters, use parentheses:

```java
int result = 2 + (3 * 4);
```

Parentheses are often preferable to relying on readers remembering precedence rules.

---

# 21. Type Conversion

Type conversion means converting a value from one type to another.

Two major concepts are:

```text
Widening conversion
Narrowing conversion
```

---

# 22. Widening Primitive Conversion

A smaller-range numeric type can often be converted to a larger-range type automatically.

Example:

```java
int number = 100;
long largerNumber = number;
```

Another example:

```java
int number = 100;
double value = number;
```

No explicit cast is required.

Conceptually:

```text
int
 ↓
long
 ↓
float/double
```

The exact conversion rules are defined by the Java Language Specification.

---

# 23. Narrowing Conversion

Converting to a type that cannot represent the entire source range generally requires an explicit cast.

Example:

```java
double price = 99.99;
int wholeNumber = (int) price;
```

The fractional part is discarded:

```text
99.99
 ↓ cast to int
99
```

A cast does not perform rounding.

---

# 24. Overflow

Consider:

```java
int number = 2_147_483_647;
number++;
```

The result does not become a larger `long` automatically.

The `int` arithmetic wraps according to Java's integer arithmetic rules.

This is an important interview and debugging concept.

If a calculation may exceed the range of `int`, choose an appropriate type or use APIs designed to detect overflow where required.

---

# 25. `String` Conversion

Primitive values can be converted to text.

Example:

```java
int age = 27;

String text = String.valueOf(age);
```

String concatenation can also convert values to text:

```java
String message = "Age: " + age;
```

---

# 26. Control Flow

Control flow determines which statements execute and in what order.

Major categories:

```text
Control Flow
├── Sequential execution
├── Selection
│   ├── if
│   ├── if-else
│   └── switch
└── Iteration
    ├── for
    ├── while
    └── do-while
```

---

# 27. `if`

Syntax:

```java
if (condition) {
    // statements
}
```

Example:

```java
int age = 27;

if (age >= 18) {
    System.out.println("Adult");
}
```

The condition must evaluate to `boolean`.

---

# 28. `if-else`

```java
if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}
```

---

# 29. `else-if`

Use multiple conditions:

```java
int marks = 82;

if (marks >= 90) {
    System.out.println("A");
} else if (marks >= 75) {
    System.out.println("B");
} else if (marks >= 60) {
    System.out.println("C");
} else {
    System.out.println("Needs improvement");
}
```

Conditions are evaluated from top to bottom.

Once a matching branch executes, the remaining branches are skipped.

---

# 30. `switch`

`switch` is useful when selecting among multiple cases.

Traditional statement:

```java
int day = 2;

switch (day) {
    case 1:
        System.out.println("Monday");
        break;

    case 2:
        System.out.println("Tuesday");
        break;

    default:
        System.out.println("Unknown day");
}
```

---

# 31. Modern `switch`

Modern Java supports switch expressions and arrow labels.

Example:

```java
int day = 2;

String name = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    default -> "Unknown";
};
```

This is an important modernization compared with older Java notes.

The arrow form avoids the accidental fall-through associated with traditional case statements.

---

# 32. `yield`

A multi-statement switch expression can use `yield`.

```java
int day = 2;

String name = switch (day) {
    case 1 -> "Monday";

    case 2 -> {
        String result = "Tuesday";
        yield result;
    }

    default -> "Unknown";
};
```

`yield` supplies the value of a switch expression from a block.

---

# 33. `for` Loop

Basic syntax:

```java
for (initialization; condition; update) {
    // body
}
```

Example:

```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
```

Execution conceptually:

```text
initialize
    ↓
check condition
    ↓
execute body
    ↓
update
    ↓
check condition again
```

---

# 34. Enhanced `for` Loop

For arrays and compatible iterable structures:

```java
int[] numbers = {10, 20, 30};

for (int number : numbers) {
    System.out.println(number);
}
```

This is commonly called the **enhanced for loop** or **for-each loop**.

It is useful when you need each element rather than an explicit index.

---

# 35. `while` Loop

```java
while (condition) {
    // body
}
```

Example:

```java
int count = 1;

while (count <= 5) {
    System.out.println(count);
    count++;
}
```

The condition is checked before each iteration.

---

# 36. `do-while`

```java
do {
    // body
} while (condition);
```

Example:

```java
int count = 1;

do {
    System.out.println(count);
    count++;
} while (count <= 5);
```

The body executes at least once because the condition is checked afterward.

---

# 37. `break`

`break` exits the nearest applicable loop or switch statement.

Example:

```java
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;
    }

    System.out.println(i);
}
```

Output:

```text
1
2
3
4
```

---

# 38. `continue`

`continue` skips the remaining body of the current loop iteration.

Example:

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue;
    }

    System.out.println(i);
}
```

Output:

```text
1
2
4
5
```

---

# 39. Labeled Statements

Java also supports labels.

Example:

```java
outer:
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        if (i == 2 && j == 2) {
            break outer;
        }
    }
}
```

A labeled `break` can exit the labeled statement.

Labeled control flow is valid Java, but should generally be used only when it improves clarity.

---

# 40. Common Beginner Mistakes

## Mistake 1 — Using `=` instead of `==`

```java
// Assignment
x = 10;

// Equality comparison
x == 10;
```

---

## Mistake 2 — Comparing strings with `==`

```java
String a = new String("Java");
String b = new String("Java");

// Usually wrong when comparing string contents:
a == b;
```

Use:

```java
a.equals(b);
```

This topic will be covered deeply in the Strings chapter.

---

## Mistake 3 — Integer division

```java
int result = 5 / 2;
```

The result is:

```text
2
```

not `2.5`.

To perform floating-point division:

```java
double result = 5.0 / 2;
```

---

## Mistake 4 — Forgetting `L`

For large `long` literals:

```java
long distance = 9_000_000_000L;
```

The `L` identifies the literal as a `long`.

---

## Mistake 5 — Thinking casting rounds numbers

```java
int value = (int) 9.99;
```

The result is:

```text
9
```

Casting does not mean mathematical rounding.

---

# 41. Important Relationships

Part 1 establishes the foundations required for the later chapters.

```text
Java source
     │
     ▼
Compilation
     │
     ▼
Bytecode
     │
     ▼
JVM
     │
     ▼
Execution
```

Variables depend on types:

```text
Variable
   │
   └── Type
       ├── Primitive
       └── Reference
```

Control flow operates on expressions:

```text
Expression
   │
   ▼
boolean result
   │
   ▼
if / while / switch
```

These foundations lead naturally into object-oriented programming:

```text
Type
 │
 ├── Primitive type
 │
 └── Reference type
       │
       ▼
     Class
       │
       ▼
     Object
```

---

# 42. Interview & Revision Points

### Fundamental questions

1. What is Java?
2. What is bytecode?
3. What is the JVM?
4. What is the difference between JDK and JVM?
5. What is the difference between JDK and the historical JRE concept?
6. Why is Java called platform independent?
7. What does `javac` do?
8. What does the `java` launcher do?
9. What is the difference between `.java` and `.class`?
10. What happens when Java source code is compiled?

### Variables and types

11. What are Java's primitive types?
12. What is the difference between primitive and reference types?
13. What is `null`?
14. Why can't an uninitialized local variable be read?
15. What is widening conversion?
16. What is narrowing conversion?
17. What happens when an `int` overflows?
18. What is the difference between `char` and `String`?
19. Why is `5 / 2` equal to `2` when both operands are integers?

### Control flow

20. What is the difference between `while` and `do-while`?
21. What is the difference between `break` and `continue`?
22. What is switch fall-through?
23. How do modern switch expressions differ from traditional switch statements?
24. What is `yield` used for?

---

# 43. Quick Revision

## Java fundamentals

- Java is a strongly typed, class-based programming language.
- Java source code is normally compiled into JVM class files.
- The JVM executes bytecode.
- The JDK provides development tools and runtime components.
- `javac` compiles Java source.
- `java` launches Java applications.
- Modern JDK distributions should not be explained simply as "JDK contains a separate JRE folder."

## Variables

- Variables have types.
- Local variables must be definitely assigned before use.
- Fields have different initialization rules.
- Reference variables can contain `null`.

## Primitive types

```text
byte
short
int
long
float
double
char
boolean
```

## Operators

```text
Arithmetic:     + - * / %
Comparison:     == != > < >= <=
Logical:        && || !
Assignment:     = += -= *= /= ...
Conditional:    ?:
```

## Type conversion

```text
Widening  → generally implicit
Narrowing → generally explicit cast
```

## Control flow

```text
if
if-else
switch
for
enhanced for
while
do-while
break
continue
```

---

# 44. Practice Questions

## Conceptual

1. Explain the difference between Java source code, bytecode and JVM execution.
2. Explain JDK vs JVM using a practical example.
3. Explain primitive vs reference types.
4. Explain widening vs narrowing conversion.
5. Explain why local variables must be initialized before use.
6. Explain the difference between `while` and `do-while`.
7. Explain traditional switch vs modern switch expressions.

## Predict the output

### Question 1

```java
int x = 10;
int y = 3;

System.out.println(x / y);
System.out.println(x % y);
```

### Question 2

```java
int x = 5;
int y = x++;

System.out.println(x);
System.out.println(y);
```

### Question 3

```java
int number = 10;

if (number > 5 && number < 20) {
    System.out.println("Inside");
} else {
    System.out.println("Outside");
}
```

### Question 4

```java
int day = 2;

String result = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    default -> "Other";
};

System.out.println(result);
```

## Coding exercises

1. Write a program that determines whether a number is positive, negative or zero.
2. Write a program that prints numbers from 1 to 100.
3. Write a program that calculates the sum of numbers from 1 to `n`.
4. Write a program that checks whether a number is even or odd.
5. Write a program that converts a temperature from Celsius to Fahrenheit.
6. Write a program that finds the largest of three numbers.
7. Rewrite an `if-else` decision using a suitable switch expression.

**Do not look up the answers immediately. Try each exercise first.**

---

# 45. Key Takeaway

The most important mental model from Part 1 is:

```text
Java source code
       │
       ▼
     javac
       │
       ▼
 JVM class files / bytecode
       │
       ▼
      JVM
       │
       ▼
   Execution
```

And at the language level:

```text
Types
  ↓
Variables
  ↓
Expressions / Operators
  ↓
Decisions + Loops
  ↓
Classes + Objects
  ↓
Larger Java programs
```

Once these foundations are comfortable, Part 2 can build naturally on them with **classes, objects, constructors, inheritance, polymorphism, abstraction and interfaces**.

---

# References

- Oracle Java SE Specifications — Java SE 27.
- Oracle JDK 25 Documentation and guides.
- Oracle Java Language and VM Specifications.
- Oracle `javac` Tool Specification.
- Oracle JDK Installation Guide.
- Oracle Java SE API Documentation.