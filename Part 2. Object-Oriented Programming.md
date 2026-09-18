# COMPLETE JAVA NOTES

# Part 2 — Object-Oriented Programming

**Modern baseline:** Java SE 27  
**Purpose:** Learning • Revision • Interviews • Practical Development

---

# Table of Contents

1. What is Object-Oriented Programming?
2. Classes
3. Objects
4. References and Objects
5. Constructors
6. `this`
7. `static`
8. Encapsulation
9. Access Modifiers
10. Inheritance
11. Method Overloading
12. Method Overriding
13. Polymorphism
14. Abstraction
15. Interfaces
16. Abstract Classes vs Interfaces
17. `Object` Class
18. Important Relationships
19. Common OOP Mistakes
20. Interview & Revision Points
21. Quick Revision
22. Practice Questions
23. References

---

# 1. What is Object-Oriented Programming?

Object-Oriented Programming, or **OOP**, is a programming approach in which software is designed around **objects and the types that define them**.

A useful mental model is:

```text
Class
  │
  │ creates / describes
  ▼
Object
  │
  ├── State
  └── Behavior
```

For example, imagine a `BankAccount`.

Its state could include:

```text
accountNumber
ownerName
balance
```

Its behavior could include:

```text
deposit()
withdraw()
getBalance()
```

In Java, a class describes the structure and behavior, while an object is an instance of that class.

---

# 2. Why OOP?

OOP helps organize larger programs by grouping related data and operations together.

Without good structure:

```text
data
functions
more data
more functions
...
```

With classes:

```text
BankAccount
├── accountNumber
├── ownerName
├── balance
├── deposit()
├── withdraw()
└── getBalance()
```

This supports concepts such as:

- encapsulation
- reuse
- abstraction
- inheritance
- polymorphism

These concepts are related, but they are **not interchangeable terms**.

---

# 3. Classes

## What is a class?

A class is a declaration that defines a Java type.

Example:

```java
public class BankAccount {

    private String ownerName;
    private double balance;

    public void deposit(double amount) {
        balance += amount;
    }
}
```

The class defines:

- fields
- methods
- constructors
- nested types
- other members

A class itself is not the same thing as an object.

---

## 3.1 Fields

Fields represent data associated with a class or its instances.

```java
class Person {
    String name;
    int age;
}
```

Here:

```text
name → field
age  → field
```

---

## 3.2 Methods

Methods define behavior.

```java
class Person {

    void sayHello() {
        System.out.println("Hello");
    }
}
```

---

## 3.3 Creating a class

General form:

```java
class ClassName {
    // fields
    // constructors
    // methods
}
```

Example:

```java
class Car {

    String model;
    int year;

    void start() {
        System.out.println("Car started");
    }
}
```

---

# 4. Objects

## What is an object?

An object is an **instance of a class**.

```java
Car car = new Car();
```

Here:

```text
Car
 ↑
type

car
 ↑
reference variable

new Car()
 ↑
object creation expression
```

A useful model:

```text
car ─────────► Car object
               ├── model
               ├── year
               └── start()
```

The variable `car` is a reference. It is not itself the object.

---

# 5. Creating Objects

Objects are commonly created using `new`.

```java
Car car = new Car();
```

Conceptually:

```text
new Car()
   │
   ▼
new object
   │
   ▼
reference stored in car
```

---

## Accessing members

```java
car.model = "Honda";
car.year = 2025;

car.start();
```

The dot operator accesses members through the reference.

---

# 6. Multiple Objects

A class can be used to create many independent objects.

```java
Car firstCar = new Car();
Car secondCar = new Car();

firstCar.model = "Honda";
secondCar.model = "Toyota";
```

Conceptually:

```text
firstCar  ───► Car object
                 model = Honda

secondCar ───► Car object
                 model = Toyota
```

The class defines the structure, while each object has its own instance state.

---

# 7. Reference Variables

Consider:

```java
Car car1 = new Car();
Car car2 = car1;
```

There is one object but two references:

```text
car1 ──────┐
           ├────► Car object
car2 ──────┘
```

Therefore:

```java
car2.model = "Honda";
```

can affect what you observe through `car1`, because both references refer to the same object.

---

# 8. `null`

A reference can contain `null`:

