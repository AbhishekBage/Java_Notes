# Java Garbage Collection (GC) – Quick Revision Notes

## What is Garbage Collection?

Garbage Collection is the JVM process that automatically identifies and removes **unused objects from Heap memory** to free memory.

Java does not require manual memory deallocation.

Example:

```java
Student s = new Student();

s = null;
```

The object is no longer reachable and becomes eligible for GC.

---

# Where Does GC Work?

GC works mainly on **Heap Memory**.

Heap contains:

* Objects
* Arrays
* String objects

GC does NOT clean Stack memory.

Stack frames are automatically removed when methods complete.

---

# When is an Object Eligible for GC?

An object becomes eligible when it is **unreachable**.

Common cases:

## 1. Null Reference

```java
Student s = new Student();

s = null;
```

## 2. Reassign Reference

```java
Student s = new Student();

s = new Student();
```

The first object becomes unreachable.

## 3. Method Completion

```java
void test(){
    Student s = new Student();
}
```

After method ends, local reference is removed.

---

# How GC Finds Garbage?

JVM uses **Reachability Analysis**.

It starts from **GC Roots** and checks reachable objects.

GC Roots examples:

* Local variables in Stack
* Static references
* Active threads
* JNI references

If there is no path from GC Root → Object:

Object is garbage.

---

# Heap Generations

Based on the Generational Hypothesis, Heap is divided into:

```text
Heap

├── Young Generation
│
└── Old Generation
```

---

# Generational Hypothesis

Principle:

"Most objects die young, and objects that survive longer usually continue living longer."

Because of this:

* Short-lived objects → Young Gen
* Long-lived objects → Old Gen

---

# Young Generation

Stores newly created objects.

Structure:

```text
Young Generation

├── Eden Space
│
├── Survivor 0
│
└── Survivor 1
```

New objects are created in Eden.

---

# Object Lifecycle

```text
new Object()

      ↓

Eden Space

      ↓

Minor GC

      ↓

Survivor Space

      ↓
(Survives multiple GCs)

      ↓

Old Generation
```

---

# Minor GC

* Cleans Young Generation.
* Happens frequently.
* Usually fast.

Process:

1. Remove dead objects from Eden.
2. Move surviving objects to Survivor.
3. Increase object age.

---

# Object Promotion

Objects that survive multiple Minor GCs are moved to Old Generation.

This is called **Promotion**.

---

# Old Generation

Contains long-lived objects:

Examples:

* Cache objects
* Application configuration
* Long-running data structures

---

# Major GC / Full GC

Cleans Old Generation.

Compared to Minor GC:

* Less frequent
* More expensive
* Can cause application pauses

---

# Garbage Collectors

## Serial GC

* Single-threaded collector.
* Used for small applications.

## Parallel GC

* Uses multiple threads.
* Focuses on high throughput.

## G1 GC (Garbage First)

* Default in modern JVM versions.
* Balances throughput and low pause times.
* Common in backend applications.

## ZGC

* Ultra low-latency collector.
* Handles very large heaps.

---

# System.gc()

Requests garbage collection.

Example:

```java
System.gc();
```

Important:

It does NOT guarantee immediate GC.

The JVM can ignore it.

---

# finalize()

Old cleanup mechanism.

```java
protected void finalize(){

}
```

Deprecated because:

* Execution not guaranteed.
* Performance issues.

Avoid using.

---

# Java Memory Leak

Java can still have memory leaks.

Example:

```java
static List<Object> list = new ArrayList<>();

while(true){
    list.add(new Object());
}
```

Objects remain reachable, so GC cannot remove them.

Result:

OutOfMemoryError.

---

# Minor GC vs Major GC

| Feature   | Minor GC         | Major GC       |
| --------- | ---------------- | -------------- |
| Cleans    | Young Generation | Old Generation |
| Frequency | High             | Low            |
| Speed     | Faster           | Slower         |
| Objects   | Short-lived      | Long-lived     |

---

# Important Interview Questions

### Where does GC work?

Heap Memory.

### Does GC clean Stack?

No. Stack frames are removed automatically.

### When is an object garbage?

When it becomes unreachable.

### Which algorithm finds unused objects?

Reachability Analysis.

### What are GC Roots?

Starting references used to find live objects.

Examples:

* Stack references
* Static references
* Active threads

### Why divide Heap into generations?

Because of the Generational Hypothesis.

### Can Java have memory leaks?

Yes, if unused objects are still referenced.

---

# Quick Flow

```text
Object Created

      ↓

Eden

      ↓

Minor GC

      ↓

Survivor

      ↓

Promotion

      ↓

Old Generation

      ↓

Major GC

      ↓

Removed
```

---

# One-Line Interview Answer

"Java Garbage Collection is an automatic memory management process where the JVM identifies unreachable heap objects using reachability analysis and removes them. Modern JVMs divide objects into Young and Old generations based on the Generational Hypothesis to optimize collection performance."
