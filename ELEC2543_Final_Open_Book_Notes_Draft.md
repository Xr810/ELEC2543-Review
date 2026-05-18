# ELEC2543 Final Open-Book Notes Draft

> Status: editable draft for printing and later revision.  
> Scope: Past Paper 2025 + Chapters 13-21 review notes, organized into 10 Parts.  
> Goal: keep all exam-relevant material in one place for fast open-book lookup.

---

## 0. How To Use This File

- Use **Past Paper Quick Lookup** before doing practice questions.
- Use each **Part** as the full explanation section.
- Use code blocks for syntax and output tracing.
- Before final printing, re-check any section marked **Needs source check** against the original paper or slides.

---

## 1. Overall Exam Map

### Past Paper 2025 Coverage

| Question | Main Topic | What To Know |
|---|---|---|
| Q1 | Java MCQ basics | JVM, data types, `final`, `super`, `String`, exception types |
| Q2a | Array + method parameter passing | reference behavior and mutation |
| Q2b/c | `try-catch-finally` | exact execution order and output |
| Q2d | `Integer` wrapper cache | `==` vs `equals` |
| Q3 | Binary Search Tree | insertion, inorder, preorder, postorder |
| Q4 | AVL Tree | height, operation complexity, rotations |
| Q5 | Merge Sort + Quick Sort | step tracing and complexity |
| Q6 | Inheritance + Interface | `extends`, `implements`, abstract class vs interface |

### 10-Part Review Plan

| Part | Topic | Exam Importance |
|---|---|---|
| Part 1 | Interface: definition, `implements`, `Comparable` | High |
| Part 2 | Inheritance: `extends`, `protected`, `super`, abstract class | Very High |
| Part 3 | Polymorphism: late binding, interface/inheritance polymorphism | High |
| Part 4 | Exceptions: `try-catch-finally`, propagation, checked vs unchecked | Very High |
| Part 5 | Recursion: base case, recursive case, Towers of Hanoi | Medium |
| Part 6 | Sorting I: Bubble, Insertion, Selection Sort + Big-O | Very High |
| Part 7 | Sorting II: Merge Sort + Quick Sort | Very High |
| Part 8 | Data Structures: Linked List, Stack, Queue | High |
| Part 9 | Trees & BST | Very High |
| Part 10 | Advanced Trees: AVL Tree + Heap | Very High |

---

## 2. Past Paper Quick Lookup

| Question | Topic | Key Answer |
|---|---|---|
| Q1(i) | JVM | Java Virtual Machine |
| Q1(ii) | Boolean default value | `false` |
| Q1(iii) | Java does not support | Multiple inheritance by classes |
| Q1(iv) | `System` / `String` package | `java.lang` |
| Q1(v) | Non-existent exception type | Compile-time exception |
| Q1(vi) | `final` keyword | All of the above |
| Q1(vii) | No `main` method | Runtime error |
| Q1(viii) | `super` keyword | Refers to direct parent class |
| Q1(ix) | `String.concat()` immutability | original string unchanged, e.g. `blue` |
| Q1(x) | String + number concatenation | `Result: 1020` |
| Q2b | `try-catch-finally` with exception | `abcde` |
| Q2c | `try-catch-finally` without exception | `abde` |
| Q2d | `Integer` cache | small cached values may compare `==` true; larger values compare by reference |
| Q3b | BST traversal giving sorted order | Inorder traversal |
| Q3c | BST preorder | `60, 41, 16, 25, 53, 46, 55, 74, 65, 63, 70` |
| Q3d | BST postorder | `25, 16, 46, 55, 53, 41, 63, 70, 65, 74, 60` |
| Q4a | AVL minimum height with `n` nodes | `floor(log2 n)` |
| Q4b | AVL operation complexity | Search / Insert / Delete all `O(log n)` |
| Q5c | Merge Sort complexity | Average = Worst = `O(n log n)` |
| Q5d | Quick Sort complexity | Average = `O(n log n)`, Worst = `O(n^2)` |

---

# Part 1 / 10: Interface

## 1.1 What Is An Interface?

An **interface** defines a set of behavior requirements. It tells a class what methods it must implement, but it normally does not provide the method bodies.

Think of an interface as a **contract**:

- Interface = contract / job description.
- `implements` = a class accepts the contract.
- A class implementing the interface must implement all required methods.

```java
public interface Doable {
    public void doThis();
    public int doThat();
    public boolean doTheOther(int num);
}
```

Important points:

- Use `interface`, not `class`.
- Methods are implicitly `public` and `abstract`.
- Method headers end with `;`, not `{}`.
- Interfaces cannot be instantiated directly.
- Traditional interfaces do not contain instance variables. They may contain constants.

## 1.2 Implementing An Interface

Use `implements`:

```java
public class CanDo implements Doable {
    public void doThis() {
        System.out.println("Doing this!");
    }

    public int doThat() {
        return 42;
    }

    public boolean doTheOther(int num) {
        return num > 0;
    }
}
```

If the class does not implement all interface methods, it will not compile unless the class is abstract.

## 1.3 Interface As A Type

An interface cannot be instantiated, but it can be used as a reference type.

```java
public interface Hello {
    public void sayHello();
}

public class Chinese implements Hello {
    private String familyname, givenname;

    public Chinese(String fn, String gn) {
        this.familyname = fn;
        this.givenname = gn;
    }

    public void sayHello() {
        System.out.println(familyname + " " + givenname + " says Ni Hao.");
    }
}

public class English implements Hello {
    private String lastname, firstname;

    public English(String ln, String fn) {
        this.lastname = ln;
        this.firstname = fn;
    }

    public void sayHello() {
        System.out.println(firstname + " " + lastname + " says Hello.");
    }
}
```

```java
Hello c1 = new Chinese("Ying", "Zheng");
Hello e1 = new English("Obama", "Barack");

c1.sayHello();
e1.sayHello();
```

This is a form of **polymorphism**: one interface type can refer to different implementation objects.

## 1.4 Interface As A Parameter

An interface can be used as a method parameter type.

```java
public static void invoke_method(Hello person) {
    person.sayHello();
}

invoke_method(new Chinese("Liu", "Bang"));
invoke_method(new English("Obama", "Barack"));
```

Benefit: one method can handle many object types as long as they implement the same interface.

## 1.5 Implementing Multiple Interfaces

Java does not support multiple inheritance by classes, but one class can implement multiple interfaces.

```java
class ManyThings implements Interface1, Interface2, Interface3 {
    // implement all methods from all interfaces
}
```

Past Paper-style example:

```java
public interface Animal {
    public void eat();
}

public interface Wings {
    public void fly();
}

public class Eagle implements Animal, Wings {
    public void eat() {
        System.out.println("The eagle is eating");
    }

    public void fly() {
        System.out.println("The eagle is flying");
    }
}
```

## 1.6 `Comparable`

`Comparable` is a standard Java interface for defining comparison logic.

```java
public interface Comparable {
    int compareTo(Object other);
}
```

Return value rule:

| Return Value | Meaning |
|---|---|
| Negative | `this < other` |
| `0` | `this == other` |
| Positive | `this > other` |

Example:

```java
public class Salary implements Comparable {
    private int basic;
    private int bonus;

    public int compareTo(Object other) {
        Salary salary = (Salary) other;
        int totalOfThis = basic + bonus;
        int totalOfOther = salary.basic + salary.bonus;

        if (totalOfThis < totalOfOther) return -1;
        if (totalOfThis > totalOfOther) return 1;
        return 0;
    }
}
```

Generic-looking method using `Comparable`:

