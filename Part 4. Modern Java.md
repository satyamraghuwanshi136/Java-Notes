# COMPLETE JAVA NOTES

# Part 4 — Modern Java

**Baseline:** Java SE 27  
**Status labels used in this chapter:**  
**Permanent** = standardized Java feature  
**Preview** = available only as a preview feature in the specified release and may change

---

# Table of Contents

1. What Changed in Modern Java?
2. Lambda Expressions
3. Functional Interfaces
4. Method References
5. Stream API
6. `Optional`
7. Modern Date and Time API
8. Records
9. Sealed Classes
10. Pattern Matching
11. Modern `switch`
12. `var` — Local Variable Type Inference
13. Text Blocks
14. Unnamed Variables and Patterns
15. Other Modern Java Features Worth Knowing
16. Putting Modern Features Together
17. Common Mistakes
18. Interview & Revision Points
19. Quick Revision
20. Practice Questions
21. References

---

# 1. What Changed in Modern Java?

Java 8 introduced several changes that fundamentally changed everyday Java programming:

```text
Java 8
├── Lambda expressions
├── Functional interfaces
├── Method references
├── Stream API
├── Optional
└── java.time
```

Later releases added language features that make Java more expressive:

```text
Java 14
└── Switch expressions

Java 16
├── Records
└── Pattern matching for instanceof

Java 17
└── Sealed classes

Java 21
├── Pattern matching for switch
└── Record patterns

Java 22
└── Unnamed variables and patterns

Java 25
├── Module import declarations
├── Compact source files / instance main methods
└── Flexible constructor bodies

Java 27
└── Primitive types in patterns, instanceof and switch — preview
```

Oracle's language-change summary provides the release-by-release history.

The important lesson is:

> Do not treat all "modern Java" features as if they were introduced in the same version.

---

# 2. Lambda Expressions

## What is a lambda?

A lambda expression is a concise way to represent behavior that can be passed to code expecting a **functional interface**.

Basic syntax:

```java
(parameters) -> expression
```

or:

```java
(parameters) -> {
    // statements
}
```

Example:

```java
(int a, int b) -> a + b
```

A lambda is not itself a class declaration. It is an expression whose target type is normally a functional interface.

---

# 3. Why Lambdas?

Before lambdas, passing behavior often required an anonymous class.

Traditional style:

```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running");
    }
};
```

Lambda:

```java
Runnable task =
        () -> System.out.println("Running");
```

The lambda removes boilerplate while preserving the important behavior.

Lambda expressions were introduced in Java 8.

---

# 4. Lambda Syntax

## No parameters

```java
() -> System.out.println("Hello")
```

## One parameter

```java
name -> System.out.println(name)
```

Parentheses may be omitted for a single implicitly typed parameter.

## Multiple parameters

```java
(a, b) -> a + b
```

## Explicit parameter types

```java
(int a, int b) -> a + b
```

If explicit types are used, the lambda parameter list must obey Java's syntax rules consistently.

---

# 5. Expression Lambda vs Block Lambda

Expression body:

```java
x -> x * 2
```

Block body:

```java
x -> {
    int result = x * 2;
    return result;
}
```

A block lambda returning a value must use `return` when required by the target functional interface.

---

# 6. Lambda Target Type

Consider:

```java
var operation = (a, b) -> a + b;
```

This does **not** work because a lambda needs a target functional-interface type.

Instead:

```java
BinaryOperator<Integer> operation =
        (a, b) -> a + b;
```

Now the compiler knows:

```text
argument 1 → Integer
argument 2 → Integer
result     → Integer
```

This is called **target typing**.

---

# 7. Lambda Scope

Lambdas are **lexically scoped**.

Example:

```java
class Example {

    private int value = 10;

    void run() {

        Runnable task = () -> {
            System.out.println(this.value);
        };

        task.run();
    }
}
```

Inside the lambda, `this` refers to the enclosing instance.

Oracle's documentation also notes that lambdas can capture local variables and parameters only when they are `final` or **effectively final**.

---

# 8. Effectively Final

This is valid:

```java
int multiplier = 10;

Function<Integer, Integer> multiply =
        value -> value * multiplier;
```

`multiplier` is effectively final because it is not reassigned.

This is invalid:

```java
int multiplier = 10;

multiplier = 20;

Function<Integer, Integer> multiply =
        value -> value * multiplier;
```

The captured local variable is no longer effectively final.

---

# 9. Functional Interfaces

A functional interface has exactly **one abstract method**.

Example:

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

Now:

```java
Calculator add = (a, b) -> a + b;
```

The lambda implements the functional interface's abstract method.

A functional interface may still contain:

- default methods
- static methods
- methods inherited from `Object`

The key restriction concerns the number of **abstract methods**.

---

# 10. `@FunctionalInterface`

Use:

```java
@FunctionalInterface
```

Example:

```java
@FunctionalInterface
interface Formatter {
    String format(String value);
}
```

The annotation tells the compiler that you intend the interface to satisfy functional-interface requirements.

If a second incompatible abstract method is added, compilation fails.

---

# 11. Common Functional Interfaces

The `java.util.function` package provides many standard functional interfaces.

| Interface | Input | Output | Typical use |
|---|---|---|---|
| `Predicate<T>` | `T` | `boolean` | Test |
| `Function<T,R>` | `T` | `R` | Transform |
| `Consumer<T>` | `T` | none | Consume |
| `Supplier<T>` | none | `T` | Produce |
| `UnaryOperator<T>` | `T` | `T` | Transform same type |
| `BinaryOperator<T>` | `T,T` | `T` | Combine same type |
| `BiFunction<T,U,R>` | `T,U` | `R` | Two-input transformation |
| `BiPredicate<T,U>` | `T,U` | `boolean` | Two-input test |
| `BiConsumer<T,U>` | `T,U` | none | Two-input action |