```java
Car car = null;
```

It does not refer to an object.

Trying to invoke an instance method through a null reference:

```java
car.start();
```

results in a `NullPointerException`.

Important distinction:

```text
null
  ≠
empty object
```

---

# 9. Constructors

## What is a constructor?

A constructor is a special member used when an object is created.

Example:

```java
class Person {

    private String name;

    Person(String name) {
        this.name = name;
    }
}
```

Create an object:

```java
Person person = new Person("Satyam");
```

---

## 9.1 Constructor syntax

A constructor:

- has the same name as the class
- has no return type
- is invoked as part of object creation

Example:

```java
class Employee {

    Employee() {
        System.out.println("Object created");
    }
}
```

Notice that this is **not**:

```java
void Employee() { }
```

That would be a method, not a constructor.

---

# 10. Default Constructor vs No-Argument Constructor

These terms are frequently confused.

A **default constructor** is a constructor automatically supplied by the compiler when the class declares **no constructors**.

Example:

```java
class Person {
}
```

The compiler can provide a no-argument constructor equivalent in effect to an empty constructor.

But:

```java
class Person {

    Person(String name) {
    }
}
```

does **not** automatically receive a no-argument constructor.

If you need one, declare it:

```java
class Person {

    Person() {
    }

    Person(String name) {
    }
}
```

This distinction is important in interviews.

---

# 11. Constructor Overloading

A class can have multiple constructors with different parameter lists.

```java
class Product {

    private String name;
    private double price;

    Product() {
        this("Unknown", 0.0);
    }

    Product(String name) {
        this(name, 0.0);
    }

    Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
}
```

This is constructor overloading.

---

# 12. `this`

`this` refers to the **current object** in an instance context.

Example:

```java
class Person {

    private String name;

    Person(String name) {
        this.name = name;
    }
}
```

Here:

```text
name
```

is the constructor parameter.

```text
this.name
```

is the instance field.

---

## 12.1 Calling another constructor with `this()`

A constructor can invoke another constructor in the same class:

```java
class Product {

    Product() {
        this("Unknown");
    }

    Product(String name) {
        // initialization
    }
}
```

The constructor invocation using `this(...)` must follow Java's constructor-invocation rules and occur as the first statement in the constructor body.

---

## 12.2 Passing the current object

`this` can also be passed as an argument:

```java
class Service {

    void register(Service service) {
        // ...
    }

    void start() {
        register(this);
    }
}
```

---

# 13. `static`

The `static` keyword defines members associated with the **class**, rather than with a particular instance.

Example:

```java
class Counter {

    static int totalCreated = 0;

    Counter() {
        totalCreated++;
    }
}
```

Create objects:

```java
new Counter();
new Counter();
new Counter();

System.out.println(Counter.totalCreated);
```

Conceptually:

```text
Class Counter
      │
      └── static totalCreated

Object 1 ── instance state
Object 2 ── instance state
Object 3 ── instance state
```

The static field is shared at the class level.

---

# 14. Static Methods

A static method can be called without creating an object:

```java
Math.max(10, 20);
```

A static method cannot directly use instance members without an object reference.

Example:

```java
class Example {

    int value = 10;

    static void printValue() {
        // System.out.println(value); // Compilation error
    }
}
```

There is no particular instance to supply `value`.

---

# 15. Instance vs Static

| Instance member | Static member |
|---|---|
| Associated with an object | Associated with the class |
| Usually accessed through an object/reference | Commonly accessed through the class name |
| Each object can have its own state | Shared class-level state |
| Can access instance members directly | Cannot directly access instance members |

Example:

```java
class User {

    String name;              // instance field
    static int userCount;     // static field

    void printName() { }      // instance method

    static void printCount() { } // static method
}
```

---

# 16. Encapsulation

## What is encapsulation?

Encapsulation means controlling access to an object's internal state and exposing an appropriate public interface.

A common implementation uses:

```text
private fields
     +
public methods
```

Example:

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }

        balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

External code cannot directly modify:

```java
account.balance
```

because it is private.

---

## Why encapsulation?

It allows a class to control its invariants.

For example, the class can enforce:

```text
balance must not become negative
deposit amount must be positive
```

This is more useful than simply "hiding variables."

