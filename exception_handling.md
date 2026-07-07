# Java Exception Handling – Quick Revision Notes

## What is an Exception?

An exception is an unexpected event that interrupts the normal execution flow of a program.

Example:

```java
int result = 10 / 0;
```

Output:

```text
ArithmeticException
```

Exception Handling allows the program to handle failures gracefully instead of crashing.

---

# Exception Hierarchy

```text
                 Object
                    |
                Throwable
                    |
        -----------------------
        |                     |
      Error              Exception
                              |
                 -----------------------
                 |                     |
        Checked Exception     Unchecked Exception
                              (RuntimeException)
```

---

# Error

Serious JVM-level problems.

Usually not handled by applications.

Examples:

* StackOverflowError
* OutOfMemoryError

---

# Checked Exception

Checked during **compile time**.

Compiler forces handling.

Examples:

* IOException
* SQLException
* FileNotFoundException

Example:

```java
try {

    FileReader file =
        new FileReader("abc.txt");

}
catch(IOException e){

}
```

---

# Unchecked Exception

Occurs during **runtime**.

Compiler does not force handling.

Extends `RuntimeException`.

Examples:

* NullPointerException
* ArithmeticException
* ArrayIndexOutOfBoundsException
* IllegalArgumentException

Example:

```java
int x = 10 / 0;
```

---

# Checked vs Unchecked Exception

| Checked            | Unchecked                |
| ------------------ | ------------------------ |
| Compile-time check | Runtime check            |
| Must handle        | Optional handling        |
| Extends Exception  | Extends RuntimeException |
| External failures  | Programming mistakes     |
| IOException        | NullPointerException     |

---

# try-catch

Used to handle exceptions.

```java
try {

    riskyCode();

}
catch(Exception e){

    handleError();

}
```

---

# Multiple Catch Blocks

Specific exceptions should come first.

Correct:

```java
catch(IOException e){

}
catch(Exception e){

}
```

Wrong:

```java
catch(Exception e){

}
catch(IOException e){

}
```

Parent exception catches everything.

---

# finally Block

Always executes after try/catch.

Used for cleanup.

Example:

```java
try {

}
catch(Exception e){

}
finally {

    closeConnection();

}
```

Note:

`finally` may not execute if:

* JVM crashes
* System.exit() is called

---

# throw Keyword

Used to manually throw an exception.

Example:

```java
if(age < 18){

    throw new IllegalArgumentException(
        "Invalid age"
    );

}
```

Purpose:

Actually creates and throws an exception.

---

# throws Keyword

Used in method declaration.

It tells the caller:

"This method may throw an exception."

Example:

```java
void readFile()
        throws IOException {

}
```

---

# throw vs throws

| throw                    | throws                       |
| ------------------------ | ---------------------------- |
| Throws exception         | Declares exception           |
| Inside method body       | Method signature             |
| Creates exception object | Passes responsibility        |
| One exception object     | Multiple exceptions possible |

Example:

```java
throw new Exception();
```

vs

```java
void test() throws Exception
```

---

# Exception Propagation

Exception moves up the call stack until handled.

Flow:

```text
repository()

     ↓

service()

     ↓

controller()
```

If lower layer doesn't handle it, caller receives it.

---

# Custom Exception

Create application-specific exceptions.

Example:

```java
class UserNotFoundException
        extends RuntimeException {


    UserNotFoundException(String message){

        super(message);
    }
}
```

Usage:

```java
throw new UserNotFoundException(
    "User not found"
);
```

---

# Try-With-Resources

Automatically closes resources.

Example:

```java
try(
    FileReader file =
        new FileReader("abc.txt")
){

}
```

Works with classes implementing:

```text
AutoCloseable
```

---

# Spring Boot Usage

Common pattern:

Service:

```java
if(user == null){

    throw new UserNotFoundException();

}
```

Global handler:

```java
@RestControllerAdvice

@ExceptionHandler(UserNotFoundException.class)
```

Used for centralized API error handling.

---

# Important Interview Points

## Can Error be caught?

Yes technically, but usually should not.

---

## Can finally override return?

Yes.

Example:

```java
try{
    return 1;
}
finally{
    return 2;
}
```

Returns:

```text
2
```

---

## Can constructor throw exception?

Yes.

---

## Can try exist without catch?

Yes.

Example:

```java
try{

}
finally{

}
```

---

# Quick Memory

```text
try
 |
 risky code


catch
 |
 handle problem


finally
 |
 cleanup


throw
 |
 create exception


throws
 |
 declare exception
```

---

# Interview One-Liners

* Exception handling prevents abnormal program termination.
* All exceptions inherit from Throwable.
* Checked exceptions are verified at compile time.
* Unchecked exceptions occur at runtime.
* `throw` creates an exception.
* `throws` delegates exception handling.
* Custom exceptions represent application-specific failures.
* Try-with-resources automatically closes resources.