---

# 12. Predicate

```java
Predicate<Integer> even =
        number -> number % 2 == 0;
```

Use:

```java
System.out.println(even.test(10));
```

`test()` returns a boolean.

---

# 13. Function

```java
Function<String, Integer> length =
        String::length;
```

Use:

```java
int size = length.apply("Java");
```

Think:

```text
T → R
```

---

# 14. Consumer

```java
Consumer<String> printer =
        value -> System.out.println(value);
```

Use:

```java
printer.accept("Java");
```

Think:

```text
T → nothing
```

---

# 15. Supplier

```java
Supplier<String> supplier =
        () -> "Java";
```

Use:

```java
String value = supplier.get();
```

Think:

```text
nothing → T
```

---

# 16. Method References

Method references provide a concise form for certain lambdas that delegate to an existing method.

Example:

```java
Function<String, Integer> length =
        String::length;
```

This is conceptually similar to:

```java
Function<String, Integer> length =
        text -> text.length();
```

Oracle describes four common kinds of method references.

---

# 17. Four Main Forms of Method Reference

## 1. Static method

```java
ContainingClass::staticMethod
```

Example:

```java
Function<String, Integer> parser =
        Integer::parseInt;
```

---

## 2. Instance method of a particular object

```java
object::instanceMethod
```

Example:

```java
String prefix = "Hello ";

Function<String, String> combine =
        prefix::concat;
```

---

## 3. Instance method of an arbitrary object of a type

```java
Type::instanceMethod
```

Example:

```java
Comparator<String> comparator =
        String::compareToIgnoreCase;
```

Conceptually:

```java
(a, b) -> a.compareToIgnoreCase(b)
```

---

## 4. Constructor reference

```java
ClassName::new
```

Example:

```java
Supplier<ArrayList<String>> factory =
        ArrayList::new;
```

Conceptually:

```java
() -> new ArrayList<>()
```

---

# 18. When Should You Use a Method Reference?

Use a method reference when it makes the code **clearer**.

Good:

```java
names.forEach(System.out::println);
```

Less useful:

```java
names.forEach(name -> System.out.println(name));
```

Both are valid; the first is simply more concise.

Do not force method references when they make the operation harder to understand.

---

# 19. Stream API

The Stream API provides a functional-style way to process sequences of elements.

Oracle describes streams as supporting functional-style operations such as map-reduce transformations over collections.

A stream is **not a collection**.

Think:

```text
Collection
   │
   │ stream()
   ▼
 Stream pipeline
   │
   ├── intermediate operations
   └── terminal operation
```

---

# 20. Creating a Stream

From a collection:

```java
List<String> names =
        List.of("A", "B", "C");

Stream<String> stream =
        names.stream();
```

From values:

```java
Stream<String> stream =
        Stream.of("A", "B", "C");
```

From an array:

```java
int[] numbers = {1, 2, 3};

IntStream stream =
        Arrays.stream(numbers);
```

---

# 21. Stream Pipeline

A typical pipeline:

```java
List<String> names =
        List.of("Satyam", "Sonam", "Amit", "Riya");

List<String> result =
        names.stream()
             .filter(name -> name.length() > 4)
             .map(String::toUpperCase)
             .toList();
```

Conceptually:

```text
source
  ↓
filter
  ↓
map
  ↓
terminal operation
```

---

# 22. Intermediate Operations

Common intermediate operations include:

```text
filter()
map()
flatMap()
distinct()
sorted()
peek()
limit()
skip()
takeWhile()
dropWhile()
```

They generally return another stream and are lazy.

---

# 23. `filter`

Keeps elements satisfying a predicate.

```java
List<Integer> evenNumbers =
        numbers.stream()
               .filter(number -> number % 2 == 0)
               .toList();
```

Conceptually:

```text
1 2 3 4 5 6
  ↓
2 4 6
```

---

# 24. `map`

Transforms every element.

```java
List<String> upperCaseNames =
        names.stream()
             .map(String::toUpperCase)
             .toList();
```

Think:

```text
one element
    ↓
one transformed element
```

---

# 25. `flatMap`

`flatMap` is useful when one input element can produce multiple output elements.

Example:

```java
List<List<String>> groups =
        List.of(
            List.of("A", "B"),
            List.of("C", "D")
        );

List<String> names =
        groups.stream()
              .flatMap(List::stream)
              .toList();
```

Conceptually:

```text
[A, B] [C, D]
     ↓ flatMap
[A, B, C, D]
```

---

# 26. `distinct`

Removes duplicates according to equality semantics.

```java
List<Integer> result =
        numbers.stream()
               .distinct()
               .toList();
```

For custom objects, correct `equals()`/`hashCode()` behavior matters.

---

# 27. `sorted`

```java
List<Integer> sorted =
        numbers.stream()
               .sorted()
               .toList();
```

Custom ordering:

```java
List<String> sorted =
        names.stream()
             .sorted(String.CASE_INSENSITIVE_ORDER)
             .toList();
```

---

# 28. `limit` and `skip`

```java
List<Integer> firstThree =
        numbers.stream()
               .limit(3)
               .toList();
```

Skip:

```java
List<Integer> afterThree =
        numbers.stream()
               .skip(3)
               .toList();
```

These are useful for pagination-like operations, sampling, and bounded processing.

---

# 29. Terminal Operations

Terminal operations produce a result or side effect and consume the stream.

Common examples:

```text
forEach()
toList()
collect()
count()
reduce()
min()
max()
findFirst()
findAny()
anyMatch()
allMatch()
noneMatch()
```

After a stream has been consumed, it cannot simply be reused as the same stream instance.

---

# 30. `forEach`

```java
names.stream()
     .forEach(System.out::println);
```

For ordinary collection traversal, a normal enhanced `for` loop can sometimes be simpler.

Streams are most valuable when expressing a processing pipeline.

---

# 31. `toList()` vs Collectors

Modern Java supports:

```java
List<String> result =
        names.stream()
             .filter(name -> name.length() > 4)
             .toList();
```

`toList()` returns an unmodifiable list according to its API contract.

For more specific collection strategies, use `Collectors`:

```java
List<String> result =
        names.stream()
             .filter(name -> name.length() > 4)
             .collect(Collectors.toList());
```

Do not automatically assume these have identical mutability characteristics.

---

# 32. `collect`

`collect()` performs a mutable reduction using a `Collector`.

Example:

```java
Map<String, Integer> lengths =
        names.stream()
             .collect(
                 Collectors.toMap(
                     name -> name,
                     String::length
                 )
             );
```

---

# 33. Grouping

One of the most useful collectors:

```java
Map<Integer, List<String>> byLength =
        names.stream()
             .collect(
                 Collectors.groupingBy(String::length)
             );
```

Conceptually:

```text
names
  ↓
group by key
  ↓
Map<Key, List<Value>>
```

---

# 34. Counting

```java
Map<Integer, Long> counts =
        names.stream()
             .collect(
                 Collectors.groupingBy(
                     String::length,
                     Collectors.counting()
                 )
             );
```

---

# 35. Partitioning

`partitioningBy` separates elements according to a boolean predicate.

```java
Map<Boolean, List<Integer>> partitioned =
        numbers.stream()
               .collect(
                   Collectors.partitioningBy(
                       number -> number % 2 == 0
                   )
               );
```

Result:

```text
true  → even numbers
false → odd numbers
```

---

# 36. `reduce`

Reduction combines multiple elements into one result.

Example:

```java
int sum =
        numbers.stream()
               .reduce(0, Integer::sum);
```

Conceptually:

```text
1 + 2 + 3 + 4
       ↓
      10
```

Reduction requires careful reasoning when used with parallel streams. An operation should obey the required identity/associativity properties.

---

# 37. Primitive Streams

Java provides:

```text
IntStream
LongStream
DoubleStream
```

Example:

```java
int sum =
        numbers.stream()
               .mapToInt(Integer::intValue)
               .sum();
```

Primitive streams can avoid some boxing overhead.

---

# 38. Lazy Evaluation

Intermediate operations are generally lazy.

This:

```java
Stream<String> stream =
        names.stream()
             .filter(name -> {
                 System.out.println(name);
                 return name.length() > 4;
             });
```

does not necessarily execute the filter merely because the pipeline was created.

Add a terminal operation:

```java
long count =
        stream.count();
```

and the pipeline is evaluated.

Think:

```text
build pipeline
      ↓
nothing necessarily executes yet
      ↓
terminal operation
      ↓
pipeline evaluation
```

---

# 39. Short-Circuiting

Some operations can stop before processing every element.

Examples:

```text
findFirst()
findAny()
anyMatch()
allMatch()
noneMatch()
limit()
```

Example:

```java
boolean containsLongName =
        names.stream()
             .anyMatch(name -> name.length() > 10);
```

Once a qualifying element is found, processing may stop.

---

# 40. Parallel Streams

You can create a parallel stream:

```java
numbers.parallelStream()
```

or:

```java
numbers.stream().parallel()
```

But parallelism is **not automatically faster**.

Performance depends on:

- workload size
- operation cost
- splitting characteristics
- CPU resources
- memory behavior
- ordering requirements
- contention
- pipeline structure

Use parallel streams deliberately and measure.

---

# 41. Stream Side Effects

Avoid unnecessary mutable shared state:

```java
List<String> result = new ArrayList<>();

names.stream()
     .filter(...)
     .forEach(result::add);
```

Prefer:

```java
List<String> result =
        names.stream()
             .filter(...)
             .toList();
```

The second form expresses the transformation directly and is easier to reason about.

---

# 42. Streams Are Not Collections

A collection:

```text
stores data
```

A stream:

```text
describes processing over data
```

Useful mental model:

```text
Collection → "What data do I have?"
Stream     → "What processing should I perform?"
```

---

# 43. `Optional`

`Optional<T>` is a container representing either:

```text
a non-null value
```

or:

```text
no value
```

Oracle describes `Optional` as primarily intended for method return types when a missing result needs explicit representation.

Example:

```java
Optional<String> name =
        Optional.of("Satyam");
```

Empty:

```java
Optional<String> empty =
        Optional.empty();
```

Nullable source:

```java
Optional<String> value =
        Optional.ofNullable(possiblyNull);
```

---

# 44. Why `Optional`?

Instead of:

```java
String findName() {
    return null;
}
```

you can model:

```java
Optional<String> findName() {
    // ...
}
```

The caller is encouraged to explicitly consider the absence case.

---

# 45. `isPresent`

```java
if (name.isPresent()) {
    System.out.println(name.get());
}
```

This works, but often misses the main benefit of a compositional `Optional` API.

Prefer methods such as:

```text
map()
flatMap()
filter()
orElse()
orElseGet()
orElseThrow()
ifPresent()
ifPresentOrElse()
```

when appropriate.

---

# 46. `orElse`

```java
String result =
        name.orElse("Unknown");
```

