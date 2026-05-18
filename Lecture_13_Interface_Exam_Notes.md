# Lecture 13: Interface - Exam Notes

> 范围：`Lecture/13_Interface.pdf`，主要是 Java interface、abstract methods、constants、`implements`、multiple interfaces、interface as type、interface parameter、`Comparable`、`compareTo`。
> 读法：先看第 0 节速查，再重点看第 2 节 implements 规则、第 4 节 interface as type、第 5-6 节 `Comparable`。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Interface 必背

| 问题 | 答案 |
|---|---|
| interface 是什么？ | collection of abstract methods and constants |
| interface 能否有 instance variables？ | no |
| abstract method 是什么？ | method header without method body |
| interface 中 methods 默认是什么 visibility？ | public by default |
| interface method 有没有 body？ | no body，header 后直接 `;` |
| interface 能否 instantiate？ | cannot instantiate |
| class 如何使用 interface？ | `implements` |
| class implements interface 后必须做什么？ | implement all abstract methods |
| class 能否 implement multiple interfaces？ | yes |
| multiple interfaces syntax | `class ManyThings implements Interface1, Interface2` |
| interface 能否作为 type name？ | yes，用来声明 reference |
| interface reference 能指向什么？ | object whose class implements that interface |
| interface parameter 好处 | one method accepts objects of different classes with same behavior |

### Comparable 必背

| 问题 | 答案 |
|---|---|
| `Comparable` 是什么？ | Java standard library interface |
| 它的核心 method | `int compareTo(Object other)` |
| `obj1.compareTo(obj2) < 0` | `obj1` less than `obj2` |
| `obj1.compareTo(obj2) == 0` | equal |
| `obj1.compareTo(obj2) > 0` | `obj1` greater than `obj2` |
| `String` 是否 implements Comparable？ | yes |
| custom class 能否 implements Comparable？ | yes |
| `Comparable` parameter 好处 | same method can compare different comparable object types |

---

## 1. What Is an Interface?

A Java interface is a collection of：

1. abstract methods。
2. constants。

Lecture key point：

```text
no instance variables
```

An abstract method is a method header without method body。

Example：

```java
public interface Doable {
    public void doThis();
    public int doThat();
    public void doThis2(float value, char ch);
    public boolean doTheOther(int num);
}
```

Important syntax：

- `interface` is reserved word。
- method headers end with semicolon。
- no method body `{ ... }` in interface method declaration。

### 1.1 Why interface?

An interface establishes a set of methods that a class will implement。

Intuitive idea：

```text
An object has a set of behaviors.
```

Interface describes behavior requirements。

For example：

```text
Anything that implements Hello must know how to sayHello().
Anything that implements Comparable must know how to compareTo(...).
```

---

## 2. Implementing an Interface

### 2.1 `implements`

A class formally implements an interface by：

1. stating so in class header。
2. providing implementations for every abstract method in the interface。

Example：

```java
public class CanDo implements Doable {
    public void doThis() {
        // implementation
    }

    public int doThat() {
        // implementation
        return 0;
    }

    public void doThis2(float value, char ch) {
        // implementation
    }

    public boolean doTheOther(int num) {
        // implementation
        return true;
    }
}
```

Key rule：

```text
If a class asserts that it implements an interface, it must define all methods in the interface.
```

If one method is missing, compilation fails unless the class is abstract。

### 2.2 Interface cannot be instantiated

Wrong：

```java
Doable d = new Doable(); // cannot instantiate interface
```

Correct：

```java
Doable d = new CanDo();
```

Because `CanDo` is a class that implements `Doable`。

### 2.3 Methods are public by default

Lecture point：

```text
Methods in an interface have public visibility by default.
```

When implementing them in a class, methods must be public。

Wrong pattern：

```java
class CanDo implements Doable {
    void doThis() { } // weaker visibility, wrong
}
```

Correct：

```java
class CanDo implements Doable {
    public void doThis() { }
}
```

---

## 3. Hello Interface Example

### 3.1 Scenario

All languages have the phrase "Hello"，but they are spelled differently。

Classes：

- `Chinese`
- `English`
- possibly `Japanese`

Interface：

- `Hello`

Different classes can have different instance variables, but all provide same behavior：

```java
sayHello()
```

### 3.2 Interface

```java
public interface Hello {
    public void sayHello();
}
```

### 3.3 English class

```java
public class English implements Hello {
    private String lastname, firstname, middlename;

    public English(String lastname, String firstname, String middlename) {
        this.lastname = lastname;
        this.firstname = firstname;
        this.middlename = middlename;
    }

    public void sayHello() {
        System.out.println(firstname + " " + middlename + " " + lastname + " says Hello.");
    }
}
```

### 3.4 Chinese class

```java
public class Chinese implements Hello {
    private String familyname, givenname;

    public Chinese(String familyname, String givenname) {
        this.familyname = familyname;
        this.givenname = givenname;
    }

    public void sayHello() {
        System.out.println(familyname + " " + givenname + " says Ni Hao.");
    }
}
```