Encapsulation is fundamentally about **controlling access and protecting a type's internal invariants**.

---

# 17. Getters and Setters

A common pattern is:

```java
class Person {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

However, don't create getters/setters mechanically for every field.

Sometimes the correct design is:

```java
public void deposit(double amount)
```

rather than:

```java
public void setBalance(double balance)
```

The former allows the class to maintain its rules.

---

# 18. Access Modifiers

Java has four access levels:

```text
public
protected
package-private
private
```

`package-private` means no access modifier is written.

---

## 18.1 `public`

Accessible wherever the declaring type/member is accessible.

```java
public void start() {
}
```

---

## 18.2 `private`

Accessible only within the declaring top-level class or enclosing class context according to Java's access rules.

```java
private double balance;
```

---

## 18.3 Package-private

No modifier:

```java
void calculate() {
}
```

Accessible to code in the same package, subject to the relevant language rules.

---

## 18.4 `protected`

`protected` is more subtle.

A protected member is accessible:

- within the same package
- and in certain contexts from subclasses in other packages

A common oversimplification is:

> "`protected` means subclasses can always access it."

That is not generally correct.

---

## Access comparison

| Modifier | Same class | Same package | Subclass outside package | Unrelated class outside package |
|---|---:|---:|---:|---:|
| `private` | Yes | No | No | No |
| package-private | Yes | Yes | No | No |
| `protected` | Yes | Yes | Yes, subject to access rules | No |
| `public` | Yes | Yes | Yes | Yes |

---

# 19. Inheritance

Inheritance allows one class to derive from another class.

Syntax:

```java
class Dog extends Animal {
}
```

Conceptually:

```text
Animal
   ▲
   │ extends
   │
  Dog
```

The subclass inherits accessible members according to Java's rules and can add or specialize behavior.

---

# 20. Example of Inheritance

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Barking");
    }
}
```

Now:

```java
Dog dog = new Dog();

dog.eat();
dog.bark();
```

The `Dog` object has access to the inherited behavior `eat()` and its own behavior `bark()`.

---

# 21. `super`

`super` can refer to the superclass portion of the current object.

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        super.sound();
        System.out.println("Dog sound");
    }
}
```

`super()` can also invoke a superclass constructor, subject to constructor invocation rules.

---

# 22. Java Class Inheritance

Java does not allow a class to extend multiple classes.

This is invalid:

```java
// class C extends A, B { }
```

Java does, however, support multiple interface inheritance.

```java
class C implements A, B {
}
```

Interfaces are discussed later in this chapter.

---

# 23. Method Overloading

Overloading means multiple methods have the same name but **different parameter lists**.

Example:

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

The parameter list differs.

---

## What does not count as overloading?

Changing only the return type is insufficient.

This is invalid:

```java
// int getValue() { ... }
// double getValue() { ... }
```

The compiler cannot distinguish them solely by return type.

---

# 24. Method Overriding

Overriding occurs when a subclass provides a new implementation of an inherited instance method that is eligible for overriding.

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

The `@Override` annotation is strongly recommended because it lets the compiler verify your intention.

---

# 25. Overloading vs Overriding

| Feature | Overloading | Overriding |
|---|---|---|
| Usually related to | Same class or inheritance hierarchy | Inheritance |
| Method name | Same | Same |
| Parameters | Must differ | Must match according to overriding rules |
| Return type alone enough? | No | Return type must be compatible |
| Resolved | Primarily at compile time | Dynamically for overridden instance methods |
| Purpose | Multiple ways to call similar behavior | Specialize inherited behavior |

---

# 26. Rules for Overriding

Important rules include:

- the method must be an eligible inherited instance method
- the overriding method cannot reduce accessibility
- return type must be compatible
- checked exceptions must obey overriding rules
- `final` methods cannot be overridden
- `private` methods are not overridden
- `static` methods are hidden, not overridden

Example:

```java
class Parent {

    protected void show() {
    }
}

class Child extends Parent {

    @Override
    public void show() {
    }
}
```

Increasing accessibility is allowed.

---

# 27. Polymorphism

Polymorphism means that the same operation can work through different underlying object types.

One of the most important forms in Java is **subtype polymorphism**.

Example:

```java
Animal animal = new Dog();
animal.sound();
```

If `Dog` overrides `sound()`, the `Dog` implementation is selected at runtime.

Conceptually:

```text
Reference type: Animal
              │
              ▼
         Dog object
              │
              ▼
       Dog.sound()
