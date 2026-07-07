# Java Streams – Quick Revision Notes

## What is Stream API?

Java Stream API (introduced in Java 8) is used to process collections of data in a **functional programming style**.

It allows operations like:

* Filtering
* Mapping
* Sorting
* Aggregation
* Grouping

Example:

```java
users.stream()
     .filter(user -> user.getAge() > 18)
     .toList();
```

---

# Why Streams?

Traditional approach:

```java
List<String> result = new ArrayList<>();

for(String name : names){

    if(name.startsWith("A")){
        result.add(name);
    }
}
```

Using Stream:

```java
List<String> result =
    names.stream()
         .filter(n -> n.startsWith("A"))
         .toList();
```

Benefits:

* Less code
* More readable
* Functional style
* Supports parallel processing

---

# Stream Pipeline

A Stream has three parts:

```text
Source
  |
  v
Intermediate Operations
  |
  v
Terminal Operation
```

Example:

```java
list.stream()
    .filter(x -> x > 10)     // Intermediate
    .map(x -> x * 2)         // Intermediate
    .collect(Collectors.toList()); // Terminal
```

---

# Creating Streams

From Collection:

```java
List<Integer> list =
    List.of(1,2,3);

list.stream();
```

From Array:

```java
Arrays.stream(array);
```

Direct Stream:

```java
Stream.of(1,2,3);
```

---

# Intermediate Operations

Intermediate operations return another Stream.

They are lazy (execute only when terminal operation runs).

---

## filter()

Used for conditions.

```java
numbers.stream()
       .filter(n -> n > 10)
       .toList();
```

Input:

```text
5,10,20
```

Output:

```text
20
```

---

## map()

Transforms data.

Example:

```java
names.stream()
     .map(String::toUpperCase)
     .toList();
```

Output:

```text
john → JOHN
```

---

## flatMap()

Flattens nested collections.

Example:

```java
List<List<Integer>> nums;

nums.stream()
    .flatMap(List::stream)
    .toList();
```

Converts:

```text
[
 [1,2],
 [3,4]
]
```

into:

```text
[1,2,3,4]
```

---

## sorted()

Sort elements.

```java
numbers.stream()
       .sorted()
       .toList();
```

Custom:

```java
users.stream()
     .sorted(
       Comparator.comparing(User::getAge)
     );
```

---

## distinct()

Removes duplicates.

```java
list.stream()
    .distinct()
    .toList();
```

Uses:

* equals()
* hashCode()

internally.

---

## limit()

Take first N elements.

```java
stream.limit(5)
```

---

## skip()

Skip first N elements.

```java
stream.skip(5)
```

---

# Terminal Operations

Terminal operations start stream execution.

---

## collect()

Collect result.

```java
List<String> result =
    stream.collect(
        Collectors.toList()
    );
```

---

## forEach()

Iterate values.

```java
stream.forEach(System.out::println);
```

---

## count()

Counts elements.

```java
long total =
    stream.count();
```

---

## reduce()

Combines elements into one value.

Example:

```java
int sum =
numbers.stream()
       .reduce(0,
       (a,b) -> a+b);
```

Output:

```text
sum of all numbers
```

---

# min() and max()

```java
numbers.stream()
       .max(Integer::compareTo);
```

```java
numbers.stream()
       .min(Integer::compareTo);
```

Returns:

```java
Optional<T>
```

---

# anyMatch()

Checks if any element matches.

```java
users.stream()
     .anyMatch(
       u -> u.getAge()>18
     );
```

Returns boolean.

---

# allMatch()

Every element should match.

```java
numbers.stream()
       .allMatch(n -> n > 0);
```

---

# noneMatch()

No element should match.

```java
numbers.stream()
       .noneMatch(n -> n < 0);
```

---

# findFirst()

Returns first element.

```java
stream.findFirst();
```

Returns:

```java
Optional<T>
```

---

# Collectors

## toList()

```java
collect(Collectors.toList())
```

---

## toSet()

```java
collect(Collectors.toSet())
```

---

## joining()

Combine strings.

```java
names.stream()
     .collect(
       Collectors.joining(",")
     );
```

Output:

```text
A,B,C
```

---

## groupingBy()

Very important.

Groups data.

Example:

```java
users.stream()
.collect(
 Collectors.groupingBy(
     User::getDepartment
 )
);
```

Result:

```text
{
 IT=[user1,user2],
 HR=[user3]
}
```

---

## partitioningBy()

Creates two groups:

true / false

Example:

```java
users.stream()
.collect(
 Collectors.partitioningBy(
     u -> u.age > 18
 )
);
```

Output:

```text
true -> adults

false -> minors
```

---

# Parallel Stream

Runs stream operations using multiple threads.

Example:

```java
list.parallelStream()
    .forEach(System.out::println);
```

Useful for:

* Large datasets
* CPU-heavy tasks

Avoid for:

* Small lists
* Database calls
* Ordered operations

---

# Lazy Evaluation

Intermediate operations don't run immediately.

Example:

```java
stream
.filter(x -> x>10)
.map(x -> x*2);
```

Nothing happens.

Execution starts only after:

```java
collect()
count()
forEach()
```

---

# Stream vs Collection

| Collection         | Stream             |
| ------------------ | ------------------ |
| Stores data        | Processes data     |
| Can reuse          | Cannot reuse       |
| Eager              | Lazy               |
| External iteration | Internal iteration |

---

# Important Interview Questions

## Can Stream be reused?

No.

Example:

```java
Stream<Integer> s =
    list.stream();

s.count();

s.forEach(System.out::println);
```

Error:

```text
Stream already operated upon
```

---

## Does Stream modify original collection?

No.

Streams create a processing pipeline.

---

## map() vs flatMap()

map:

```text
One input → One output
```

flatMap:

```text
One input → Multiple outputs
(flatten result)
```

---

# Most Used Stream Methods

```text
Intermediate:

filter()
map()
flatMap()
sorted()
distinct()
limit()
skip()


Terminal:

collect()
forEach()
reduce()
count()
min()
max()
findFirst()
anyMatch()
```

---

# Backend Examples

Filtering users:

```java
users.stream()
     .filter(User::isActive)
     .toList();
```

Convert DTO:

```java
users.stream()
     .map(UserDto::new)
     .toList();
```

Group records:

```java
orders.stream()
      .collect(
       groupingBy(
        Order::getStatus
       )
      );
```

---

# One-Line Interview Answer

"Java Streams provide a functional way to process collections using a pipeline of lazy intermediate operations and terminal operations, enabling cleaner, declarative, and potentially parallel data processing."