```java
public static Comparable largest(Comparable c1, Comparable c2, Comparable c3) {
    if (c1.compareTo(c2) > 0 && c1.compareTo(c3) > 0) return c1;
    if (c2.compareTo(c1) > 0 && c2.compareTo(c3) > 0) return c2;
    return c3;
}
```

Examples:

```java
largest(1, 8, 6);                      // 8
largest("apple", "bye", "ELEC2543");   // "bye" by dictionary order
```

## 1.7 Interface Hierarchy

Interfaces can extend other interfaces.

```java
interface Add_Sub {
    public void add(double x, double y);
    public void subtract(double x, double y);
}

interface Mul_Div {
    public void multiply(double x, double y);
    public void divide(double x, double y);
}

interface Calculator extends Add_Sub, Mul_Div {
    public void printResult(double result);
}

public class MyCalculator implements Calculator {
    public void add(double x, double y)      { printResult(x + y); }
    public void subtract(double x, double y) { printResult(x - y); }
    public void multiply(double x, double y) { printResult(x * y); }
    public void divide(double x, double y)   { printResult(x / y); }

    public void printResult(double result) {
        System.out.println("The result is : " + result);
    }
}
```

## 1.8 Summary

- Interface defines required behavior.
- A class uses `implements` to implement an interface.
- A class must implement all interface methods.
- Interface cannot be instantiated, but can be used as variable type and parameter type.
- A class can implement multiple interfaces.
- `Comparable` defines `compareTo`.
- Interfaces can extend other interfaces.

---

# Part 2 / 10: Inheritance

## 2.1 What Is Inheritance?

Inheritance lets a new class derive from an existing class. The child class automatically receives the parent class's accessible methods and variables.

Terminology:

| English | Synonyms | Chinese |
|---|---|---|
| parent class | superclass / base class | 父类 |
| child class | subclass / derived class | 子类 |

Inheritance should represent an **is-a** relationship.

Example:

```text
Vehicle
   ^
   |
  Car
```

Car is a Vehicle.

## 2.2 `extends`

```java
public class Book {
    protected int pages = 1500;

    public void setPages(int numPages) {
        pages = numPages;
    }

    public int getPages() {
        return pages;
    }
}

public class Dictionary extends Book {
    private int definitions = 52500;

    public double computeRatio() {
        return (double) definitions / pages;
    }

    public void setDefinitions(int n) {
        definitions = n;
    }

    public int getDefinitions() {
        return definitions;
    }
}
```

Usage:

```java
Dictionary webster = new Dictionary();
System.out.println(webster.getPages());
System.out.println(webster.getDefinitions());
System.out.println(webster.computeRatio());
```

`Dictionary` can call `getPages()` because it inherits it from `Book`.

## 2.3 `protected`

| Modifier | Same Class | Subclass | Other Classes |
|---|---:|---:|---:|
| `private` | yes | no | no |
| `protected` | yes | yes | no, except same package |
| `public` | yes | yes | yes |

Important:

- `private` members exist in the object, but subclasses cannot access them directly.
- `protected` members are directly accessible in subclasses.
- In UML, `#` means protected and `+` means public.

```text
Book
# pages : int
+ setPages() : void
+ getPages() : int
```

## 2.4 `super`

### Use 1: Calling Parent Constructor

`super(...)` must be the first line of the child constructor.

```java
public class Book2 {
    protected int pages;

    public Book2(int numPages) {
        pages = numPages;
    }
}

public class Dictionary2 extends Book2 {
    private int definitions;

    public Dictionary2(int numPages, int numDefinitions) {
        super(numPages);
        definitions = numDefinitions;
    }
}
```

### Use 2: Calling Parent Method

```java
public class Advice extends Thought {
    public void message() {
        System.out.println("Warning: Dates in calendar are closer than they appear.");
        super.message();
    }
}
```

## 2.5 Method Overriding

Overriding means a subclass redefines an inherited method with the same name and same parameter list.

```java
public class Thought {
    public void message() {
        System.out.println("I feel like I'm diagonally parked in a parallel universe.");
    }
}

public class Advice extends Thought {
    public void message() {
        System.out.println("Warning: Dates in calendar are closer than they appear.");
        System.out.println();
        super.message();
    }
}
```

## 2.6 Overloading vs Overriding

| | Overloading | Overriding |
|---|---|---|
| Where | Same class | Parent-child classes |
| Method name | Same | Same |
| Parameter list | Different | Same |
| Purpose | Same operation with different parameters | Subclass behavior replaces parent behavior |

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }
}
```

```java
public class Animal {
    public void sound() {
        System.out.println("...");
    }
}

public class Dog extends Animal {
    public void sound() {
        System.out.println("Woof!");
    }
}
```

Rules:

- `final` method cannot be overridden.
- `final` class cannot be extended.
- Constructors are not inherited, so they cannot be overridden.
- A child class can define a variable with the same name as parent class variable, but this is shadowing and is usually poor style.

## 2.7 Class Hierarchy

```text
        HKUPerson
        /      \
   Student    Staff
```

- A subclass may also become a superclass.
- Subclasses of the same parent are siblings.
- A subclass inherits accessible members from all ancestor classes.

## 2.8 `Object` Class

All Java classes directly or indirectly extend `Object`.

Common inherited methods:

| Method | Purpose |
|---|---|
| `String toString()` | returns string representation |
| `boolean equals(Object obj)` | default compares references |
| `Object clone()` | creates copy |

When writing `toString()`, you are overriding `Object.toString()`.

## 2.9 Abstract Class

An abstract class cannot be instantiated. It represents a general concept.

```java
public abstract class HKUPerson {
    protected String name, hkid;

    public HKUPerson(String name, String hkid) {
        this.name = name;
        this.hkid = hkid;
    }

    public String toString() {
        return name + " " + hkid;
    }

    public abstract int charge(int base);
}
```

Subclasses must implement all abstract methods, or they must also be declared abstract.

```java
public class Student extends HKUPerson {
    private String studID;

    public Student(String name, String hkid, String studID) {
        super(name, hkid);
        this.studID = studID;
    }

    public String toString() {
        return super.toString() + " " + studID;
    }

    public int charge(int base) {
        return base * 2;
    }
}

public class Staff extends HKUPerson {
    private String staffID;

    public Staff(String name, String hkid, String staffID) {
        super(name, hkid);
        this.staffID = staffID;
    }

    public String toString() {
        return super.toString() + " " + staffID;
    }

    public int charge(int base) {
        return base * 3;
    }
}
```

### Abstract Class vs Interface

| | Abstract Class | Interface |
|---|---|---|
| Keyword | `abstract class` | `interface` |
| Non-abstract methods | yes | traditionally no |
| Instance variables | yes | no |
| Relationship keyword | `extends` | `implements` |
| Multiple inheritance | no | a class can implement multiple interfaces |
| Instantiable | no | no |
| Purpose | shared base implementation | behavior contract |

## 2.10 Visibility And Indirect Access

Subclasses cannot directly access parent `private` members, but can access them indirectly through public/protected methods.

```java
public class FoodItem {
    private int fatGrams;
    protected int servings;

    private int calories() {
        return fatGrams * 9;
    }

