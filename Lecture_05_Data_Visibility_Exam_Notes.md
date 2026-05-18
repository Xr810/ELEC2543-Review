# Lecture 5: Data Visibility - Exam Notes

> 范围：`Lecture/05_Data_Visibility.pdf`，主要是 data scope、instance/local data、encapsulation、visibility modifiers、accessors/mutators、method declarations、parameters、driver program、Account example、constructors revisited。
> 读法：先看第 0 节速查，再重点看第 2 节 encapsulation、第 3 节 visibility、第 4 节 getters/setters、第 6 节 Account 例子。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Data Visibility 必背

| 问题 | 答案 |
|---|---|
| scope 是什么？ | data can be referenced 的 area |
| class level data scope | all methods in class |
| method local data scope | only inside that method |
| instance data | class-level variable；each object has own copy |
| local data | declared inside method；method finishes 后 destroyed |
| encapsulation 是什么？ | hide internal data/workings; clients use public services |
| 为什么不要 public instance variables？ | violates encapsulation；client can modify state directly |
| public method 也叫什么？ | service method |
| support method 应该 public 吗？ | 不应该，通常 private |
| accessor 是什么？ | returns current value, usually `getX()` |
| mutator 是什么？ | changes value, usually `setX(...)` |
| mutator 比 public variable 好在哪里？ | can validate/restrict changes |
| Java visibility modifiers | `public`, `protected`, `private` |
| default visibility | no modifier；same package access |
| constructor return type | none, not even `void` |
| driver program | contains `main`, drives/tests other classes |

### Visibility 速查

| Modifier | Meaning in this lecture |
|---|---|
| `public` | can be referenced anywhere |
| `private` | can be referenced only within that class |
| default | no visibility modifier；same package |
| `protected` | inheritance-related，Lecture 14 详细讲 |

---

## 1. Data Scope

### 1.1 Scope definition

`scope` 是：

```text
the area in a program in which data can be referenced
```

### 1.2 Class-level data

```java
public class Die {
    private final int MAX = 6;
    private int faceValue;

    public int roll() {
        faceValue = (int)(Math.random() * MAX) + 1;
        return faceValue;
    }
}
```

`MAX` 和 `faceValue` declared at class level。

They can be referenced by all methods in `Die`。

### 1.3 Local data

```java
public String toString() {
    String result = Integer.toString(faceValue);
    return result;
}
```

`result` is local data。

只能在 `toString()` 内使用。

### 1.4 Instance data

`faceValue` 是 instance data because each `Die` instance has own version。

如果：

```java
Die die1 = new Die();
Die die2 = new Die();
```

则：

```text
die1 has its own faceValue.
die2 has its own faceValue.
```

Objects share method definitions, but each object has its own data space。

---

## 2. Encapsulation

### 2.1 What is encapsulation?

From external view, object is an encapsulated entity。

It provides services：

```text
public methods form the interface to the object
```

Client can use services, but should not need to know internal implementation。

Black-box intuition：

```text
Client -> calls public methods -> object manages private data internally
```

### 2.2 Why encapsulation?

Advantages：

1. Protects object from unwanted access by clients。
2. Allows access to a level without revealing complex details below。
3. Reduces human errors。
4. Simplifies maintenance。
5. Makes application easier to understand。

### 2.3 Bad design: public data

If instance variables are public：

```java
public int speed;
public int fuel;
```

Client can do unrealistic operations：

```java
car.speed = -999;
car.fuel = 1000000;
```

This violates real-life constraints。

### 2.4 Good design: private data + public methods

Better：

```java
private int speed;

public void accelerate(int amount) {
    if (amount > 0) {
        speed += amount;
    }
}
```

Object controls valid state changes。

---

## 3. Visibility Modifiers

### 3.1 Modifier

Modifier is Java reserved word specifying characteristics of data/method。

Examples：

```java
public
private
protected
final
```

### 3.2 `public`

Public member can be referenced anywhere。

Good for:

- service methods。
- constants that need external access。

Bad for:

- instance variables that clients should not directly change。

### 3.3 `private`