```

---

# 28. Reference Type vs Object Type

This is a crucial interview concept.

```java
Animal animal = new Dog();
```

There are two relevant types:

```text
Animal
  ↑
  │ reference / compile-time type

Dog
  ↑
  │ object / runtime type
```

The compiler uses the **reference's compile-time type** to determine what members are available through the variable.

At runtime, overridden instance methods use the **actual object's implementation**.

---

## Example

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }

    void fetch() {
        System.out.println("Fetching");
    }
}
```

Then:

```java
Animal animal = new Dog();

animal.sound();  // Bark
animal.eat();    // Eating
// animal.fetch(); // Compilation error
```

Why?

The compiler sees `animal` as an `Animal` reference, so `fetch()` is not available through that reference.

But the runtime object is a `Dog`, so the overridden `sound()` implementation is used.

This is the essence of dynamic method dispatch.

---

# 29. Casting References

A reference can sometimes be narrowed:

```java
Animal animal = new Dog();

Dog dog = (Dog) animal;
```

The cast is valid here because the runtime object really is a `Dog`.

But:

```java
Animal animal = new Cat();

Dog dog = (Dog) animal;
```

can result in `ClassCastException` at runtime.

Before casting, consider whether the design can avoid the need for explicit downcasting.

---

# 30. Abstraction

Abstraction means exposing relevant behavior while hiding implementation details.

For example:

```text
PaymentService
    pay()
```

A user of the API may need to know:

```text
"call pay"
```

without needing to know every internal implementation detail.

Java supports abstraction primarily through:

- abstract classes
- interfaces

---

# 31. Abstract Classes

An abstract class cannot be instantiated directly.

```java
abstract class Shape {

    abstract double area();

    void printDescription() {
        System.out.println("A geometric shape");
    }
}
```

A subclass can implement the abstract method:

```java
class Circle extends Shape {

    private final double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}
```

You cannot do:

```java
// Shape shape = new Shape();
```

but you can do:

```java
Shape shape = new Circle(5);
```

---

# 32. Abstract Methods

An abstract method declares behavior without providing the implementation in the abstract class.

```java
abstract double area();
```

A concrete subclass generally must implement inherited abstract methods unless it is itself abstract.

---

# 33. Interfaces

An interface defines a contract that classes can implement.

Example:

```java
interface Payable {

    void pay(double amount);
}
```

Implementation:

```java
class CreditCardPayment implements Payable {

    @Override
    public void pay(double amount) {
        System.out.println("Paid: " + amount);
    }
}
```

Usage:

```java
Payable payment = new CreditCardPayment();
payment.pay(500);
```

---

# 34. Interface Methods

Modern Java interfaces can contain more than abstract method declarations.

They can include:

- abstract methods
- default methods
- static methods
- private methods

Example:

```java
interface Logger {

    void log(String message);

    default void info(String message) {
        log("INFO: " + message);
    }

    static String version() {
        return "1.0";
    }

    private String normalize(String value) {
        return value.trim();
    }
}
```

Not all interface members are inherited or accessed in the same way; static and private interface methods have specific rules.

---

# 35. Multiple Interfaces

A class can implement multiple interfaces:

```java
interface Printable {
    void print();
}

interface Exportable {
    void export();
}

class Report implements Printable, Exportable {

    @Override
    public void print() {
        System.out.println("Printing");
    }

    @Override
    public void export() {
        System.out.println("Exporting");
    }
}
```

This provides a form of multiple inheritance of type/contracts without multiple class inheritance.

---

# 36. Default Method Conflict

Suppose two interfaces provide the same default method:

```java
interface A {
    default void show() {
        System.out.println("A");
    }
}

interface B {
    default void show() {
        System.out.println("B");
    }
}
```

A class implementing both generally must resolve the conflict:

```java
class C implements A, B {

    @Override
    public void show() {
        A.super.show();
    }
}
```

The language has specific rules for resolving inherited default methods.

---

# 37. Abstract Class vs Interface