    public int caloriesPerServing() {
        return calories() / servings;
    }
}
```

## 2.11 Summary

- Use `extends` for inheritance.
- Inheritance should represent an is-a relationship.
- `protected` allows subclass access while limiting outside access.
- `super(...)` calls parent constructor and must be first in child constructor.
- `super.method()` calls parent method.
- Overriding: same signature in subclass.
- Overloading: same method name, different parameters.
- Abstract classes cannot be instantiated.
- All Java classes inherit from `Object`.

---

# Part 3 / 10: Polymorphism

## 3.1 Polymorphic Reference

A polymorphic reference can refer to different object types at different times.

```java
Holiday day;
day = new Christmas();
```

Parent class reference can point to child class object.

## 3.2 Late Binding

Binding means connecting a method call to the actual method implementation.

| Type | Time |
|---|---|
| Early binding | compile time |
| Late binding / dynamic binding | runtime |

Java uses late binding for overridden methods. The method version called depends on the object's actual runtime type, not the reference's declared type.

```java
Holiday day = new Christmas();
day.celebrate();  // calls Christmas celebrate() if overridden
```

Important compile-time rule:

```java
Holiday day = new Christmas();
day.getTree();  // compile error if Holiday does not define getTree()
```

To call child-specific method:

```java
((Christmas) day).getTree();
```

## 3.3 Polymorphism Through Inheritance

Class hierarchy:

```text
        StaffMember
        /        \
 Volunteer     Employee
               /      \
        Executive    Hourly
```

```java
abstract public class StaffMember {
    protected String name, address, phone;

    public StaffMember(String name, String address, String phone) {
        this.name = name;
        this.address = address;
        this.phone = phone;
    }

    public String toString() {
        return "Name: " + name + "\nAddress: " + address + "\nPhone: " + phone;
    }

    public abstract double pay();
}
```

```java
public class Volunteer extends StaffMember {
    public Volunteer(String name, String address, String phone) {
        super(name, address, phone);
    }

    public double pay() {
        return 0.0;
    }
}
```

```java
public class Employee extends StaffMember {
    protected String socialSecurityNumber;
    protected double payRate;

    public Employee(String name, String address, String phone, String ssn, double rate) {
        super(name, address, phone);
        socialSecurityNumber = ssn;
        payRate = rate;
    }

    public double pay() {
        return payRate;
    }
}
```

```java
public class Executive extends Employee {
    private double bonus;

    public Executive(String name, String address, String phone, String ssn, double rate) {
        super(name, address, phone, ssn, rate);
        bonus = 0;
    }

    public void awardBonus(double execBonus) {
        bonus = execBonus;
    }

    public double pay() {
        double payment = super.pay() + bonus;
        bonus = 0;
        return payment;
    }
}
```

```java
public class Hourly extends Employee {
    private int hoursWorked;

    public Hourly(String name, String address, String phone, String ssn, double rate) {
        super(name, address, phone, ssn, rate);
        hoursWorked = 0;
    }

    public void addHours(int moreHours) {
        hoursWorked += moreHours;
    }

    public double pay() {
        double payment = payRate * hoursWorked;
        hoursWorked = 0;
        return payment;
    }
}
```

Polymorphic array:

```java
public class Staff {
    private StaffMember[] staffList;

    public Staff() {
        staffList = new StaffMember[6];
        staffList[0] = new Executive("Sam", "123 Main Line", "555-0469", "123-45-6789", 2423.07);
        staffList[1] = new Employee("Carla", "456 Off Line", "555-0101", "987-65-4321", 1246.15);
        staffList[3] = new Hourly("Diane", "678 Fifth Ave.", "555-0690", "958-47-3625", 10.55);
        staffList[4] = new Volunteer("Norm", "987 Suds Blvd.", "555-8374");

        ((Executive) staffList[0]).awardBonus(500.00);
        ((Hourly) staffList[3]).addHours(40);
    }

    public void payday() {
        for (int count = 0; count < staffList.length; count++) {
            double amount = staffList[count].pay();

            if (amount == 0.0)
                System.out.println("Thanks!");
            else
                System.out.println("Paid: " + amount);
        }
    }
}
```

`staffList[count].pay()` calls different `pay()` methods based on actual object type.

## 3.4 References And Inheritance

Upcasting: child to parent, safe and implicit.

```java
MusicPlayer mplayer = new CDPlayer();
```

Downcasting: parent to child, explicit and risky.

```java
CDPlayer cdplayer = (CDPlayer) mplayer;
```

Bad example:

```java
CDPlayer cdplayer = (CDPlayer) new MusicPlayer();  // may compile, runtime error
```

Reason: every CDPlayer is a MusicPlayer, but not every MusicPlayer is a CDPlayer.

## 3.5 Polymorphism Through Interfaces

```java
public interface Speaker {
    public void speak();
    public void announce(String str);
}

public class Philosopher implements Speaker {
    public void speak() {
        System.out.println("I think therefore I am.");
    }

    public void announce(String str) {
        System.out.println("Philosopher announces: " + str);
    }

    public void pontificate() {
        System.out.println("Pontificating...");
    }
}

public class Dog implements Speaker {
    public void speak() {
        System.out.println("Woof!");
    }

    public void announce(String str) {
        System.out.println("Dog barks: " + str);
    }
}
```

```java
Speaker guest = new Philosopher();
guest.speak();

guest = new Dog();
guest.speak();
```

Compile-time restriction:

```java
Speaker special = new Philosopher();
special.pontificate();  // error: Speaker does not define pontificate()
```

Quick Check:

```java
Speaker first = new Dog();              // legal
Philosopher second = new Philosopher(); // legal
second.pontificate();                   // legal
first = second;                         // legal
```

## 3.6 Polymorphism In Sorting

Use `Comparable[]` to sort different comparable object types.

```java
public static void selectionSort(Comparable[] list) {
    int min;
    Comparable temp;

    for (int index = 0; index < list.length - 1; index++) {
        min = index;

        for (int scan = index + 1; scan < list.length; scan++) {
            if (list[scan].compareTo(list[min]) < 0)
                min = scan;
        }

        temp = list[min];
        list[min] = list[index];
        list[index] = temp;
    }
}
```

`compareTo` call is polymorphic:

- `String[]` calls `String.compareTo`.
- `Contact[]` calls `Contact.compareTo`.
- `Salary[]` calls `Salary.compareTo`.

Example `Contact`:

```java
public class Contact implements Comparable {
    private String firstName, lastName, phone;

    public int compareTo(Object other) {
        String otherFirst = ((Contact) other).getFirstName();
        String otherLast = ((Contact) other).getLastName();

        if (lastName.equals(otherLast))
            return firstName.compareTo(otherFirst);
        else
            return lastName.compareTo(otherLast);
    }
}
```

## 3.7 Summary

- Polymorphism means one reference can point to many object types.
- Late binding decides overridden method at runtime.
- Compiler checks methods using declared reference type.
- Upcasting is safe; downcasting needs explicit cast and may fail.
- Polymorphism works through both inheritance and interfaces.
- `Comparable` + polymorphism enables generic sorting.

---

# Part 4 / 10: Exceptions

## 4.1 What Is An Exception?

An exception is an object describing an unusual or error condition.

Options when exception occurs:

- Ignore it: program terminates and prints stack trace.
- Handle it locally with `try-catch`.
- Let it propagate upward to caller.

## 4.2 Ignoring An Exception

```java
public class Zero {
    public static void main(String[] args) {
        int numerator = 10;
        int denominator = 0;

        System.out.println(numerator / denominator);
        System.out.println("This text will not be printed.");
    }
}
```

Output includes:

```text
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at Zero.main(Zero.java:17)
```

The second `println` does not execute.

## 4.3 `try-catch`

```java
try {
    zone = code.charAt(9);
    district = Integer.parseInt(code.substring(3, 7));
}
catch (StringIndexOutOfBoundsException exception) {
    System.out.println("Improper code length: " + code);
}
catch (NumberFormatException exception) {
    System.out.println("District is not numeric: " + code);
}
```

Rules:

- When exception occurs in `try`, execution immediately leaves the `try` block.
- Java executes the first matching `catch`.
- If no exception occurs, all `catch` blocks are skipped.

## 4.4 `finally`

`finally` always executes, whether or not an exception occurred.

```java
try {
    // risky code
}
catch (SomeException e) {
    // handle exception
}
finally {
    // always execute
}
```

Execution:

- With exception: partial `try` -> matching `catch` -> `finally` -> following code.
- Without exception: full `try` -> skip `catch` -> `finally` -> following code.

## 4.5 Past Paper Q2b/c

```java
public class Demo {
    public static void main(String args[]) {
        System.out.print("a");
        try {
            System.out.print("b");
            throw new IllegalArgumentException();
        }
        catch (RuntimeException e) {
            System.out.print("c");
        }
        finally {
            System.out.print("d");
        }
        System.out.print("e");
    }
}
```

With `throw`:

```text
a -> b -> exception -> catch RuntimeException -> c -> finally d -> e
```

Output:

```text
abcde
```

Reason: `IllegalArgumentException` is a subclass of `RuntimeException`.

If `throw` line is commented out:

```text
a -> b -> no exception -> skip catch -> finally d -> e
```

Output:

```text
abde
```

## 4.6 Exception Propagation

If a method does not handle an exception, the exception propagates up the call chain.

```java
public void level3() {
    int result = 10 / 0;
}

