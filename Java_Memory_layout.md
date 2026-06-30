# Java Memory Layout – Quick Revision Notes

> **Interview Note:** Beginners often refer to Java Memory Layout (Stack, Heap, Method Area). The official Java Memory Model (thread visibility, `volatile`, `synchronized`) is a separate advanced topic.

---

# JVM Memory Areas

| Memory Area             | Stores                                            |
| ----------------------- | ------------------------------------------------- |
| **Stack**               | Method calls, local variables, references         |
| **Heap**                | Objects, arrays, String Pool                      |
| **Method Area**         | Class metadata, static variables, bytecode        |
| **PC Register**         | Current instruction being executed (rarely asked) |
| **Native Method Stack** | Native (JNI) method execution                     |

---

# Stack Memory

* Stores **local variables**.
* Stores **method call stack (stack frames)**.
* Stores **references** to heap objects.
* Each thread has its own stack.
* Memory is automatically freed when a method returns.

Example:

```java
int x = 10;
Student s = new Student();
```

* `x` → Stack
* `s` (reference) → Stack
* `Student` object → Heap

---

# Heap Memory

* Stores **objects** and **arrays**.
* Shared among all threads.
* Objects created using `new`.
* Managed by the Garbage Collector.

Example:

```java
Student s = new Student();
```

* Object → Heap
* Reference `s` → Stack

---

# Method Area

Stores:

* Class metadata
* Method bytecode
* Static variables
* Runtime Constant Pool

Example:

```java
class Student {
    static String college = "ABC";
}
```

`college` → Method Area (one shared copy)

---

# Variable Storage

| Variable Type     | Stored In            |
| ----------------- | -------------------- |
| Local Variable    | Stack                |
| Instance Variable | Heap (inside object) |
| Static Variable   | Method Area          |

---

# String Pool

```java
String s = "Hello";
```

* String literals are stored in the **String Pool** (inside Heap).
* Duplicate literals share the same object.

---

# Garbage Collection

An object becomes **eligible for Garbage Collection** when **no reachable reference** points to it.

```java
Student s = new Student();
s = null;
```

The object can now be reclaimed by the JVM.

---

# Method Calls

Each method call creates a **Stack Frame**.

```
main()
   ↓
display()
```

When `display()` finishes, its stack frame is removed automatically.

---

# Java is Pass-by-Value

Java always passes **copies of values**.

* Primitive → value copied.
* Object → **reference value** copied.

Both references can point to the same object.

---

# Interview Questions

* Where are objects stored? → **Heap**
* Where are local variables stored? → **Stack**
* Where are static variables stored? → **Method Area**
* Why are static variables shared? → **One copy per class**
* What is a Stack Frame? → **Memory for one method call**
* When is an object garbage collected? → **No reachable references**

---

# Memory Diagram

```
           JVM Memory
               |
--------------------------------
|            |                 |
Stack       Heap          Method Area
|            |                 |
Local        Objects      Class Metadata
Variables    Arrays       Static Variables
References   String Pool  Bytecode
Method Calls
```

---

# Memory Tricks

* **Stack** → "Temporary" (Method execution)
* **Heap** → "Objects"
* **Method Area** → "Shared Class Information"

---

# One-Line Summary

* **Stack** → Local variables & method calls.
* **Heap** → Objects & arrays.
* **Method Area** → Static members & class metadata.
* **Garbage Collector** → Cleans unreachable heap objects.
* **Java is always Pass-by-Value** (copies the reference value for objects).
