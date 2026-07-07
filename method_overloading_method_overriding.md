# Java: Method Overloading vs Method Overriding (Quick Revision)

# Method Overloading

### Definition

Multiple methods with the **same name** but **different parameter lists** in the **same class**.

### Key Points

* Compile-time polymorphism (Static Binding).
* Method name must be the same.
* Parameters must differ (number, type, or order).
* Return type alone **cannot** overload a method.
* Constructors can be overloaded.
* Static methods can be overloaded.

### Example

```java
class Calculator {

    int add(int a, int b) { return a + b; }

    int add(int a, int b, int c) { return a + b + c; }

    double add(double a, double b) { return a + b; }
}
```

---

# Method Overriding

### Definition

A child class provides its **own implementation** of a method already defined in the parent class.

### Key Points

* Runtime polymorphism (Dynamic Method Dispatch).
* Method name and parameters must be the same as in the parent class.
* Must have an inheritance relationship (IS-A).
* Return type must be the same or covariant.
* Access modifier cannot be more restrictive than the parent method.
* Final methods cannot be overridden.
* Static methods cannot be overridden (they can be hidden).
* Use `@Override` annotation for clarity and error checking.

### Example

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

---

# Key Differences

| Feature            | Method Overloading                 | Method Overriding                  |
| ------------------ | ---------------------------------- | ---------------------------------- |
| Definition         | Same method name, different params | Same method, redefined in subclass |
| Polymorphism Type  | Compile-time                       | Runtime                            |
| Inheritance Needed | No                                 | Yes                                |
| Parameters         | Must differ                        | Must be the same                   |
| Return Type        | Can differ (not alone)             | Must be same or covariant          |
| Access Modifier    | No restriction                     | Cannot be more restrictive         |
| Static Methods     | Can be overloaded                  | Cannot be overridden               |
| Binding            | Static Binding                     | Dynamic Binding                    |