public void level2() {
    level3();
}

public void level1() {
    try {
        level2();
    }
    catch (ArithmeticException e) {
        System.out.println("Caught: " + e.getMessage());
    }
}
```

After exception occurs in `level3`, statements after the exception in `level3` and `level2` do not execute.

## 4.7 Exception Class Hierarchy

```text
Throwable
├── Error
└── Exception
    ├── RuntimeException
    │   ├── ArithmeticException
    │   ├── NullPointerException
    │   ├── IndexOutOfBoundsException
    │   │   └── StringIndexOutOfBoundsException
    │   ├── IllegalArgumentException
    │   └── NumberFormatException
    └── Other checked exceptions
        ├── IOException
        ├── ClassNotFoundException
        └── NoSuchMethodException
```

## 4.8 Checked vs Unchecked Exceptions

| | Checked Exception | Unchecked Exception |
|---|---|---|
| Definition | `Exception` subclass but not `RuntimeException` subclass | `RuntimeException` and its subclasses |
| Compiler requirement | must catch or declare with `throws` | not forced by compiler |
| Examples | `IOException`, `ClassNotFoundException`, `NoSuchMethodException` | `NullPointerException`, `ArithmeticException`, `IndexOutOfBoundsException` |

Past Paper Q1(v):

- Checked exception: exists.
- Unchecked exception: exists.
- Runtime exception: exists.
- Compile-time exception: does not exist.

## 4.9 `throw`

Use `throw` to manually throw an exception.

```java
public class OutOfRangeException extends Exception {
    OutOfRangeException(String message) {
        super(message);
    }
}

public static void main(String[] args) throws OutOfRangeException {
    final int MIN = 25, MAX = 40;
    int value = scan.nextInt();

    if (value < MIN || value > MAX)
        throw new OutOfRangeException("Input value is out of range.");

    System.out.println("End of main method.");
}
```

Code immediately after `throw` in the same path is unreachable.

## 4.10 I/O And `IOException`

File I/O may throw checked exceptions.

```java
import java.io.*;

public static void main(String[] args) throws IOException {
    PrintWriter outFile = new PrintWriter("test.txt");
    outFile.println("Hello, file!");
    outFile.close();
}
```

Standard streams:

- `System.out`: standard output.
- `System.in`: standard input.
- `System.err`: standard error output.

## 4.11 Summary

- `try` contains risky code.
- `catch` handles matching exception.
- `finally` always executes.
- Checked exceptions must be caught or declared.
- Unchecked exceptions are `RuntimeException` subclasses.
- Exception propagation passes unhandled exceptions up the call chain.
- Q2b output: `abcde`.
- Q2c output: `abde`.

---

# Part 5 / 10: Recursion

## 5.1 What Is Recursion?

Recursion means defining or solving something in terms of itself.

Example recursive list definition:

```text
A List is:
  a number
  or a number comma List
```

Example:

```text
24, 88, 40, 37
= 24, (88, (40, (37)))
```

## 5.2 Two Required Parts

Every recursive method needs:

| Part | Meaning |
|---|---|
| Base case | stopping condition; no recursive call |
| Recursive case | calls itself, moving closer to base case |

Missing base case leads to infinite recursion and stack overflow.

## 5.3 Factorial

Definition:

```text
1! = 1
N! = N * (N - 1)!
```

```java
public int factorial(int n) {
    if (n == 1)
        return 1;
    else
        return n * factorial(n - 1);
}
```

Trace:

```text
factorial(5)
= 5 * factorial(4)
= 5 * 4 * factorial(3)
= 5 * 4 * 3 * factorial(2)
= 5 * 4 * 3 * 2 * factorial(1)
= 5 * 4 * 3 * 2 * 1
= 120
```

Each recursive call creates its own execution environment.

## 5.4 Sum From 1 To N

```text
sum(1) = 1
sum(N) = N + sum(N - 1)
```

```java
public int sum(int num) {
    int result;

    if (num == 1)
        result = 1;
    else
        result = num + sum(num - 1);

    return result;
}
```

Trace:

```text
sum(4)
= 4 + sum(3)
= 4 + 3 + sum(2)
= 4 + 3 + 2 + sum(1)
= 10
```

Important: recursion is not always better. For simple sums, iteration may be clearer.

## 5.5 Direct vs Indirect Recursion

Direct recursion:

```java
public void methodA() {
    methodA();
}
```

Indirect recursion:

```text
m1() -> m2() -> m3() -> m1()
```

Both require a base case.

## 5.6 Maze Traversal

Recursive search with backtracking:

```java
public boolean traverse(int row, int column) {
    if (!valid(row, column))
        return false;

    if (row == grid.length - 1 && column == grid[row].length - 1) {
        grid[row][column] = PATH;
        return true;
    }

    grid[row][column] = TRIED;

    boolean found = traverse(row + 1, column) ||
                    traverse(row - 1, column) ||
                    traverse(row, column + 1) ||
                    traverse(row, column - 1);

    if (found)
        grid[row][column] = PATH;

    return found;
}
```

## 5.7 Towers Of Hanoi

Rules:

- 3 pegs.
- N disks start on peg 1.
- Move all disks to peg 3.
- Move one disk at a time.
- Larger disk cannot be placed on smaller disk.

Recursive idea:

1. Move top `N - 1` disks from start to temp.
2. Move largest disk from start to end.
3. Move `N - 1` disks from temp to end.

```java
private void moveTower(int numDisks, int start, int end, int temp) {
    if (numDisks == 1)
        moveOneDisk(start, end);
    else {
        moveTower(numDisks - 1, start, temp, end);
        moveOneDisk(start, end);
        moveTower(numDisks - 1, temp, end, start);
    }
}