If the optional is empty, `"Unknown"` is returned.

---

# 47. `orElseGet`

```java
String result =
        name.orElseGet(this::loadDefaultName);
```

The supplier is used only when the value is absent.

This distinction matters:

```text
orElse(...)
    ↓
fallback expression is evaluated before the call

orElseGet(...)
    ↓
fallback supplier is evaluated only when needed
```

This can matter when the fallback is expensive or has side effects.

---

# 48. `orElseThrow`

```java
String result =
        name.orElseThrow();
```

Or:

```java
String result =
        name.orElseThrow(
            () -> new IllegalStateException("Name missing")
        );
```

---

# 49. `map`

```java
Optional<Integer> length =
        name.map(String::length);
```

Conceptually:

```text
Optional<String>
       ↓ map
Optional<Integer>
```

If the original optional is empty, the result remains empty.

---

# 50. `flatMap`

Use `flatMap` when the mapping operation already returns an `Optional`.

Example:

```java
Optional<User> user = ...;

Optional<Address> address =
        user.flatMap(User::address);
```

Without `flatMap`, you could accidentally produce:

```text
Optional<Optional<Address>>
```

---

# 51. Optional Should Not Be Everywhere

A common mistake is:

```java
Optional<String> field;
```

for every class field.

`Optional` is primarily designed to communicate optional results, especially as return values. It is not a universal replacement for `null` in every location. Oracle's API documentation specifically gives method return types as its primary use case.

---

# 52. Optional and `get()`

Avoid:

```java
optional.get();
```

unless you have a proven reason that the value is present.

Prefer:

```java
orElse(...)
orElseGet(...)
orElseThrow(...)
ifPresent(...)
map(...)
flatMap(...)
```

---

# 53. Modern Date/Time API

Java 8 introduced `java.time`.

Important types:

```text
LocalDate
LocalTime
LocalDateTime
ZonedDateTime
OffsetDateTime
Instant
Duration
Period
Year
YearMonth
MonthDay
ZoneId
DateTimeFormatter
```

The `java.time` API is the modern standard API for date/time work. Oracle's current documentation describes it as the main API for dates, times, instants and durations.

---

# 54. `LocalDate`

Use when you need a date without a time or time zone.

```java
LocalDate today =
        LocalDate.now();
```

Create:

```java
LocalDate date =
        LocalDate.of(2026, 9, 18);
```

---

# 55. `LocalTime`

Use for a time without a date or time zone.

```java
LocalTime time =
        LocalTime.of(9, 30);
```

---

# 56. `LocalDateTime`

Combines date and time without a zone.

```java
LocalDateTime dateTime =
        LocalDateTime.of(
            2026, 9, 18,
            9, 30
        );
```

Important:

> `LocalDateTime` does not represent an absolute instant.

It has no time-zone or offset.

---

# 57. `Instant`

`Instant` represents a point on the UTC timeline.

Example:

```java
Instant now =
        Instant.now();
```

Use it when you need an absolute timestamp.

Conceptually:

```text
LocalDateTime
    ↓
human/local date and time

Instant
    ↓
point on global timeline
```

---

# 58. `ZonedDateTime`

Use when the time zone matters.

```java
ZonedDateTime meeting =
        ZonedDateTime.now(
            ZoneId.of("Asia/Kolkata")
        );
```

Another zone:

```java
ZonedDateTime tokyo =
        meeting.withZoneSameInstant(
            ZoneId.of("Asia/Tokyo")
        );
```

This is important when converting an event between time zones.

---

# 59. `Period` vs `Duration`

### `Period`

Date-based amount:

```java
Period period =
        Period.ofDays(10);
```

### `Duration`

Time-based amount:

```java
Duration duration =
        Duration.ofHours(10);
```

Mental model:

```text
Period   → dates
Duration → time-based elapsed amount
```

---

# 60. Date/Time Immutability

Modern date/time classes are generally immutable.

Example:

```java
LocalDate date =
        LocalDate.of(2026, 9, 18);

LocalDate tomorrow =
        date.plusDays(1);
```

`date` itself does not change.

---

# 61. Parsing and Formatting

Use `DateTimeFormatter`.

```java
DateTimeFormatter formatter =
        DateTimeFormatter.ofPattern("dd-MM-yyyy");

LocalDate date =
        LocalDate.parse("18-09-2026", formatter);
```

Format:

```java
String formatted =
        date.format(formatter);
```

The formatter API is designed for printing and parsing date-time objects.

---

# 62. Avoid Old Date/Time APIs for New Code

Legacy APIs include:

```text
java.util.Date
java.util.Calendar
SimpleDateFormat
```

They remain relevant when working with older code, but `java.time` should generally be the starting point for new Java applications.

---

# 63. Records

A record is a concise way to declare a restricted form of class intended for modeling simple aggregates of values.

Example:

```java
public record Person(
    String name,
    int age
) {
}
```

Record classes became a permanent Java feature in Java 16.

---

# 64. What a Record Provides

The declaration:

```java
record Person(String name, int age) {}
```

automatically provides, subject to the record rules:

- private final component fields
- accessor methods named after components
- canonical constructor
- `equals()`
- `hashCode()`
- `toString()`

Accessors are:

```java
person.name()
person.age()
```

not:

```java
person.getName()
person.getAge()
```

---

# 65. Record Example

```java
record Product(
    String name,
    double price
) {}
```

Usage:

```java
Product product =
        new Product("Laptop", 75000);

System.out.println(product.name());
System.out.println(product.price());
```

---

# 66. Records Are Not Just "Immutable Classes"

This statement is too simplistic.

