# Java Notes: Abstract Class vs Interface

# 1. Abstraction

Abstraction is the process of hiding implementation details and exposing only the essential functionality.

Java achieves abstraction using:

* Abstract Classes
* Interfaces

---

# 2. Abstract Class

## Definition

An abstract class is a class that cannot be instantiated. It acts as a blueprint for other classes and can contain both implemented and unimplemented methods.

```java
abstract class Animal {

    String name;

    Animal(String name) {
        this.name = name;
    }

    abstract void sound();

    void sleep() {
        System.out.println(name + " is sleeping");
    }
}
```

### Characteristics

* Cannot create objects directly.
* Can contain abstract methods.
* Can contain concrete methods.
* Can have constructors.
* Can have instance variables.
* Can have static methods.
* Can have final methods.
* Supports access modifiers (private, protected, public, package-private).

---

## Example

```java
class Dog extends Animal {

    Dog(String name) {
        super(name);
    }

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

---

## When to Use

Use an abstract class when:

* Classes share common code.
* Classes share common state (instance variables).
* You need constructors.
* You want to provide default implementations.
* There is an "IS-A" relationship.

Examples:

* Animal
* Vehicle
* Employee
* BankAccount

---

# 3. Interface

## Definition

An interface defines a contract that implementing classes must follow.

It describes **what** a class can do, not **how** it does it.

```java
interface Flyable {

    void fly();

}
```

Implementation:

```java
class Bird implements Flyable {

    @Override
    public void fly() {
        System.out.println("Bird is flying");
    }
}
```

---

## Characteristics

* Cannot be instantiated.
* Supports multiple inheritance.
* Methods are public by default.
* Variables are automatically:

  * public
  * static
  * final
* No constructors.
* Used to define capabilities.

---

## Java 8 Features

### Default Methods

```java
interface Animal {

    default void sleep() {
        System.out.println("Sleeping");
    }

}
```

### Static Methods

```java
interface MathUtil {

    static int square(int x) {
        return x * x;
    }

}
```

Usage:

```java
MathUtil.square(5);
```

---

## Java 9 Feature

Private helper methods inside interfaces.

```java
interface A {

    private void helper() {
    }

}
```

---

## When to Use

Use interfaces when:

* Multiple unrelated classes share the same behavior.
* You want loose coupling.
* You are designing APIs.
* You need multiple inheritance.

Examples:

* Runnable
* Comparable
* Serializable
* Cloneable
* AutoCloseable

---

# 4. Abstract Class vs Interface

| Feature              | Abstract Class       | Interface                                     |
| -------------------- | -------------------- | --------------------------------------------- |
| Object Creation      | ❌ No                 | ❌ No                                          |
| Constructors         | ✅ Yes                | ❌ No                                          |
| Instance Variables   | ✅ Yes                | ❌ No                                          |
| Constants            | ✅ Yes                | ✅ Yes (`public static final`)                 |
| Abstract Methods     | ✅ Yes                | ✅ Yes                                         |
| Concrete Methods     | ✅ Yes                | ✅ Yes (`default` & `static`)                  |
| Multiple Inheritance | ❌ No                 | ✅ Yes                                         |
| Access Modifiers     | All                  | Mostly public (`private` helpers from Java 9) |
| Purpose              | Share code and state | Define a contract                             |

---

# 5. Relationship

### Abstract Class → IS-A

```
Dog IS-A Animal
Car IS-A Vehicle
```

### Interface → CAN-DO

```
Bird CAN Fly
Robot CAN Charge
Phone CAN ConnectToWifi
```

---

# 6. Multiple Inheritance

Java does not allow:

```java
class A {}

class B {}

class C extends A, B {}
```

Reason:

Diamond Problem.

Interfaces solve this because they only define contracts.

```java
interface A {

    void print();

}

interface B {

    void print();

}

class C implements A, B {

    @Override
    public void print() {
        System.out.println("Hello");
    }

}
```

---

# 7. Combining Both

A class can extend one abstract class and implement multiple interfaces.

```java
abstract class Animal {

    abstract void sound();

}

interface Flyable {

    void fly();

}

interface Swimmable {

    void swim();

}

class Duck extends Animal implements Flyable, Swimmable {

    @Override
    void sound() {
        System.out.println("Quack");
    }

    @Override
    public void fly() {
        System.out.println("Flying");
    }

    @Override
    public void swim() {
        System.out.println("Swimming");
    }
}
```

---

# 8. What is an Annotation?

Annotations begin with `@`.

They provide metadata to the compiler, JVM, IDE, or frameworks.

They do **not** directly change program logic.

---

## @Override

Ensures a method correctly overrides a superclass or interface method.

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }

}
```

Benefits:

* Detects spelling mistakes.
* Improves readability.
* Compiler validation.

---

## @Deprecated

Marks APIs that should no longer be used.

```java
@Deprecated
void oldMethod() {

}
```

---

## @SuppressWarnings

Suppresses compiler warnings.

```java
@SuppressWarnings("unchecked")
List list = new ArrayList();
```

---

## @FunctionalInterface

Ensures an interface contains exactly one abstract method.

```java
@FunctionalInterface
interface Calculator {

    int add(int a, int b);

}
```

---

# 9. Common Interview Questions

### Q1. Can an abstract class implement an interface?

Yes.

```java
interface Animal {
    void sound();
}

abstract class Dog implements Animal {

}
```

---

### Q2. Can an interface extend another interface?

Yes.

```java
interface A {

}

interface B extends A {

}
```

An interface can extend multiple interfaces.

```java
interface C extends A, B {

}
```

---

### Q3. Can a class extend multiple abstract classes?

No.

Java does not support multiple inheritance of classes.

---

### Q4. Can an interface have constructors?

No.

---

### Q5. Can abstract classes have constructors?

Yes.

They are called when subclasses are instantiated.

---

### Q6. Can interfaces have variables?

Yes.

They are always:

```java
public static final
```

Example:

```java
interface Constants {

    int MAX = 100;

}
```

---

### Q7. Why were default methods introduced in Java 8?

To allow adding new methods to existing interfaces without breaking classes that already implement them.

---

# 10. Interview Rule of Thumb

Use an **Abstract Class** when:

* You want to share code.
* You want shared state (fields).
* You need constructors.
* There is an "IS-A" relationship.

Use an **Interface** when:

* You want to define behavior.
* Multiple unrelated classes should implement the same contract.
* You need multiple inheritance.
* You want loose coupling and extensibility.

---

# Quick Revision

**Abstract Class**

* Partial abstraction
* Can have fields
* Can have constructors
* Can have implemented methods
* Single inheritance
* "IS-A" relationship

**Interface**

* Contract
* No instance fields
* No constructors
* Supports multiple inheritance
* Default & static methods (Java 8)
* Private methods (Java 9)
* "CAN-DO" relationship

---

# Memory Trick

**Abstract Class = "What something IS"**

```
Dog IS-A Animal
Car IS-A Vehicle
```

**Interface = "What something CAN DO"**

```
Bird CAN Fly
Phone CAN ConnectToWifi
Robot CAN Charge
```