private void moveOneDisk(int start, int end) {
    System.out.println("Move one disk from " + start + " to " + end);
}
```

For N disks:

```text
number of moves = 2^N - 1
time complexity = O(2^N)
```

## 5.8 Summary

- Recursion = method calls itself.
- Must have base case and recursive case.
- Missing base case causes infinite recursion / stack overflow.
- Factorial, sum, maze traversal, and Towers of Hanoi are key examples.
- Towers of Hanoi takes `2^N - 1` moves.
- Recursion is best for naturally recursive problems.

---

# Part 6 / 10: Sorting I - Simple Sorting Algorithms + Big-O

## 6.1 Sorting Basics

Sorting means arranging elements in a chosen order, usually ascending.

Bubble Sort, Insertion Sort, and Selection Sort:

- use repeated passes.
- make the array more sorted each pass.
- have time complexity `O(n^2)`.

## 6.2 Bubble Sort

Strategy: compare adjacent elements and swap them, so small elements bubble upward.

Example first pass on `[2, 4, 6, 1, 3, 5]`, scanning from bottom:

```text
[2, 4, 6, 1, 3, 5]
compare 3,5 -> no swap
compare 1,3 -> no swap
compare 6,1 -> swap -> [2, 4, 1, 6, 3, 5]
compare 4,1 -> swap -> [2, 1, 4, 6, 3, 5]
compare 2,1 -> swap -> [1, 2, 4, 6, 3, 5]
```

Code:

```java
public static void bubbleSort(int[] list) {
    for (int i = 1; i < list.length; i++) {
        for (int j = list.length - 1; j >= i; j--) {
            if (list[j - 1] > list[j]) {
                int temp = list[j - 1];
                list[j - 1] = list[j];
                list[j] = temp;
            }
        }
    }
}
```

## 6.3 Insertion Sort

Strategy: maintain a sorted left region and insert the next element into the correct position.

Example:

```text
Initial: [2] | [4, 6, 1, 3, 5]
Insert 4 -> [2, 4] | [6, 1, 3, 5]
Insert 6 -> [2, 4, 6] | [1, 3, 5]
Insert 1 -> [1, 2, 4, 6] | [3, 5]
Insert 3 -> [1, 2, 3, 4, 6] | [5]
Insert 5 -> [1, 2, 3, 4, 5, 6]
```

Code:

```java
public static void insertionSort(int[] list) {
    for (int i = 1; i < list.length; i++) {
        int key = list[i];
        int j = i;

        while (j > 0 && list[j - 1] > key) {
            list[j] = list[j - 1];
            j--;
        }

        list[j] = key;
    }
}
```

## 6.4 Selection Sort

Strategy: find the smallest value in the unsorted region and swap it into the first unsorted position.

Example:

```text
[2, 4, 6, 1, 3, 5]
round 1: min 1, swap with 2 -> [1, 4, 6, 2, 3, 5]
round 2: min 2, swap with 4 -> [1, 2, 6, 4, 3, 5]
round 3: min 3, swap with 6 -> [1, 2, 3, 4, 6, 5]
round 4: min 4, already correct
round 5: min 5, swap with 6 -> [1, 2, 3, 4, 5, 6]
```

Code:

```java
public static void selectionSort(int[] list) {
    for (int i = 0; i < list.length - 1; i++) {
        int minimum = i;

        for (int j = i + 1; j < list.length; j++) {
            if (list[j] < list[minimum])
                minimum = j;
        }

        int temp = list[i];
        list[i] = list[minimum];
        list[minimum] = temp;
    }
}
```

## 6.5 Comparison

| Algorithm | Main Idea | Result Of Each Pass | Complexity |
|---|---|---|---|
| Bubble Sort | adjacent swaps | smallest reaches correct position | `O(n^2)` |
| Insertion Sort | insert into sorted region | sorted region grows | `O(n^2)` |
| Selection Sort | select minimum | one element fixed | `O(n^2)` |

## 6.6 Running Time Analysis

Common cases:

- Worst case: maximum operations.
- Average case: typical operations.
- Best case: minimum operations.

## 6.7 Big-O

Big-O describes growth rate as input size `n` grows. Ignore constants and lower-order terms.

```text
3n^3 + 90n^2 - 2n + 5 = O(n^3)
```

Formal idea:

```text
f(n) = O(g(n))
if there exist constants c and n0 such that
for all n >= n0, f(n) <= c * g(n)
```

Common order, fastest to slowest:

| Big-O | Name | Example |
|---|---|---|
| `O(1)` | constant | array random access |
| `O(log n)` | logarithmic | binary search |
| `O(n)` | linear | scanning array |
| `O(n log n)` | linearithmic | Merge Sort, average Quick Sort |
| `O(n^2)` | quadratic | Bubble, Insertion, Selection |
| `O(2^n)` | exponential | Towers of Hanoi |

Example:

```text
2n^2 = O(n^2)  correct and tight
2n^2 = O(n^3)  correct but not tight
2n^2 = O(n)    incorrect
```

Usually use the tightest bound.

## 6.8 Why Simple Sorts Are `O(n^2)`

Bubble Sort comparison count:

```text
(n - 1) + (n - 2) + ... + 2 + 1
= n(n - 1) / 2
= 0.5n^2 - 0.5n
= O(n^2)
```

Insertion Sort and Selection Sort have similar quadratic behavior.

## 6.9 Summary

- Bubble Sort: adjacent swaps, `O(n^2)`.
- Insertion Sort: insert into sorted left region, `O(n^2)`.
- Selection Sort: find minimum and swap, `O(n^2)`.
- Big-O focuses on growth trend.
- Complexity order: `O(1) < O(log n) < O(n) < O(n log n) < O(n^2) < O(2^n)`.

---

# Part 7 / 10: Sorting II - Merge Sort + Quick Sort

## 7.1 Divide And Conquer

Both Merge Sort and Quick Sort use divide-and-conquer:

- Divide: split problem.
- Conquer: solve subproblems.
- Combine: combine subproblem results.

---

## 7A. Merge Sort

## 7.2 Merge Sort Idea

```text
Split array into two halves
Sort left half recursively
Sort right half recursively
Merge two sorted halves
```

Key point: merging two sorted arrays is `O(n)`.

## 7.3 Past Paper Array: `[2, 6, 5, 7, 9, 8, 3, 4]`

Divide:

```text
[2, 6, 5, 7, 9, 8, 3, 4]
-> [2, 6, 5, 7]    [9, 8, 3, 4]
-> [2, 6] [5, 7]   [9, 8] [3, 4]
-> [2] [6] [5] [7] [9] [8] [3] [4]
```

Merge level 1:

```text
[2] + [6] -> [2, 6]
[5] + [7] -> [5, 7]
[9] + [8] -> [8, 9]
[3] + [4] -> [3, 4]
```

Merge level 2:

```text
[2, 6] + [5, 7]
2 vs 5 -> 2
6 vs 5 -> 5
6 vs 7 -> 6
remaining 7
=> [2, 5, 6, 7]

[8, 9] + [3, 4]
8 vs 3 -> 3
8 vs 4 -> 4
remaining 8, 9
=> [3, 4, 8, 9]
```

Final merge:

```text
[2, 5, 6, 7] + [3, 4, 8, 9]
2 vs 3 -> 2
5 vs 3 -> 3
5 vs 4 -> 4
5 vs 8 -> 5
6 vs 8 -> 6
7 vs 8 -> 7
remaining 8, 9
=> [2, 3, 4, 5, 6, 7, 8, 9]
```

## 7.4 Merge Sort Code

```java
public static void mergeSort(int[] list) {
    if (list.length <= 1) return;

    int[] first = new int[list.length / 2];
    int[] second = new int[list.length - first.length];

    for (int i = 0; i < first.length; i++)
        first[i] = list[i];

    for (int i = 0; i < second.length; i++)
        second[i] = list[first.length + i];

    mergeSort(first);
    mergeSort(second);

    int iFirst = 0, iSecond = 0, i = 0;

    while (iFirst < first.length && iSecond < second.length) {
        if (first[iFirst] <= second[iSecond])
            list[i++] = first[iFirst++];
        else
            list[i++] = second[iSecond++];
    }

    while (iFirst < first.length)
        list[i++] = first[iFirst++];

    while (iSecond < second.length)
        list[i++] = second[iSecond++];
}
```

## 7.5 Merge Sort Complexity

Recurrence:

```text
T(1) = c
T(n) = 2T(n/2) + kn
```

Expansion:

```text
T(n) = 2[kn/2 + 2T(n/4)] + kn
     = 2kn + 4T(n/4)
     = 3kn + 8T(n/8)
     = i*kn + 2^i*T(n/2^i)