Private member can be referenced only within that class。

Good for:

- instance variables。
- support/helper methods。

### 3.4 Default visibility

No visibility modifier：

```java
int value;
```

Default visibility means accessible by classes in same package。

This is not same as `private`。

### 3.5 `protected`

Lecture 5 only introduces it。

It involves inheritance and is discussed later。

For now remember：

```text
protected is more open than private, but less open than public.
```

### 3.6 Public variables usually wrong

课件重点：

```text
Public variables violate encapsulation.
Instance variables should not be declared public.
```

Exception:

```text
public constants are acceptable
```

Because `final` value cannot be changed。

Example：

```java
public static final int MAX_SIZE = 100;
```

### 3.7 Service method vs support method

| Method type | Meaning | Visibility |
|---|---|---|
| service method | provides object's services to clients | usually `public` |
| support method | helps service methods internally | usually `private` |

---

## 4. Accessors and Mutators

### 4.1 Accessor

Accessor returns current value of private variable。

Naming:

```text
getX
```

Example：

```java
public double getX() {
    return x;
}
```

### 4.2 Mutator

Mutator changes value of private variable。

Naming:

```text
setX
```

Example：

```java
public void setX(double newX) {
    x = newX;
}
```

### 4.3 Why mutator is better than public variable

Mutator can restrict invalid values。

For `Die`：

```java
public void setFaceValue(int value) {
    if (value >= 1 && value <= MAX) {
        faceValue = value;
    }
}
```

If `faceValue` were public，client could set：

```java
die.faceValue = 999;
```

Mutator prevents that。

### 4.4 Point exercise

Given：

```java
public class Point {
    private double x;
    private double y;
}
```

Getter methods：

```java
public double getX() {
    return x;
}

public double getY() {
    return y;
}
```

Copy constructor：

```java
public Point(Point p) {
    x = p.x;
    y = p.y;
}
```

Because same class can access private fields of another object of same class。

---

## 5. Method Declarations and Parameters

### 5.1 Method declaration

Method declaration specifies code executed when method is invoked。

When method is invoked：

1. flow of control jumps to method。
2. method body executes。
3. when complete, flow returns to call location。

### 5.2 Method header

Example：

```java
char calc(int num1, int num2, String message)
```

Parts：

| Part | Example |
|---|---|
| return type | `char` |
| method name | `calc` |
| parameter list | `int num1, int num2, String message` |

Formal parameters：

```text
names in method declaration/header
```

### 5.3 Method body

```java
char calc(int num1, int num2, String message) {
    int sum = num1 + num2;
    char result = message.charAt(sum);
    return result;
}
```

`sum` and `result` are local data。

Return expression must conform to return type。

### 5.4 Return statement

If method returns value：

```java
return expression;
```

If method does not return value：

```java
void
```

Example：

```java
public void setFaceValue(int value) {
    faceValue = value;
}
```

### 5.5 Parameters

When method is called：

```text
actual parameters are copied into formal parameters
```

Example：

```java
ch = obj.calc(25, count, "Hello");
```

Formal:

```text
num1, num2, message
```

Actual:

```text
25, count, "Hello"
```

Lecture 9 expands this with objects/arrays。

---

## 6. Account Example

### 6.1 Account state

`Account` state：

```java
private final double RATE = 0.035;
private long acctNumber;
private double balance;
private String name;
```

Meanings：

- `RATE` interest rate。
- `acctNumber` account number。
- `balance` current balance。
- `name` owner。

### 6.2 Account constructor

```java
public Account(String owner, long account, double initial) {
    name = owner;
    acctNumber = account;
    balance = initial;
}
```

Initializes owner, account number, initial balance。

### 6.3 Account service methods

Deposit：

```java
public double deposit(double amount) {
    balance = balance + amount;
    return balance;
}
```

Withdraw：

```java
public double withdraw(double amount, double fee) {
    balance = balance - amount - fee;
    return balance;
}
```

Add interest：

```java
public double addInterest() {
    balance += (balance * RATE);
    return balance;
}
```

Accessor：