### 3.5 Driver using concrete types

```java
public class HelloDriver {
    public static void main(String[] args) {
        Chinese c1, c2;
        English e1;

        c1 = new Chinese("Ying", "Zheng");
        c2 = new Chinese("Liu", "Bang");
        e1 = new English("Trump", "Donald", "John");

        c1.sayHello();
        c2.sayHello();
        e1.sayHello();
    }
}
```

Output idea：

```text
Ying Zheng says Ni Hao.
Liu Bang says Ni Hao.
Donald John Trump says Hello.
```

The exact names may vary by slide, but behavior pattern is same。

---

## 4. Multiple Interfaces

A class can implement multiple interfaces。

Syntax：

```java
class ManyThings implements Interface1, Interface2 {
    // all methods of both interfaces
}
```

Rule：

```text
The class must implement all methods in all interfaces listed in the header.
```

This is different from class inheritance：

- a class can extend only one class。
- a class can implement multiple interfaces。

---

## 5. Using Interface as a Type Name

### 5.1 Interface reference

Although an interface cannot be instantiated, it can be used as a type to declare a reference。

Example：

```java
Hello c1 = new Chinese("Ying", "Zheng");
```

This is valid because `Chinese implements Hello`。

Meaning：

```text
c1 is a Hello reference pointing to a Chinese object.
```

### 5.2 Driver using interface type

```java
public class HelloDriver {
    public static void main(String[] args) {
        Hello c1, c2;
        Hello e1;

        c1 = new Chinese("Ying", "Zheng");
        c2 = new Chinese("Liu", "Bang");
        e1 = new English("Obama", "Barack", "Hussein");

        c1.sayHello();
        c2.sayHello();
        e1.sayHello();
    }
}
```

Why this works：

- `c1`, `c2`, `e1` are declared as `Hello` references。
- each object implements `Hello`。
- compiler knows every `Hello` has `sayHello()`。

### 5.3 What methods can be called through interface reference?

If variable type is `Hello`：

```java
Hello person = new Chinese("Ying", "Zheng");
```

You can call methods declared in `Hello`：

```java
person.sayHello();
```

You cannot call methods only defined in `Chinese` unless you cast or declare concrete type。

Exam idea：

```text
Reference type determines what methods are directly accessible at compile time.
Object type determines which implementation runs.
```

---

## 6. Interface Reference as Parameter

Lecture example：

```java
public class HelloDriver {
    public static void invoke_method(Hello person) {
        person.sayHello();
    }

    public static void main(String[] args) {
        Chinese c1, c2;
        English e1;

        c1 = new Chinese("Ying", "Zheng");
        c2 = new Chinese("Liu", "Bang");
        e1 = new English("Obama", "Barack", "Hussein");

        invoke_method(c1);
        invoke_method(c2);
        invoke_method(e1);
    }
}
```

Why useful？

```text
We can define a method that takes objects of different classes but similar behavior.
```

Here:

- `Chinese` and `English` are different classes。
- both implement `Hello`。
- therefore both can be passed to a method expecting `Hello`。

This is a key polymorphism-like idea before Lecture 15。

---

## 7. The `Comparable` Interface

### 7.1 Definition

The Java standard class library contains many helpful interfaces。

`Comparable` contains one abstract method：

```java
public interface Comparable {
    int compareTo(Object other);
}
```

The `String` class implements `Comparable`，which gives strings lexicographic ordering。

### 7.2 `compareTo` return values

For:

```java
obj1.compareTo(obj2)
```

| Return | Meaning |
|---:|---|
| negative | `obj1` is less than `obj2` |
| `0` | `obj1` and `obj2` are equal |
| positive | `obj1` is greater than `obj2` |

This matches Lecture 11 string comparison。

### 7.3 Any class can implement `Comparable`

If a class has a natural ordering, it can implement `Comparable`。

The class must define:

```java
public int compareTo(Object other)
```

---

## 8. `Salary implements Comparable`

Lecture example：

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

### 8.1 Trace idea

Comparison is based on:

```java
basic + bonus
```

If this object's total salary is smaller：

```java
return -1;
```

If larger：

```java
return 1;
```

If same：

```java
return 0;
```

### 8.2 Cast

```java
Salary salary = (Salary) other;
```

Because parameter type is `Object` in the lecture version of `Comparable`。

The method needs to treat `other` as a `Salary` to access `basic` and `bonus`。

---

## 9. `Fraction implements Comparable`

Lecture example idea：

```java
public class Fraction implements Comparable {
    private int numerator, denominator;

    public String toString() {
        return Integer.toString(numerator) + "/" + Integer.toString(denominator);
    }

    public int compareTo(Object other) {
        Fraction f = (Fraction) other;

        if (Math.abs(f.value() - value()) < 0.00001) return 0;
        if (f.value() > value()) return -1;
        return 1;
    }
}
```

Comparison uses numeric fraction value。

Important：