```

When `n / 2^i = 1`, `i = log2 n`.

```text
T(n) = kn log n + cn
     = O(n log n)
```

| Case | Complexity |
|---|---|
| Best | `O(n log n)` |
| Average | `O(n log n)` |
| Worst | `O(n log n)` |

Merge Sort is stable in time complexity.

---

## 7B. Quick Sort

## 7.6 Quick Sort Idea

```text
Choose pivot
Partition:
  left side <= pivot
  right side > pivot
Put pivot in final correct position
Recursively sort both sides
```

Key point: after partitioning, the pivot is permanently in its correct position.

## 7.7 Past Paper Array With First Element As Pivot

Initial:

```text
[2, 6, 5, 7, 9, 8, 3, 4]
```

First partition, pivot = `2`:

```text
No value <= 2 on right side except pivot itself.
Result: [2] | [6, 5, 7, 9, 8, 3, 4]
```

Partition right side, pivot = `6`:

```text
[6, 5, 7, 9, 8, 3, 4]

left pointer stops at 7
right pointer stops at 4
swap 7 and 4:
[6, 5, 4, 9, 8, 3, 7]

left pointer stops at 9
right pointer stops at 3
swap 9 and 3:
[6, 5, 4, 3, 8, 9, 7]

left pointer stops at 8
right pointer crosses left
swap pivot 6 with right pointer value 3:
[3, 5, 4, 6, 8, 9, 7]
```

Now:

```text
[2] [3, 5, 4] [6] [8, 9, 7]
```

Continue recursively:

```text
[2] [3] [4, 5] [6] [7] [8, 9]
=> [2, 3, 4, 5, 6, 7, 8, 9]
```

## 7.8 Quick Sort Code

```java
public static void quickSort(int[] list) {
    qsort(list, 0, list.length - 1);
}

private static void qsort(int[] list, int i, int j) {
    if (i >= j) return;

    if (i + 1 == j) {
        if (list[i] > list[j]) swap(list, i, j);
        return;
    }

    int pos = findPivot(list, i, j);

    if (pos != i) qsort(list, i, pos - 1);
    if (pos != j) qsort(list, pos + 1, j);
}

private static int findPivot(int[] list, int i, int j) {
    int pivot = list[i];
    int left = i + 1, right = j;

    while (left < right) {
        while (left <= j && list[left] <= pivot) left++;
        while (right > i && list[right] > pivot) right--;

        if (left < right) {
            swap(list, left, right);
            left++;
            right--;
        }
    }

    if (list[right] <= pivot) {
        swap(list, i, right);
        return right;
    }

    return i;
}
```

## 7.9 Quick Sort Complexity

Best / average case:

```text
pivot splits array roughly in half
number of levels = log2 n
work per level = n
total = O(n log n)
```

Worst case:

```text
pivot is always smallest or largest
n + (n - 1) + (n - 2) + ... + 1
= O(n^2)
```

Common worst case: already sorted array while always choosing first element as pivot.

## 7.10 Merge Sort vs Quick Sort

| | Merge Sort | Quick Sort |
|---|---|---|
| Strategy | split then merge | partition around pivot |
| Pivot | no | yes |
| Average | `O(n log n)` | `O(n log n)` |
| Worst | `O(n log n)` | `O(n^2)` |
| Extra space | `O(n)` | often in-place |
| Stability | stable | not stable |
| Practical note | consistent but extra memory | often fast, memory efficient |

## 7.11 Summary

- Merge Sort: divide, recursively sort, merge.
- Quick Sort: choose pivot, partition, recursively sort.
- Merge Sort average and worst: `O(n log n)`.
- Quick Sort average: `O(n log n)`, worst: `O(n^2)`.
- Quick Sort worst case can happen when data is sorted and pivot is first element.

---

# Part 8 / 10: Data Structures - Linked List, Stack, Queue

## 8.1 Static vs Dynamic Data Structures

| | Static | Dynamic |
|---|---|---|
| Size | fixed at compile time | can grow/shrink at runtime |
| Examples | array | linked list, stack, queue, tree |
| Strength | simple, efficient | flexible |
| Cost | inflexible size | reference overhead |

Here, "static" is about data structure size, not Java's `static` keyword.

## 8.2 Abstract Data Type

An ADT packages:

- data organization.
- operations on the data.
- hidden implementation details.

Java collection ADTs include:

- `List`
- `Stack`
- `Queue`
- `Set`
- `Map`

ADT users care about what operations are available, not how they are implemented.

## 8.3 Object References As Links

```java
class Node {
    int info;
    Node next;
}
```

`next` stores a reference to another `Node`.

## 8.4 Linked List

Singly linked list:

```text
head -> [data|next] -> [data|next] -> [data|null]
```

Doubly linked list:

```text
null <- [prev|data|next] <-> [prev|data|next] <-> [prev|data|next] -> null
```

## 8.5 Linked List Operations

Insert `newNode` after `current`:

```java
newNode.next = current.next;
current.next = newNode;
```

Order matters. If `current.next = newNode` happens first, the original next node may be lost.

Delete node after `current`:

```java
if (current.next != null)
    current.next = current.next.next;
```

Doubly linked list deletion:

```java
if (current.next != null) {
    current.next = current.next.next;
    if (current.next != null)
        current.next.prev = current;
}
```

## 8.6 MagazineList Example

```java
public class MagazineList {
    private MagazineNode list;

    public MagazineList() {
        list = null;
    }

    public void add(Magazine mag) {
        MagazineNode node = new MagazineNode(mag);

        if (list == null) {
            list = node;
        }
        else {
            MagazineNode current = list;

            while (current.next != null)
                current = current.next;

            current.next = node;
        }
    }

    private class MagazineNode {
        public Magazine magazine;
        public MagazineNode next;

        public MagazineNode(Magazine mag) {
            magazine = mag;
            next = null;
        }
    }
}
```

This uses an inner class for list nodes.

## 8.7 Java `LinkedList`

```java
LinkedList<String> list = new LinkedList<String>();

list.add("A");
list.add(1, "B");

list.set(0, "Z");

list.remove("A");
list.remove(0);

for (String s : list)
    System.out.println(s);
```

## 8.8 Queue

Queue is FIFO: First-In, First-Out.

Analogy: bank line. First person in line is served first.

Operations:

| Operation | Meaning |
|---|---|
| `enqueue(item)` | add item to rear |
| `dequeue()` | remove and return item from front |
| `empty()` | check whether queue is empty |

Exercise:

```text
enqueue(45), enqueue(12), enqueue(28), dequeue(), dequeue(), enqueue(69), enqueue(27)

enqueue(45) -> [45]
enqueue(12) -> [45, 12]
enqueue(28) -> [45, 12, 28]
dequeue()   -> remove 45 -> [12, 28]
dequeue()   -> remove 12 -> [28]
enqueue(69) -> [28, 69]
enqueue(27) -> [28, 69, 27]

final head -> tail: 28, 69, 27
```

Linked-list implementation:

```java
public class MyQueue {
    private MyNode front, rear;

    public void enqueue(Object item) {
        MyNode newNode = new MyNode(item);

        if (empty()) {
            front = rear = newNode;
        }
        else {
            rear.next = newNode;
            rear = newNode;
        }
    }

    public Object dequeue() {
        if (empty()) return null;

        Object obj = front.obj;

        if (front == rear)
            front = rear = null;
        else
            front = front.next;

        return obj;
    }
}
```

ArrayList implementation:

```java
public class MyQueue {
    private ArrayList<Object> queue = new ArrayList<Object>();