Records have final component fields and do not expose ordinary setters, but **deep immutability is not guaranteed**.

Example:

```java
record Team(List<String> members) {}
```

The record component reference is final, but the referenced list may still be mutable.

Therefore:

```text
record
 ≠ automatically deeply immutable object
```

---

# 67. Record Methods

A record can have methods:

```java
record Rectangle(double width, double height) {

    double area() {
        return width * height;
    }
}
```

It can implement interfaces.

```java
record Employee(String id)
        implements Comparable<Employee> {

    @Override
    public int compareTo(Employee other) {
        return id.compareTo(other.id);
    }
}
```

---

# 68. Compact Constructor

Records support compact constructors useful for validation.

```java
record Person(String name, int age) {

    Person {
        if (age < 0) {
            throw new IllegalArgumentException(
                "Age cannot be negative"
            );
        }
    }
}
```

The compact constructor avoids explicitly repeating the component parameter list and assignment boilerplate.

---

# 69. Records Cannot Extend Arbitrary Classes

A record already has `java.lang.Record` as its superclass and cannot extend another class.

It can implement interfaces.

A record is implicitly final. Nested record classes are implicitly static.

---

# 70. Sealed Classes

A sealed class or interface restricts which types may directly extend or implement it.

Example:

```java
sealed interface Payment
        permits CardPayment, CashPayment {
}
```

Implementations:

```java
final class CardPayment
        implements Payment {
}

final class CashPayment
        implements Payment {
}
```

Sealed classes became permanent in Java 17.

---

# 71. `final`, `sealed`, `non-sealed`

A permitted subtype must explicitly define its extensibility.

```java
sealed class Shape
        permits Circle, Rectangle, Polygon {
}
```

Then a permitted child may be:

```java
final class Circle extends Shape {
}
```

or:

```java
sealed class Polygon extends Shape
        permits Triangle, Hexagon {
}
```

or:

```java
non-sealed class Rectangle extends Shape {
}
```

The three meanings are:

```text
final
  ↓
cannot be subclassed

sealed
  ↓
subclasses are explicitly controlled

non-sealed
  ↓
reopens inheritance below this point
```

Oracle's language specification explicitly describes these relationships.

---

# 72. Why Sealed Classes?

Sealed hierarchies are useful when the domain has a **known set of alternatives**.

Example:

```text
Payment
├── CardPayment
├── CashPayment
└── UpiPayment
```

This works particularly well with modern pattern matching and exhaustive `switch`.

---

# 73. Pattern Matching

Pattern matching combines:

```text
test
+
type extraction
```

Instead of:

```java
if (value instanceof String) {
    String text = (String) value;
    System.out.println(text.length());
}
```

Modern Java allows:

```java
if (value instanceof String text) {
    System.out.println(text.length());
}
```

Pattern matching for `instanceof` became permanent in Java 16.

---

# 74. Pattern Variable Scope

The pattern variable exists where the match is known to be successful.

Example:

```java
if (value instanceof String text
        && !text.isBlank()) {

    System.out.println(text);
}
```

This allows the compiler to reason about the scope of `text`.

---

# 75. Negated Pattern

Example:

```java
if (!(value instanceof String text)) {
    return;
}

System.out.println(text.length());
```

After the early return, `text` is known to be available.

This is often called **flow scoping**.

---

# 76. Pattern Matching with Records

Record patterns allow extraction from record components.

Example:

```java
record Point(int x, int y) {}
```

Then:

```java
if (object instanceof Point(int x, int y)) {
    System.out.println(x + y);
}
```

Record patterns became permanent in Java 21.

---

# 77. Nested Record Patterns

Patterns can be nested.

```java
record Point(int x, int y) {}
record Line(Point start, Point end) {}
```

You can conceptually match:

```java
if (object instanceof Line(
        Point(int x1, int y1),
        Point(int x2, int y2))) {

    System.out.println(
        x2 - x1
    );
}
```

This reduces manual accessor calls.

---

# 78. Modern `switch`

Older Java code:

```java
switch (day) {
    case 1:
        name = "Monday";
        break;
    case 2:
        name = "Tuesday";
        break;
    default:
        name = "Unknown";
}
```

Modern Java:

```java
String name = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    default -> "Unknown";
};
```

Switch expressions became permanent in Java 14.

---

# 79. Why Switch Expressions?

They can:

- return values
- eliminate many accidental fall-through cases
- make decision logic more concise
- encourage exhaustive thinking

Example:

```java
String category = switch (score) {
    case 90, 100 -> "Excellent";
    case 75, 80, 85 -> "Good";
    default -> "Other";
};
```

---

# 80. `yield`

A switch expression can use a block:

```java
String result = switch (day) {
    case 1 -> "Monday";

    case 2 -> {
        String value = "Tuesday";
        yield value;
    }

    default -> "Unknown";
};
```

`yield` provides the value of that switch-expression arm.

---

# 81. `switch` with Type Patterns

Modern switch can pattern-match reference types.

Example:

```java
static String describe(Object value) {
    return switch (value) {
        case Integer i -> "Integer: " + i;
        case String s -> "String: " + s;
        case null -> "null";
        default -> "Other";
    };
}
```

Pattern matching for switch became permanent in Java 21.

---

# 82. `case null`

Modern enhanced switch can explicitly match `null`:

```java
switch (value) {
    case null -> System.out.println("null");
    case String s -> System.out.println(s);
    default -> System.out.println("other");
}
```

This avoids necessarily receiving a `NullPointerException` just because the selector is null in an enhanced switch.

---

# 83. Exhaustive Switch

Switch expressions must be exhaustive.

With enums:

