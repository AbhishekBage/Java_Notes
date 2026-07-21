Java Access Modifiers – Quick Revision Notes

What are Access Modifiers?

Access modifiers control the visibility (accessibility) of classes, methods, variables, and constructors.

Java provides 4 access modifiers:

- "public"
- "protected"
- default (package-private)
- "private"

---

Access Levels

Modifier| Same Class| Same Package| Subclass (Different Package)| Different Package
"public"| ✅| ✅| ✅| ✅
"protected"| ✅| ✅| ✅| ❌
default (package-private)| ✅| ✅| ❌| ❌
"private"| ✅| ❌| ❌| ❌

---

1. public

Accessible from anywhere.

Example:

public class Employee {

    public void display() {
        System.out.println("Hello");
    }
}

Use when the member should be available to all classes.

---

2. protected

Accessible:

- Within the same package.
- In subclasses, even if they are in a different package.

Example:

protected int salary;

Commonly used in inheritance.

---

3. Default (Package-Private)

If no access modifier is specified, Java uses package-private access.

Example:

class Student {

    void show() {
    }
}

Accessible only within the same package.

---

4. private

Accessible only inside the same class.

Example:

private String password;

Used for data hiding and encapsulation.

Access is usually provided through getter and setter methods.

---

Access Modifiers on Classes

Top-level classes can only be:

- "public"
- package-private (default)

Not allowed:

- "private"
- "protected"

Example:

public class Demo {
}

or

class Demo {
}

---

Access Modifiers on Methods & Variables

Methods, variables, and constructors can use all four modifiers:

- "public"
- "protected"
- default
- "private"

---

Encapsulation

Encapsulation means hiding data and exposing it through controlled methods.

Example:

class Employee {

    private int salary;

    public int getSalary() {
        return salary;
    }

    public void setSalary(int salary) {
        this.salary = salary;
    }
}

Benefits:

- Data security
- Better maintainability
- Controlled access

---

Interview Questions

Which access modifier is the most restrictive?

"private"

---

Which access modifier is the least restrictive?

"public"

---

Can a top-level class be private?

No.

Only nested (inner) classes can be "private".

---

What is the default access modifier?

Package-private (default).

Accessible only within the same package.

---

Why use private variables?

To achieve encapsulation and prevent direct access.

---

Difference between protected and default?

- protected: Accessible in the same package and in subclasses outside the package.
- default: Accessible only within the same package.

---

Quick Revision

public
↓
Accessible everywhere

protected
↓
Same package + subclasses

default
↓
Same package only

private
↓
Same class only

Visibility Order:

public
   ↓
protected
   ↓
default
   ↓
private

---

One-Line Summary

- public → Accessible from anywhere.
- protected → Same package + subclasses.
- default → Same package only.
- private → Same class only.