    public void enqueue(Object item) {
        queue.add(item);
    }

    public Object dequeue() {
        if (queue.isEmpty()) return null;
        return queue.remove(0);
    }
}
```

## 8.9 Stack

Stack is LIFO: Last-In, First-Out.

Analogy: stack of plates. Last plate placed on top is removed first.

Operations:

| Operation | Meaning |
|---|---|
| `push(item)` | add item to top |
| `pop()` | remove and return top |
| `peek()` | view top without removing |
| `empty()` | check whether stack is empty |

Exercise:

```text
push(45), push(12), push(28), pop(), pop(), push(69), push(27), push(99)

push(45) -> [45]
push(12) -> [45, 12]
push(28) -> [45, 12, 28]
pop()    -> remove 28 -> [45, 12]
pop()    -> remove 12 -> [45]
push(69) -> [45, 69]
push(27) -> [45, 69, 27]
push(99) -> [45, 69, 27, 99]

final bottom -> top: 45, 69, 27, 99
```

String reverse example:

```java
Stack word = new Stack();
```

Input:

```text
artxE eseehc esaelp
```

Output:

```text
Extra cheese please
```

Java provides `java.util.Stack` with `push`, `pop`, `peek`, `empty`.

## 8.10 Graph

Graph:

- nodes connected by edges.
- no required root.
- arbitrary connections.

Directed graph / digraph:

- edges have direction.
- directed edges are also called arcs.

Undirected graph:

- edges have no direction.

Tree is a special graph: rooted, hierarchical, and without cycles.

## 8.11 Java Collections And Generics

```java
LinkedList<Book> myList = new LinkedList<Book>();
ArrayList<String> myArray = new ArrayList<String>();
Stack<Integer> myStack = new Stack<Integer>();
```

Generics:

```java
LinkedList<Book> list = new LinkedList<Book>();
list.add(new Book("Java"));
Book b = list.get(0);
```

Benefit: type safety and no cast needed when retrieving.

## 8.12 Summary

- Dynamic structures use references as links and can change size at runtime.
- Linked list nodes contain data and next reference.
- Insert: set `newNode.next` first, then update `current.next`.
- Queue: FIFO, `enqueue` at rear, `dequeue` at front.
- Stack: LIFO, `push` and `pop` at top.
- Queue exercise final: `28, 69, 27`.
- Stack exercise final: `45, 69, 27, 99`.
- Java Collections include `LinkedList`, `ArrayList`, `Stack`, etc.

---

# Part 9 / 10: Trees & BST

## 9.1 Tree Terminology

Example:

```text
            10
           /  \
          6    15
         / \   /
        2   8 13
         \   \
          5   11
```

| Term | Meaning |
|---|---|
| root | top node, no parent |
| leaf | node with no children |
| internal node | node with one or two children |
| parent / child | directly connected upper/lower nodes |
| siblings | nodes with same parent |
| subtree | a node and all its descendants |
| ancestor | nodes on path from root to a node |
| descendant | nodes below a node |
| height of tree | number of edges from root to farthest leaf |

## 9.2 Binary Tree

A binary tree is a tree where each node has at most two children: left child and right child.

## 9.3 Tree Traversal

| Traversal | Order | Memory Tip |
|---|---|---|
| Preorder | Root -> Left -> Right | root first |
| Inorder | Left -> Root -> Right | root middle |
| Postorder | Left -> Right -> Root | root last |

Example tree:

```text
        10
       /  \
      6    15
     / \   /
    2   8 13
     \   \
      5   11
```

Preorder:

```text
10, 6, 2, 5, 8, 11, 15, 13
```

Inorder:

```text
2, 5, 6, 8, 11, 10, 13, 15
```

Postorder:

```text
5, 2, 11, 8, 6, 13, 15, 10
```

Note: for a valid BST, inorder traversal gives ascending order.

## 9.4 Binary Search Tree

BST property:

```text
For any node N:
all values in left subtree <= N
all values in right subtree > N
```

Therefore, inorder traversal of BST is sorted ascending.

## 9.5 BST Insertion

Rule:

```text
if new value <= current node value -> go left
if new value > current node value -> go right
if current position is null -> insert new node there
```

## 9.6 Past Paper Q3 BST Construction

Insert in order:

```text
60, 41, 74, 16, 53, 65, 25, 46, 55, 63, 70
```

Step logic:

```text
60 -> root
41 < 60 -> left of 60
74 > 60 -> right of 60
16 < 60, 16 < 41 -> left of 41
53 < 60, 53 > 41 -> right of 41
65 > 60, 65 < 74 -> left of 74
25 < 60, 25 < 41, 25 > 16 -> right of 16
46 < 60, 46 > 41, 46 < 53 -> left of 53
55 < 60, 55 > 41, 55 > 53 -> right of 53
63 > 60, 63 < 74, 63 < 65 -> left of 65
70 > 60, 70 < 74, 70 > 65 -> right of 65
```

Correct final BST:

```text
            60
           /  \
         41    74
        /  \   /
      16   53 65
        \  / \ / \
        25 46 55 63 70
```

Same tree with spacing:

```text
            60
          /    \
        41      74
       /  \     /
     16    53  65
       \   / \ /  \
       25 46 55 63 70
```

**Needs source check note:** the original text version previously placed `63` under `55`, but the insertion rule and traversal answers require `63` to be the left child of `65`.

## 9.7 Past Paper Q3 Traversal Answers

Q3b: traversal giving sorted order:

```text
Inorder traversal
```

Inorder:

```text
16, 25, 41, 46, 53, 55, 60, 63, 65, 70, 74
```

Preorder:

```text
60, 41, 16, 25, 53, 46, 55, 74, 65, 63, 70
```

Postorder:

```text
25, 16, 46, 55, 53, 41, 63, 70, 65, 74, 60
```

## 9.8 BST Deletion

Three cases:

1. Delete leaf node: remove directly.
2. Delete node with one child: replace deleted node with its child.
3. Delete node with two children: replace with smallest node in right subtree.

Find smallest value in right subtree:

```java
private int findSmallestValue(Node root) {
    if (root.left == null)
        return root.value;
    else
        return findSmallestValue(root.left);
}
```

## 9.9 BST Java Implementation

Insertion:

```java
public void insert(int value) {
    root = insertRec(root, value);
}

private Node insertRec(Node root, int value) {
    if (root == null)
        return new Node(value);

    if (value <= root.value)
        root.left = insertRec(root.left, value);
    else
        root.right = insertRec(root.right, value);

    return root;
}
```

Inorder traversal:

```java
public void inorder(Node root) {
    if (root == null) return;

    inorder(root.left);
    System.out.print(root.value + " ");
    inorder(root.right);
}
```

## 9.10 Summary

- BST left subtree values are `<=` node value.
- BST right subtree values are `>` node value.
- Insertion follows comparisons until reaching `null`.
- Inorder gives sorted order.
- Preorder: root-left-right.
- Postorder: left-right-root.
- Delete leaf directly; replace one-child node with child; replace two-child node with right subtree minimum.

---

# Part 10 / 10: Advanced Trees - AVL Tree + Heap

---

## 10A. AVL Tree

## 10.1 Why AVL Tree?

Regular BST can become a linked list if inserted data is sorted.

Example:

```text
insert 1, 2, 3, 4, 5

1
 \
  2
   \
    3
     \
      4
       \
        5