```java
public double getBalance() {
    return balance;
}
```

### 6.4 `toString()` with formatting

```java
public String toString() {
    NumberFormat fmt = NumberFormat.getCurrencyInstance();
    return acctNumber + "\t" + name + "\t" + fmt.format(balance);
}
```

This formats balance as currency。

Requires：

```java
import java.text.NumberFormat;
```

### 6.5 Driver program

`Transactions` contains `main` and drives `Account` objects。

Driver program：

```text
drives the use of other classes and tests their services
```

Example：

```java
Account acct1 = new Account("Ted Murphy", 72354, 102.56);
acct1.deposit(25.85);
acct1.addInterest();
System.out.println(acct1);
```

### 6.6 Which Account object is updated?

Quick Check：

```java
Account myAcct = new Account(...);
myAcct.deposit(50);
```

`myAcct.deposit(50)` updates the `Account` object referenced by `myAcct`。

Method call is through a particular object reference。

---

## 7. Constructors Revisited

### 7.1 Constructor has no return type

Correct：

```java
public Account(String owner, long account, double initial) {
    ...
}
```

Wrong：

```java
public void Account(String owner, long account, double initial) {
    ...
}
```

The wrong version is regular method, not constructor。

### 7.2 Default constructor

If programmer defines no constructor：

```text
Java provides a default constructor that accepts no parameters.
```

But if programmer defines any constructor, default no-argument constructor is not automatically added in the usual way。

So if you need it, define it explicitly。

---

## 8. Designing Robust Methods

### 8.1 Validate parameters

Lecture notes Account improvements：

```text
withdraw should verify amount parameter is positive
```

Example：

```java
public double withdraw(double amount, double fee) {
    if (amount > 0 && fee >= 0 && amount + fee <= balance) {
        balance = balance - amount - fee;
    }
    return balance;
}
```

### 8.2 Encapsulation supports validation

If `balance` were public：

```java
acct.balance = -999;
```

No validation。

If `balance` private，all changes go through methods where class designer can enforce rules。

---

## 9. 考试答题模板

### 9.1 Explain encapsulation

```text
Encapsulation hides an object's internal data and implementation details.
Clients use public service methods.
Instance variables should be private so the object controls valid state changes.
```

### 9.2 Visibility choice

```text
Instance variables -> private.
Service methods -> public.
Support methods -> private.
Constants that must be shared -> public final / public static final.
```

### 9.3 Write restricted setter

```java
public void setFaceValue(int value) {
    if (value >= 1 && value <= MAX) {
        faceValue = value;
    }
}
```

### 9.4 Method header explanation

For:

```java
char calc(int num1, int num2, String message)
```

Answer：

```text
return type: char
method name: calc
formal parameters: int num1, int num2, String message
```

---

## 10. 最容易错的点

### 10.1 Public instance variables violate encapsulation

Avoid：

```java
public double balance;
```

Prefer：

```java
private double balance;
public double getBalance() { return balance; }
```

### 10.2 Public methods are not always good

Support/helper methods should not be public if clients should not call them。

### 10.3 Constructor with `void`

```java
public void Account(...) { }
```

is not constructor。

### 10.4 Local variable cannot be used outside method

```java
public String toString() {
    String result = "...";
}

// result not visible here
```

### 10.5 Formal parameter is local

Formal parameters exist only during method invocation。

They are local variables of the method。

---

## 11. 最后 30 秒记忆版

```text
Scope = where data can be referenced.
Class-level data can be used by all methods in class.
Method local data can be used only in that method.

Instance data = each object has own copy.
Local data = created when method runs, destroyed when it finishes.

Encapsulation:
hide data, expose services.
Instance variables should be private.
Public variables violate encapsulation.

public: accessible anywhere.
private: only inside class.
default: same package.
protected: inheritance-related.

Accessor/getter returns value.
Mutator/setter changes value and can validate.

Method header = return type + name + parameter list.
Formal parameters are in method header.
Actual parameters are in method call.

Constructor has no return type, not even void.
Driver program contains main and tests/uses other classes.
```
