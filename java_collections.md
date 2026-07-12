# Java Collections Framework – Quick Revision Notes

## What is Java Collections Framework (JCF)?

The Java Collections Framework (JCF) is a set of interfaces and classes used to **store, manipulate, and retrieve groups of objects efficiently**.

Provides:

* Dynamic storage
* Searching
* Sorting
* Iteration
* Thread-safe collections
* Utility algorithms

Package:

```text id="srsjlwm"
java.util
```

---

# Collection Hierarchy

```text id="s4cdgz"
                  Iterable
                      |
                 Collection
          _________|_________
         |         |         |
       List       Set      Queue
         |         |         |
   ArrayList   HashSet   PriorityQueue
   LinkedList  TreeSet   ArrayDeque
   Vector      LinkedHashSet

Map (Separate Hierarchy)
    |
 HashMap
 LinkedHashMap
 TreeMap
 Hashtable
```

---

# Iterable

Root interface for iteration.

Provides:

```java id="n5v9yu"
iterator()
```

Allows:

```java id="dcmmjk"
for(Object obj : list)
```

---

# Collection Interface

Parent of:

* List
* Set
* Queue

Common methods:

```java id="r9bhic"
add()
remove()
contains()
size()
isEmpty()
clear()
```

---

# List

Characteristics:

* Ordered
* Allows duplicates
* Index-based
* Maintains insertion order

Implementations:

* ArrayList
* LinkedList
* Vector

Example:

```java id="t95g9j"
List<String> list =
    new ArrayList<>();
```

---

# ArrayList

Uses a **dynamic array** internally.

Advantages:

* Fast random access (O(1))
* Good for read-heavy operations

Disadvantages:

* Slow insert/delete in the middle (O(n))

Allows:

* Duplicates
* Null values

---

# LinkedList

Uses a **Doubly Linked List**.

Advantages:

* Fast insertion/deletion (O(1) at known position)

Disadvantages:

* Slow random access (O(n))

Also implements:

```text id="cq8lgh"
Deque
Queue
```

---

# Vector

Dynamic array like ArrayList.

Difference:

* Synchronized
* Thread-safe
* Slower

Rarely used in modern applications.

---

# Set

Characteristics:

* No duplicate elements
* No index
* Uses `equals()` and `hashCode()` to determine uniqueness

Implementations:

* HashSet
* LinkedHashSet
* TreeSet

---

# HashSet

Uses:

```text id="yzaxui"
HashMap internally
```

Features:

* No duplicates
* Unordered
* One null allowed
* Average O(1) operations

Uses:

* `equals()`
* `hashCode()`

---

# LinkedHashSet

Same as HashSet but:

* Maintains insertion order

Internally:

```text id="4ftjms"
LinkedHashMap
```

---

# TreeSet

Stores elements in **sorted order**.

Internally:

```text id="g6q50t"
Red-Black Tree
```

Complexity:

```text id="ydx7an"
O(log n)
```

No null values.

---

# Queue

FIFO (First In, First Out).

Main implementations:

* PriorityQueue
* ArrayDeque

Common methods:

```java id="4ymiyr"
offer()
poll()
peek()
```

---

# PriorityQueue

Elements ordered by:

* Natural ordering
* Comparator

Does NOT maintain insertion order.

Internally:

```text id="j4k6z6"
Binary Heap
```

Operations:

```text id="dkqecm"
offer() O(log n)

poll() O(log n)

peek() O(1)
```

---

# ArrayDeque

Double-ended queue.

Supports insertion/removal from both ends.

Faster than:

* Stack
* LinkedList (for queue operations)

Methods:

```java id="g0ydbj"
addFirst()

addLast()

removeFirst()

removeLast()
```

---

# Map

Map is **NOT** part of the Collection interface.

Stores:

```text id="wex5mr"
Key → Value
```

Keys:

* Unique

Values:

* Can duplicate

---

# HashMap

Most commonly used Map.

Characteristics:

* No ordering
* One null key
* Multiple null values
* Average O(1)

Internally:

```text id="pvrfeq"
Hash Table

(Java 8: Bucket + Red-Black Tree)
```

Uses:

* `equals()`
* `hashCode()`

---

# LinkedHashMap

Same as HashMap but:

Maintains insertion order.

Internally:

```text id="b6q6z6"
Hash Table
+
Doubly Linked List
```

---

# TreeMap

Stores keys in sorted order.

Internally:

```text id="i7h17c"
Red-Black Tree
```

Complexity:

```text id="0zh0ls"
O(log n)
```

No null keys.

---

# Hashtable

Older synchronized Map.

Features:

* Thread-safe
* No null key
* No null value

Mostly replaced by:

```text id="8eyb7m"
ConcurrentHashMap
```

---

# Time Complexity

| Collection    |  Search  |  Insert  |  Delete  |
| ------------- | :------: | :------: | :------: |
| ArrayList     |   O(1)   |   O(1)*  |   O(n)   |
| LinkedList    |   O(n)   |  O(1)**  |  O(1)**  |
| HashSet       |   O(1)   |   O(1)   |   O(1)   |
| TreeSet       | O(log n) | O(log n) | O(log n) |
| HashMap       |   O(1)   |   O(1)   |   O(1)   |
| TreeMap       | O(log n) | O(log n) | O(log n) |
| PriorityQueue |   O(n)   | O(log n) | O(log n) |

* At end (amortized).
** When node/position is already known.

---

# When to Use What?

## Need ordered list?

```text id="2eozhy"
ArrayList
```

---

## Frequent insert/delete?

```text id="6x1pfa"
LinkedList
```

---

## Unique elements?

```text id="02jn2t"
HashSet
```

---

## Sorted unique elements?

```text id="vsk4ij"
TreeSet
```

---

## Key-Value storage?

```text id="4e2m4i"
HashMap
```

---

## Sorted key-value?

```text id="p0i9f9"
TreeMap
```

---

## FIFO queue?

```text id="6njlwm"
ArrayDeque
```

---

## Priority-based processing?

```text id="5jj0tr"
PriorityQueue
```

---

# Interview Questions

### Difference between Collection and Collections?

**Collection**

Interface.

Example:

```java id="pbjlwm"
Collection<Integer> c;
```

**Collections**

Utility class.

Methods:

```java id="9m9ylf"
Collections.sort()

Collections.reverse()

Collections.shuffle()
```

---

### Is Map part of Collection?

No.

It is a separate hierarchy.

---

### Which collections use equals() and hashCode()?

* HashMap
* HashSet
* LinkedHashMap
* LinkedHashSet
* Hashtable

---

### Which collections maintain insertion order?

* ArrayList
* LinkedList
* LinkedHashSet
* LinkedHashMap

---

### Which collections sort data automatically?

* TreeSet
* TreeMap
* PriorityQueue (priority order, not full sorting)

---

# Quick Memory

```text id="5v5eut"
List
-----
Ordered
Duplicates
Index

Set
----
Unique
No Index

Queue
------
FIFO

Map
----
Key → Value

Hash = Fast

Tree = Sorted

Linked = Ordered
```

---

# One-Line Interview Summary

* **ArrayList** → Dynamic array, fast random access.
* **LinkedList** → Doubly linked list, fast insert/delete.
* **HashSet** → Unique elements using hashing.
* **TreeSet** → Sorted unique elements.
* **HashMap** → Fast key-value storage using hashing.
* **TreeMap** → Sorted key-value storage.
* **PriorityQueue** → Priority-based processing.
* **ArrayDeque** → Efficient double-ended queue.