| Abstract class | Interface |
|---|---|
| Declared with `abstract class` | Declared with `interface` |
| Can have instance fields | Fields are constants (`public static final` semantics for declared fields) |
| Can have constructors | Cannot have constructors |
| A class extends one class | A class can implement multiple interfaces |
| Can contain concrete and abstract methods | Can contain abstract, default, static and private methods |
| Useful for shared implementation/state | Useful for contracts/capabilities and multiple type inheritance |

Choose based on design, not simply on a rigid rule such as "always use interfaces."

---

# 38. `Object` Class

Every Java class ultimately derives from `Object`, except that `Object` itself has no superclass.

Example:

```text
Object
  ▲
  │
Animal
  ▲
  │
Dog
```

Therefore methods from `Object` are fundamental to ordinary Java objects.

Important methods include:

```text
toString()
equals(Object)
hashCode()
getClass()
clone()
finalize()   ← historical/deprecated; not for new development
```

---

# 39. `toString()`

The default `Object.toString()` representation is generally not what application code wants.

Override it when a useful textual representation is appropriate:

```java
class Person {

    private final String name;

    Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{name='%s'}".formatted(name);
    }
}
```

Then:

```java
Person person = new Person("Satyam");

System.out.println(person);
```

prints a meaningful description.

---

# 40. `equals()`

`equals()` is used to compare objects for logical equality according to the class's chosen equality definition.

Example:

```java
class Person {

    private final String id;

    Person(String id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object other) {
        if (this == other) {
            return true;
        }

        if (!(other instanceof Person person)) {
            return false;
        }

        return id.equals(person.id);
    }

    @Override
    public int hashCode() {
        return id.hashCode();
    }
}
```

---

# 41. `equals()` and `hashCode()`

This relationship is critical:

> If two objects are equal according to `equals()`, they must have the same `hashCode()`.

The reverse is not required.

```text
a.equals(b) == true
        ↓
a.hashCode() == b.hashCode()
```

This matters enormously when objects are stored in hash-based collections such as `HashMap` and `HashSet`.

---

# 42. `getClass()`

`getClass()` returns the runtime class of an object.

Example:

```java
Object value = "Java";

System.out.println(value.getClass());
```

This is useful when runtime type information is required, although normal application design should not unnecessarily depend on runtime type inspection.

---

# 43. `final` and Inheritance

A class declared `final` cannot be extended:

```java
final class Utility {
}
```

This is invalid:

```java
// class Child extends Utility { }
```

A `final` method cannot be overridden:

```java
class Parent {

    final void show() {
    }
}
```

A subclass cannot provide another implementation of that method.

---

# 44. Static Method Hiding

Static methods participate in **method hiding**, not overriding.

Example:

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void show() {
        System.out.println("Child");
    }
}
```

Then:

```java
Parent value = new Child();
value.show();
```

does not use runtime overriding dispatch in the way an instance method would.

This is a classic interview trap.

---

# 45. Constructors and Inheritance

Constructors are **not inherited**.

When a subclass object is created, superclass construction is still part of object initialization.

Example:

```java
class Parent {

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child constructor");
    }
}
```

Creating:

```java
new Child();
```

causes superclass construction before the subclass constructor body completes.

---

# 46. Initialization Relationship

A simplified picture:

```text
Create Child object
       │
       ▼
Superclass initialization
       │
       ▼
Superclass constructor
       │
       ▼
Subclass initialization
       │
       ▼
Subclass constructor
```

Actual initialization rules include field initializers, instance initializers and constructor invocation ordering, so this simplified diagram should be used as a learning model rather than a complete JVM execution trace.

---

# 47. Four Core OOP Ideas

A useful study model:

```text
                 OOP
                  │
      ┌───────────┼───────────┐
      │           │           │
Encapsulation  Inheritance  Polymorphism
      │                       │
      └────── Abstraction ────┘