```

Search becomes `O(n)` instead of `O(log n)`.

AVL Tree keeps the tree balanced through rotations.

## 10.2 AVL Definition

AVL Tree is a self-balancing BST.

For every node:

```text
height difference between left subtree and right subtree <= 1
```

Height convention:

- empty tree height = `-1`.
- single node height = `0`.

Example not AVL:

```text
        5
       / \
      3   9
     / \
    1   4
     \
      2

node 5:
left subtree height = 2
right subtree height = 0
difference = 2 > 1
not AVL
```

Example AVL:

```text
        5
       / \
      3   9
     / \
    1   4

node 5 difference = 1
node 3 difference = 0
AVL
```

## 10.3 Past Paper Q4a: Minimum Height

For an AVL Tree with `n` nodes, minimum height:

```text
floor(log2 n)
```

Common values:

| Nodes `n` | Minimum Height |
|---|---|
| 1 | 0 |
| 2-3 | 1 |
| 4-7 | 2 |
| 8-15 | 3 |

For height `h`, maximum nodes:

```text
2^(h + 1) - 1
```

## 10.4 Past Paper Q4b: Operation Complexity

| Operation | Complexity |
|---|---|
| Search | `O(log n)` |
| Insert | `O(log n)` |
| Delete | `O(log n)` |

Reason: AVL height is always `O(log n)`.

## 10.5 AVL Rotations

After insertion, if a node becomes unbalanced, use rotations.

### Single Rotation

Use single rotation when imbalance is on the same side:

- left-left too high -> right rotation.
- right-right too high -> left rotation.

Right-right example, left rotation:

```text
before:              after:
    a                   b
     \                 / \
      b       ->      a   Z
     / \             / \
    Y   Z           X   Y
```

Concrete example:

```text
before:        after:
    5             7
     \           / \
      7    ->   5   8
     / \         \
    6   8         6
```

### Double Rotation

Use double rotation when imbalance is on opposite sides:

- left-right too high -> rotate left child left, then current node right.
- right-left too high -> rotate right child right, then current node left.

Memory rule:

```text
same side -> single rotation
opposite side -> double rotation
```

Table:

| Imbalance | Rotation |
|---|---|
| left-left | single |
| right-right | single |
| left-right | double |
| right-left | double |

Right-left example:

```text
step 1: rotate right child right

    8                  8
     \                  \
      9        ->        5
     /                    \
    5                      9

step 2: rotate current node left

    8                  5
     \                / \
      5      ->      8   9
       \
        9
```

## 10.6 Past Paper Q4c: Insert And Rotate

**Needs source check:** the provided notes for Q4c contain inconsistent original trees:

```text
        8
       / \
      2   9
     / \
    4   5
       /
      6
```

and later:

```text
    5
   / \
  3   7
 / \   \
1   4   8
 \
  2
```

The general exam method is:

1. Insert the new value using normal BST insertion.
2. Walk upward from inserted node.
3. Find first node whose left/right subtree height difference exceeds 1.
4. Determine imbalance type: LL, RR, LR, or RL.
5. Apply single or double rotation.
6. Redraw final balanced AVL tree.

If the inserted value is `70`, use the exact original Past Paper tree diagram to determine the final rotation.

Keep this rotation rule for the exam:

```text
LL -> right single rotation
RR -> left single rotation
LR -> left rotation on child, then right rotation on node
RL -> right rotation on child, then left rotation on node
```

---

## 10B. Heap

## 10.7 What Is A Heap?

A heap is a complete binary tree satisfying heap property.

Complete binary tree:

- every level except possibly the bottom is full.
- bottom level is filled from left to right.

Min-heap property:

```text
each node value <= each child value
```

Valid min-heap:

```text
        4
       / \
      5   7
     / \ /
    16 10 15
```

Invalid heap:

```text
        4
       / \
      15  7
     / \ /
    5  10 16
```

Root is always the minimum value.

## 10.8 Heap Operations

### Delete Minimum

Steps:

1. Copy last node value to root.
2. Delete last node.
3. Re-heapify downward by swapping with smaller child until heap property is restored.

Example:

```text
original:
    4
   / \
  5   7
 / \ /
16 10 15

replace root with last node 15:
    15
   /  \
  5    7
 / \
16 10

swap 15 with smaller child 5:
    5
   / \
  15  7
 / \
16 10

swap 15 with smaller child 10:
    5
   / \
  10  7
 / \
16 15
```

### Insert

Steps:

1. Add new node at bottom-leftmost available position.
2. Re-heapify upward by swapping with parent while new value is smaller.

Example insert `4` into heap `[5, 7, 15, 16, 10]`:

```text
add at end:
    5
   / \
  7   15
 / \ /
16 10 4

4 < 15, swap
4 < 5, swap

final:
    4
   / \
  7   5
 / \ /
16 10 15
```

## 10.9 Heap Sort

Idea:

1. Insert all numbers into heap: `O(n log n)`.
2. Delete minimum repeatedly: `O(n log n)`.
3. Deletion order is sorted ascending.

Total complexity:

```text
O(n log n)
```

## 10.10 Array Implementation Of Heap

Level-order storage:

```text
        4
       / \
      5   7
     / \ /
    16 10 15

array: {4, 5, 7, 16, 10, 15}
index:  0  1  2   3   4   5
```

For node at index `i`:

```text
left child index  = 2*i + 1
right child index = 2*i + 2
parent index      = (i - 1) / 2
```

## 10.11 Summary

AVL Tree:

- Self-balancing BST.
- Every node has left/right subtree height difference at most 1.
- Empty tree height = `-1`.
- Single node height = `0`.
- Minimum height with `n` nodes = `floor(log2 n)`.
- Search, insert, delete = `O(log n)`.
- Same-side imbalance -> single rotation.
- Opposite-side imbalance -> double rotation.

Heap:

- Complete binary tree + heap property.
- Min-heap root is minimum.
- Delete min: replace root with last node, then re-heapify down.
- Insert: add at end, then re-heapify up.
- Heap Sort = `O(n log n)`.
- Array indices: left `2i + 1`, right `2i + 2`, parent `(i - 1) / 2`.

---

# Final One-Page Formula Sheet

## Java OOP

```text
interface: behavior contract
implements: class implements interface
extends: class inherits class / interface extends interface
super(...): parent constructor, must be first line
super.method(): call parent method
overloading: same class, same method name, different parameters
overriding: child class, same method signature, different body
abstract class: cannot instantiate, can have variables and implemented methods
```

## Polymorphism

```text
declared type controls what methods compiler allows
actual runtime object controls which overridden method executes
upcasting: child -> parent, safe
downcasting: parent -> child, explicit cast, runtime risk
```

## Exceptions

```text
try with exception: try partial -> catch -> finally -> after
try without exception: try full -> skip catch -> finally -> after
checked: must catch or throws
unchecked: RuntimeException subclasses
```

Past Paper outputs:

```text
with throw: abcde
without throw: abde
```

## Sorting

| Algorithm | Average | Worst |
|---|---|---|
| Bubble | `O(n^2)` | `O(n^2)` |
| Insertion | `O(n^2)` | `O(n^2)` |
| Selection | `O(n^2)` | `O(n^2)` |
| Merge | `O(n log n)` | `O(n log n)` |
| Quick | `O(n log n)` | `O(n^2)` |
| Heap Sort | `O(n log n)` | `O(n log n)` |

## Trees

```text
Preorder  = Root -> Left -> Right
Inorder   = Left -> Root -> Right
Postorder = Left -> Right -> Root

BST:
left <= node < right
inorder gives ascending order

AVL:
balance factor <= 1
search/insert/delete = O(log n)
same side -> single rotation
opposite side -> double rotation

Heap:
complete binary tree
min-heap root is minimum
array: left=2i+1, right=2i+2, parent=(i-1)/2
```