- uses tolerance for floating-point comparison。
- returns `0` if values essentially equal。
- returns `-1` if `this` fraction is smaller。
- returns `1` if `this` fraction is larger。

This connects Lecture 11 float comparison with Lecture 13 `Comparable`。

---

## 10. `Comparable` as Parameter

### 10.1 Big idea

`Comparable` can be used as a type in parameter list。

Then a single method can work for different types of comparable objects。

Lecture example：

```java
public class ComparableDemo {
    public static Comparable largest(Comparable c1, Comparable c2, Comparable c3) {
        if (c1.compareTo(c2) > 0 && c1.compareTo(c3) > 0) return c1;
        if (c2.compareTo(c1) > 0 && c2.compareTo(c3) > 0) return c2;
        return c3;
    }

    public static void main(String[] args) {
        int n1 = 1, n2 = 8, n3 = 6;
        System.out.println("The largest among n1-n3 is " + largest(n1, n2, n3));

        String s1 = "apple", s2 = "bye", s3 = "ELEC2543";
        System.out.println("The largest among s1-s3 is " + largest(s1, s2, s3));

        Fraction f1 = new Fraction(1, 2);
        Fraction f2 = new Fraction(2, 3);
        Fraction f3 = new Fraction(1, 3);
        System.out.println("The largest among f1-f3 is " + largest(f1, f2, f3));
    }
}
```

### 10.2 Why can `largest(int, int, int)` compile?

Question from lecture：

```text
Method largest accepts three object references as parameters. Why can we call largest(int, int, int)?
```

Answer：

```text
Because of autoboxing. The int values are automatically boxed into Integer objects, and Integer implements Comparable.
```

So:

```java
largest(n1, n2, n3)
```

becomes conceptually：

```java
largest(Integer.valueOf(n1), Integer.valueOf(n2), Integer.valueOf(n3))
```

### 10.3 Largest among Strings

For:

```java
String s1 = "apple", s2 = "bye", s3 = "ELEC2543";
```

`String.compareTo` uses lexicographic ordering。

Remember Lecture 11：

```text
uppercase letters come before lowercase letters
```

So `"ELEC2543"` may come before lowercase strings even though it visually starts with E。

Between `"apple"` and `"bye"`：

```text
"bye" is larger than "apple"
```

The largest is likely:

```text
bye
```

### 10.4 Largest among Fractions

For:

```java
Fraction f1 = new Fraction(1, 2); // 0.5
Fraction f2 = new Fraction(2, 3); // 0.666...
Fraction f3 = new Fraction(1, 3); // 0.333...
```

Largest:

```text
2/3
```

because `Fraction.compareTo` compares numeric values。

---

## 11. Interface vs Class Inheritance

| Concept | Interface | Class inheritance |
|---|---|---|
| Keyword | `implements` | `extends` |
| Main purpose | require behaviors | reuse/extend class implementation |
| Instantiate directly? | no | normal class yes, abstract class no |
| Methods | abstract headers in lecture | implemented methods possible |
| Instance variables | no | yes |
| Multiple allowed? | class can implement many interfaces | class extends one class |
| Relationship | can-do behavior | is-a relationship |

This table connects Lecture 13 with Lecture 14。

---

## 12. 考试答题模板

### 12.1 问：what is an interface?

```text
An interface is a collection of abstract methods and constants. It establishes a set of methods that implementing classes must provide. An interface cannot be instantiated and has no instance variables.
```

### 12.2 问：what must a class do when it implements an interface?

```text
The class must state the interface in its implements clause and provide implementations for all abstract methods declared in the interface.
```

### 12.3 问：why use interface as parameter type?

```text
Using an interface as a parameter type allows one method to accept objects of different classes as long as they implement the same interface and therefore provide the required behavior.
```

### 12.4 问：what is `Comparable`?

```text
Comparable is a standard Java interface with compareTo. A class implements Comparable to define how its objects can be ordered. compareTo returns a negative integer, zero, or a positive integer depending on whether the receiver is less than, equal to, or greater than the argument.
```

---

## 13. 最容易错的点

1. Interface cannot be instantiated。
2. Interface methods have no body in the lecture version。
3. Interface methods are public by default。
4. Implementing class must implement all methods。
5. Class can implement multiple interfaces。
6. Interface can be used as a reference type。
7. Interface reference can only directly call methods declared in interface。
8. `Comparable.compareTo` returns negative / zero / positive。
9. `String` implements `Comparable`。
10. `Integer` implements `Comparable`，so autoboxed `int` can be passed to `Comparable` parameter。
11. `compareTo(Object other)` often needs a cast to the real class。

---

## 14. 最后 30 秒记忆版

```text
interface = abstract methods + constants
no instance variables
cannot instantiate

class C implements I:
must implement all methods in I

Interface as type:
Hello h = new Chinese(...);
method(Hello person) can accept Chinese, English, etc.

Comparable:
int compareTo(Object other)
<0 this smaller
0 equal
>0 this larger

largest(int,int,int) works because int autoboxes to Integer,
and Integer implements Comparable.
```