```

### Encapsulation

Control access to state and protect invariants.

### Inheritance

Create a subtype relationship between classes.

### Polymorphism

Use a common type while allowing different implementations.

### Abstraction

Expose essential behavior while hiding unnecessary implementation details.

---

# 48. Common OOP Mistakes

## Mistake 1 — "Class = Object"

Incorrect.

```text
Class  → describes a type
Object → instance of that type
```

---

## Mistake 2 — "Reference = Object"

Not necessarily.

```java
Animal animal = new Dog();
```

The reference type is `Animal`, while the runtime object is a `Dog`.

---

## Mistake 3 — Confusing overloading and overriding

Remember:

```text
Overloading  → different parameter lists
Overriding   → subclass replaces inherited instance behavior
```

---

## Mistake 4 — Thinking static methods are overridden

They are hidden, not overridden.

---

## Mistake 5 — Thinking private methods are overridden

A private method is not inherited in a way that makes subclass overriding possible.

A same-signature method in the subclass is a separate method.

---

## Mistake 6 — Using inheritance only for code reuse

Inheritance represents a type relationship.

If the relationship is not conceptually appropriate, composition is often a better design.

---

# 49. Composition vs Inheritance

Instead of:

```text
Car extends Engine
```

you generally model:

```text
Car
 └── has an Engine
```

Example:

```java
class Engine {

    void start() {
        System.out.println("Engine started");
    }
}

class Car {

    private final Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }

    void start() {
        engine.start();
        System.out.println("Car started");
    }
}
```

This is **composition**.

A useful design principle is:

> Prefer composition when the relationship is "has-a"; use inheritance when the subtype relationship genuinely models the domain.

This is a design guideline, not an absolute prohibition against inheritance.

---

# 50. Important Concept Connections

The OOP concepts build on one another:

```text
Class
  ↓
Object
  ↓
Constructor
  ↓
Encapsulation
  ↓
Inheritance
  ↓
Overriding
  ↓
Polymorphism
```

Interfaces add another route to abstraction:

```text
Interface
    ↓
implements
    ↓
Class
    ↓
Object
```

And everything connects to `Object`:

```text
Object
  ▲
  │
all ordinary Java classes
```

---

# 51. Interview & Revision Points

### Classes and objects

1. What is the difference between a class and an object?
2. What is a reference variable?
3. Can multiple references point to the same object?
4. What happens when a reference contains `null`?

### Constructors

5. What is a constructor?
6. Can a constructor have a return type?
7. What is the difference between a default constructor and a no-argument constructor?
8. Are constructors inherited?
9. Can constructors be overloaded?
10. What is `this()`?

### `this` and `static`

11. What does `this` mean?
12. Why is `this` useful when constructor parameters have the same names as fields?
13. What is the difference between an instance field and a static field?
14. Why can't a static method directly access an instance field?
15. Can a static method access an object through a supplied reference?

### Encapsulation/access

16. What is encapsulation?
17. Why should fields often be private?
18. What are Java's four access levels?
19. Why is `protected` more complicated than "subclasses can access it"?

### Inheritance

20. What is inheritance?
21. Does Java support multiple inheritance of classes?
22. What is `super`?
23. Are constructors inherited?

### Overloading/overriding

24. What is method overloading?
25. Can methods be overloaded by return type alone?
26. What is method overriding?
27. Can a static method be overridden?
28. Can a private method be overridden?
29. What does `@Override` do?

### Polymorphism

30. Explain:

```java
Animal animal = new Dog();
```

31. Which type determines what methods are visible to the compiler?
32. Which object's overridden implementation executes at runtime?
33. What can cause `ClassCastException`?

### Abstraction/interfaces

34. What is abstraction?
35. What is an abstract class?
36. Can an abstract class have constructors?
37. Can an abstract class have concrete methods?
38. Can a class implement multiple interfaces?
39. What are default interface methods?
40. What is a default-method conflict?

### `Object`

41. Why is `Object` important?
42. What is the contract between `equals()` and `hashCode()`?
43. Why override `toString()`?
44. What does `getClass()` return?
45. What happened to `finalize()`?

---

# 52. Quick Revision

## Classes and objects

```text
Class  = type definition
Object = instance
Reference = variable that refers to an object
```

## Constructors

```text
constructor
    ↓
used during object creation
```

Important:

```text
No declared constructor
      ↓
compiler may provide default constructor
```

But if you declare any constructor, don't assume a no-argument constructor is automatically added.

---

## `this`

```text
this
├── current object
├── disambiguates fields/parameters
└── invokes another constructor with this(...)
```

---

## `static`

```text
static
  ↓