```java
enum Status {
    NEW, PAID, SHIPPED
}
```

This can be exhaustive:

```java
String message = switch (status) {
    case NEW -> "New";
    case PAID -> "Paid";
    case SHIPPED -> "Shipped";
};
```

For sealed hierarchies, the compiler can use the permitted type information to reason about exhaustiveness.

---

# 84. Sealed Classes + Pattern Matching + Switch

This combination is extremely powerful.

```java
sealed interface Shape
        permits Circle, Rectangle {
}

record Circle(double radius)
        implements Shape {
}

record Rectangle(double width, double height)
        implements Shape {
}
```

Then:

```java
static double area(Shape shape) {
    return switch (shape) {
        case Circle c ->
            Math.PI * c.radius() * c.radius();

        case Rectangle r ->
            r.width() * r.height();
    };
}
```

Conceptually:

```text
sealed hierarchy
       ↓
known alternatives
       ↓
pattern matching
       ↓
exhaustive switch
       ↓
safer domain modeling
```

This is one of the clearest examples of modern Java features working together.

---

# 85. Java 27 Primitive Patterns — Preview

Java SE 27 introduces a **preview** feature expanding pattern matching so primitive types can participate more broadly in patterns, `instanceof`, and `switch`.

Oracle currently lists:

```text
Primitive Types in Patterns,
instanceof, and switch
```

as a Java 27 preview feature.

This is important:

> **Do not treat this as finalized Java syntax yet.**

Preview syntax must be enabled appropriately and can change in a future release.

For stable production learning, focus primarily on the finalized pattern-matching features from Java 16 and Java 21.

---

# 86. `var`

Java 10 introduced local-variable type inference.

Example:

```java
var name = "Java";
```

The compiler infers:

```text
name → String
```

Another example:

```java
var numbers = new ArrayList<Integer>();
```

The variable still has a definite static type.

`var` does **not** turn Java into a dynamically typed language.

---

# 87. What `var` Can and Cannot Do

Valid:

```java
var name = "Satyam";
var count = 10;
```

Invalid:

```java
// var value;
```

The compiler needs an initializer to infer the type.

Also not valid as a method parameter or field declaration in ordinary Java syntax:

```java
// void process(var value) { }
// class Example { var field = ...; }
```

Use `var` when it improves readability, not simply to remove type information.

---

# 88. Text Blocks

Text blocks became permanent in Java 15.

Example:

```java
String json = """
        {
          "name": "Satyam",
          "language": "Java"
        }
        """;
```

They are useful for:

- JSON
- SQL
- HTML
- multiline text

They often make embedded text considerably more readable.

---

# 89. Unnamed Variables and Patterns

Java 22 introduced unnamed variables and patterns as a permanent feature.

An underscore `_` can be used where a value is intentionally unused in supported contexts.

Example:

```java
for (var _ : items) {
    process();
}
```

The exact contexts in which unnamed variables/patterns are permitted are defined by the language specification.

The important design idea is:

```text
"I intentionally don't need this value."
```

rather than creating a misleading variable name.

Oracle lists unnamed variables and patterns as permanent beginning in Java 22.

---

# 90. Modern Source Simplifications

Modern Java has also introduced features intended to make small programs easier to write.

As of Java 25, Oracle lists:

```text
Module import declarations
Compact source files and instance main methods
Flexible constructor bodies
```

as permanent language features.

These are worth recognizing when reading very modern Java examples, but they are not prerequisites for ordinary object-oriented Java development.

---

# 91. Virtual Threads

Although concurrency is covered deeply in Part 5, virtual threads are an important modern Java feature worth recognizing.

Virtual threads became permanent in Java 21.

Example:

```java
Thread.startVirtualThread(() ->
        System.out.println("Running"));
```

They are lightweight threads intended especially for high-throughput applications with many concurrent tasks.

Important:

> Virtual threads do not change Java's basic concurrency model, and they are not a replacement for streams.

Detailed concurrency guidance belongs in Part 5.

---

# 92. Sequenced Collections

Java 21 introduced a family of interfaces for collections with a defined encounter sequence.

Important types include:

```text
SequencedCollection
SequencedSet
SequencedMap
```

They provide a common abstraction for operations involving first/last elements and reversed views where applicable.

This is particularly useful because older collection APIs did not always expose front/back ordering through a common interface.

---

# 93. Unmodifiable Factory Methods

Modern Java provides concise collection factories:

```java
List<String> names =
        List.of("A", "B", "C");

Set<Integer> numbers =
        Set.of(1, 2, 3);

Map<String, Integer> values =
        Map.of(
            "A", 1,
            "B", 2
        );
```

These complement the functional style introduced by lambdas and streams.

---

# 94. `Optional` + Streams

These concepts often appear together.

Example:

```java
Optional<String> firstLongName =
        names.stream()
             .filter(name -> name.length() > 5)
             .findFirst();
```

Pipeline:

```text
List
 ↓
stream()
 ↓
filter()
 ↓
findFirst()
 ↓
Optional<String>
```

This is a very common modern Java pattern.

---

# 95. Collections + Streams + Records

Suppose:

```java
record Employee(
    String name,
    String department,
    int salary
) {}
```

Then:

```java
Map<String, List<Employee>> byDepartment =
        employees.stream()
                 .collect(
                     Collectors.groupingBy(
                         Employee::department
                     )
                 );
```

Here several modern features cooperate:

```text
record
  +
method reference
  +
stream
  +
collector
  +
generic collections
```

This is representative of real modern Java code.

---

# 96. Clean Modern Pipeline Example