associated with class-level context
```

Instance:

```text
object-specific
```

Static:

```text
class-associated
```

---

## Encapsulation

```text
private state
     +
controlled public API
     =
encapsulation
```

---

## Inheritance

```text
Parent
  ▲
  │
Child
```

Java:

```text
one superclass
multiple interfaces
```

---

## Overloading

```text
same method name
different parameter list
```

## Overriding

```text
subclass
   ↓
specialized inherited instance method
```

---

## Polymorphism

```java
Animal animal = new Dog();
animal.sound();
```

Compile time:

```text
Animal reference
```

Runtime:

```text
Dog object
```

---

## Abstraction

```text
what
 ↓
interface / abstract type
```

Implementation details can remain behind the abstraction.

---

## `Object`

Remember:

```text
equals()
hashCode()
toString()
getClass()
```

And:

```text
equals(a, b)
    ↓
same hashCode required when equal
```

---

# 53. Practice Questions

## Conceptual

1. Explain class, object and reference using a real-world analogy.
2. Explain the difference between a default constructor and a no-argument constructor.
3. Explain why constructors are not inherited.
4. Explain instance vs static members.
5. Explain encapsulation with an example where a setter would be a bad design.
6. Explain inheritance vs composition.
7. Explain overloading vs overriding.
8. Explain runtime polymorphism using:

```java
Animal a = new Dog();
```

9. Explain abstract class vs interface.
10. Explain the `equals()` / `hashCode()` contract.

---

## Predict the output

### Question 1

```java
class Parent {

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    @Override
    void show() {
        System.out.println("Child");
    }
}

public class Main {

    public static void main(String[] args) {
        Parent value = new Child();
        value.show();
    }
}
```

---

### Question 2

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void show() {
        System.out.println("Child");
    }
}

public class Main {

    public static void main(String[] args) {
        Parent value = new Child();
        value.show();
    }
}
```

---

### Question 3

```java
class Counter {

    static int count;

    Counter() {
        count++;
    }
}

public class Main {

    public static void main(String[] args) {
        new Counter();
        new Counter();
        new Counter();

        System.out.println(Counter.count);
    }
}
```

---

### Question 4

```java
class Parent {

    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child");
    }
}

public class Main {

    public static void main(String[] args) {
        new Child();
    }
}
```

---

# 54. Coding Exercises

1. Create a `BankAccount` class with private state and methods for deposit and withdrawal.
2. Create a `Student` class with overloaded constructors.
3. Create a `Shape` hierarchy using an abstract class.
4. Create `Payable` and `Refundable` interfaces and implement both in a class.
5. Demonstrate method overloading using a calculator.
6. Demonstrate method overriding using `Animal`, `Dog` and `Cat`.
7. Demonstrate runtime polymorphism using a list of `Animal` references.
8. Override `equals()`, `hashCode()` and `toString()` correctly for a small domain class.
9. Create an example demonstrating the difference between static method hiding and instance-method overriding.
10. Refactor an inheritance-based design into composition.

**Try these yourself before looking for solutions.**

---

# 55. Final Mental Model

The entire Part 2 can be remembered as:

```text
                  CLASS
                    │
                    ▼
                 OBJECT
                    │
              ┌─────┴─────┐
              │           │
            STATE      BEHAVIOR
              │           │
              └─────┬─────┘
                    │
             ENCAPSULATION
                    │
          ┌─────────┴─────────┐
          │                   │
     INHERITANCE          INTERFACE
          │                   │
          ▼                   ▼
     OVERRIDING          ABSTRACTION
          │                   │
          └─────────┬─────────┘
                    ▼
              POLYMORPHISM
```

The key Java relationship is:

```java
Animal animal = new Dog();
```

One line demonstrates several core ideas:

```text
Animal  → reference type / abstraction
Dog     → runtime object type
new     → object creation
extends → inheritance
@Override → overriding
sound() → polymorphic dispatch
```

That relationship is worth mastering because it appears throughout real-world Java applications.

---

# References

- Oracle Java Language Specification — Classes, Objects, Inheritance and Interfaces.
- Oracle Java Virtual Machine Specification.
- Java SE API Documentation — `Object`, reflection and standard APIs.
- Oracle Java Tutorials / Language documentation for classes, objects and inheritance.
- Java SE Platform and Development Kit documentation.