```java
record Employee(
    String name,
    String department,
    int salary
) {}

List<Employee> employees = List.of(
    new Employee("Amit", "Engineering", 90000),
    new Employee("Riya", "HR", 70000),
    new Employee("Satyam", "Engineering", 100000)
);

List<String> highPaidEngineering =
        employees.stream()
                 .filter(employee ->
                     employee.department()
                             .equals("Engineering"))
                 .filter(employee ->
                     employee.salary() >= 90000)
                 .map(Employee::name)
                 .sorted()
                 .toList();
```

The pipeline reads almost like a specification:

```text
employees
    ↓
Engineering
    ↓
salary >= 90000
    ↓
extract names
    ↓
sort
    ↓
produce list
```

---

# 97. Common Mistakes

## Mistake 1 — Thinking a lambda is an object with its own named class

A lambda is an expression targeting a functional interface.

---

## Mistake 2 — Using `var` as dynamic typing

Wrong idea:

```text
var = dynamically typed
```

Correct:

```text
var = compile-time local type inference
```

---

## Mistake 3 — Using streams for everything

Sometimes this:

```java
for (Employee employee : employees) {
    ...
}
```

is clearer than a long stream pipeline.

Use streams when they improve expression of the transformation/processing.

---

## Mistake 4 — Treating parallel streams as automatically faster

Parallel execution has overhead and semantic considerations.

Measure before relying on it for performance.

---

## Mistake 5 — Calling `Optional.get()` everywhere

Prefer explicit absence handling.

---

## Mistake 6 — Using `LocalDateTime` when a timezone-aware instant is required

`LocalDateTime` has no timezone.

For an absolute event timestamp, consider `Instant`.

For a date/time in a specific region, consider `ZonedDateTime`.

---

## Mistake 7 — Calling records "deeply immutable"

Record components are final references, but referenced objects can still be mutable.

---

## Mistake 8 — Treating sealed classes as simply a stricter `final`

`final` allows no subclasses.

`sealed` allows a specific, controlled set of direct subclasses.

---

## Mistake 9 — Treating preview features as standard features

Preview syntax can change.

Java 27's primitive patterns are currently preview.

---

# 98. Important Version Timeline

| Feature | Java version | Status |
|---|---:|---|
| Lambda expressions | 8 | Permanent |
| Functional interfaces / `java.util.function` | 8 | Permanent |
| Stream API | 8 | Permanent |
| `Optional` | 8 | Permanent |
| `java.time` | 8 | Permanent |
| `var` | 10 | Permanent |
| Text blocks | 15 | Permanent |
| Switch expressions | 14 | Permanent |
| Records | 16 | Permanent |
| Pattern matching for `instanceof` | 16 | Permanent |
| Sealed classes | 17 | Permanent |
| Pattern matching for `switch` | 21 | Permanent |
| Record patterns | 21 | Permanent |
| Virtual threads | 21 | Permanent |
| Sequenced Collections | 21 | Permanent |
| Unnamed variables/patterns | 22 | Permanent |
| Primitive types in patterns/`instanceof`/`switch` | 27 | **Preview** |

Oracle's current Java-language change history is the authoritative release timeline for these language features.

---

# 99. Interview & Revision Points

## Lambda

1. What is a lambda expression?
2. Why does a lambda need a target type?
3. What is target typing?
4. What does "effectively final" mean?
5. How is lambda `this` different from an anonymous class's `this` behavior?

## Functional interfaces

6. What is a functional interface?
7. Can a functional interface have default methods?
8. What does `@FunctionalInterface` do?
9. Difference between `Predicate`, `Function`, `Consumer` and `Supplier`?
10. Difference between `Function` and `UnaryOperator`?

## Method references

11. What are the four common kinds of method reference?
12. When is `String::length` equivalent to a lambda?
13. What does `ArrayList::new` represent?

## Streams

14. What is the difference between a collection and a stream?
15. What are intermediate operations?
16. What are terminal operations?
17. Why are stream intermediate operations lazy?
18. Difference between `map()` and `flatMap()`?
19. Difference between `filter()` and `map()`?
20. What is `reduce()`?
21. What is a `Collector`?
22. What is `groupingBy()`?
23. Why can parallel streams be slower?
24. Can a stream be reused after a terminal operation?

## Optional

25. Why was `Optional` introduced?
26. Difference between `orElse()` and `orElseGet()`?
27. Difference between `map()` and `flatMap()` on Optional?
28. Why is `Optional.get()` discouraged?
29. Where is `Optional` primarily intended to be used?

## Date/time

30. `LocalDate` vs `LocalDateTime`?
31. `LocalDateTime` vs `Instant`?
32. `Instant` vs `ZonedDateTime`?
33. `Period` vs `Duration`?
34. Why is `java.time` preferred over legacy date APIs?

## Records

35. What is a record?
36. What does a record automatically provide?
37. Can a record contain methods?
38. Can a record implement interfaces?
39. Why is a record not deeply immutable?
40. What is a compact constructor?

## Sealed classes

41. Why use sealed classes?
42. Difference between `final`, `sealed` and `non-sealed`?
43. How do sealed classes work with exhaustive switch?

## Pattern matching

44. What is pattern matching?
45. What is a type pattern?
46. What is a record pattern?
47. What is flow scoping?

## Modern switch

48. Switch statement vs switch expression?
49. What is `yield`?
50. What is an exhaustive switch?
51. How can switch match types?
52. What is `case null`?

---

# 100. Quick Revision

## Lambda

```text
(parameters) -> expression
```

Lambda:

```text
behavior
  ↓
functional interface
```

---

## Functional interfaces

```text
Predicate<T>       → T → boolean
Function<T,R>      → T → R
Consumer<T>        → T → void
Supplier<T>        → () → T
```

---

## Method references

```text
Class::staticMethod
object::instanceMethod
Type::instanceMethod
Class::new
```

---

## Streams

```text
source
 ↓
filter
 ↓
map
 ↓
collect / toList / reduce
```

Remember:

```text
intermediate → lazy
terminal     → triggers evaluation
```

---

## Optional

```text
Optional<T>
 ├── value
 └── empty
```

Important:

```text
orElse()
orElseGet()
orElseThrow()
map()
flatMap()
```

---

## Date/Time

```text
LocalDate       → date
LocalTime       → time
LocalDateTime   → date + time, no zone
Instant         → point on UTC timeline
ZonedDateTime   → date/time + zone
Period          → date-based amount
Duration        → time-based amount
```

---

## Records

```text
record
  ↓
compact value-oriented class declaration
```

Automatically supplies important structural methods/accessors according to record rules.

---

## Sealed

```text
sealed
 ↓
controlled hierarchy
```

---

## Pattern matching

```text
test
 +
extract
 =
pattern matching
```

---

## Modern switch

```java
String result = switch (value) {
    case Type t -> ...;
    default -> ...;
};
```

Modern switch combines naturally with records and sealed hierarchies.

---

# 101. Practice Questions

## Conceptual

1. Explain why a lambda requires a target type.
2. Explain functional interfaces with `Predicate`, `Function`, `Consumer`, and `Supplier`.
3. Explain the four method-reference forms.
4. Explain the difference between `map()` and `flatMap()`.
5. Explain why intermediate stream operations are lazy.
6. Explain when a parallel stream can hurt performance.
7. Explain `Optional.orElse()` vs `orElseGet()`.
8. Explain why `LocalDateTime` cannot represent an absolute instant.
9. Explain why records are not necessarily deeply immutable.
10. Explain sealed classes and `non-sealed`.
11. Explain pattern matching for `instanceof`.
12. Explain record patterns.
13. Explain switch expressions and `yield`.
14. Explain exhaustive switch over a sealed hierarchy.
15. Explain why Java 27 primitive pattern support should currently be treated differently from finalized Java features.

---

# 102. Predict the Result

### Question 1

```java
List<Integer> numbers =
        List.of(1, 2, 3, 4, 5);

long result =
        numbers.stream()
               .filter(n -> n % 2 == 0)
               .count();

System.out.println(result);
```

---

### Question 2

```java
Optional<String> value =
        Optional.empty();

String result =
        value.orElse("Default");

System.out.println(result);
```

---

### Question 3

```java
record Person(String name, int age) {}

Person a = new Person("A", 20);
Person b = new Person("A", 20);

System.out.println(a.equals(b));
```

---

### Question 4

```java
static String describe(Object value) {
    return switch (value) {
        case String s -> "String";
        case Integer i -> "Integer";
        case null -> "Null";
        default -> "Other";
    };
}
```

Determine the result for:

```java
describe("Java");
describe(10);
describe(null);
```

---

# 103. Coding Exercises

1. Sort a list of employees using a lambda.
2. Rewrite that sorting logic using a method reference where appropriate.
3. Filter employees by salary using `Predicate`.
4. Transform employee objects into employee names using `map`.
5. Group employees by department using `groupingBy`.
6. Find the highest-paid employee using a stream.
7. Write a method returning `Optional<Employee>`.
8. Build a small date/time utility using `LocalDate`, `Instant` and `ZonedDateTime`.
9. Create a record with validation in a compact constructor.
10. Build a sealed payment hierarchy.
11. Use a pattern-matching switch over the sealed hierarchy.
12. Create nested record patterns and extract values.
13. Rewrite a traditional switch statement as a switch expression.
14. Create one example that combines records + streams + method references + collectors.

---

# 104. Final Modern Java Mental Model

The most important progression is:

```text
Java 8
  │
  ├── Lambda
  ├── Functional interfaces
  ├── Method references
  ├── Streams
  ├── Optional
  └── java.time
          │
          ▼
Modern Java style
          │
          ├── var
          ├── switch expressions
          ├── records
          ├── sealed types
          ├── pattern matching
          ├── record patterns
          ├── text blocks
          ├── virtual threads
          └── newer source-language improvements
```

The most important conceptual transformation is:

```text
Traditional Java
    ↓
Objects + interfaces + imperative loops
    ↓
Modern Java
    ↓
Objects
+
functions as values
+
declarative pipelines
+
value-oriented data
+
restricted type hierarchies
+
pattern matching
```

A particularly important combination to master is:

```java
sealed interface Shape
        permits Circle, Rectangle {}

record Circle(double radius)
        implements Shape {}

record Rectangle(double width, double height)
        implements Shape {}
```

combined with:

```java
return switch (shape) {
    case Circle c ->
        Math.PI * c.radius() * c.radius();

    case Rectangle r ->
        r.width() * r.height();
};
```

This demonstrates how **records + sealed types + pattern matching + switch expressions** work together to create concise, type-safe domain models.

---

# References

1. Oracle Java SE 27 Specifications — JLS, JVM Specification, API and tool specifications.
2. Oracle Java Language Changes Summary — Java 8 through Java 27.
3. Oracle Java SE API — `java.util.stream`.
4. Oracle Java SE API — `Optional`.
5. Oracle Java SE API — `java.time` and `java.time.format`.
6. Oracle Java Language Specification — records and sealed classes.
7. Oracle documentation — method references.
8. Oracle documentation — lambda expressions and lexical scoping.
9. OpenJDK JEP 444 — Virtual Threads, finalized in Java 21.