# Lecture 4: Creating Objects - Exam Notes

> 范围：`Lecture/04_Object_Creation.pdf`，主要是 object reference、`new`、instantiation、primitive assignment vs reference assignment、aliases、garbage collection、`this` reference。
> 读法：先看第 0 节速查，再重点看第 3 节 assignment、第 4 节 aliases、第 6 节 `this`。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Object Creation 必背

| 问题 | 答案 |
|---|---|
| variable 可以 hold 什么？ | primitive value 或 object reference |
| `Die die1;` 会不会创建 object？ | 不会，只声明 reference variable |
| object 一般用什么创建？ | `new` operator |
| `new Die(5)` 做什么？ | creates object and calls constructor |
| creating object 叫什么？ | instantiation |
| object 是什么？ | instance of a class |
| primitive variable 存什么？ | value itself |
| object reference variable 存什么？ | address/reference to object |
| primitive assignment copies 什么？ | value |
| reference assignment copies 什么？ | address/reference |
| aliases 是什么？ | two or more references refer to same object |
| alias 改 object 会怎样？ | all aliases see the change because only one object exists |
| garbage 是什么？ | object with no valid references |
| Java garbage collection | automatic periodically |
| `this` 是什么？ | current object reference |
| `this` 常见用途 | distinguish instance variables from parameters with same names |

---

## 1. Creating Objects

### 1.1 Primitive variable vs object reference variable

```java
int num;
Die die1;
```

这两行都只是 declarations。

状态：

```text
num: uninitialized primitive variable
die1: uninitialized object reference variable
```

重点：

```text
No object is created with Die die1;
```

### 1.2 Object reference holds address

Object reference variable 存的是 object 的 address / reference。

可以把它想成 pointer：

```text
die1 ----> Die object
```

但声明 reference 不等于已经指向 object。

### 1.3 `new` operator

创建 object：

```java
die1 = new Die(5);
```

`new` 做两件事：

1. allocate/create a new object。
2. call constructor to set up object。

Syntax：

```java
new <classname>([parameters])
```

例子：

```java
Die die1 = new Die();
Die die2 = new Die(2);
```

### 1.4 Instantiation

Creating an object is called：

```text
instantiation
```

Object is：

```text
an instance of a particular class
```

所以：

```java
Die die1 = new Die();
```

means：

```text
die1 refers to an instance of class Die.
```

---

## 2. Primitive Values and References in Memory

### 2.1 Primitive stores value itself

```java
int num = 42;
```

Memory idea：

```text
num contains 42
```

`num` 不是指向另一个 object；value 在 variable 自己里面。

### 2.2 Object reference stores address

```java
Die die1 = new Die();
```

Memory idea：

```text
die1 ----> Die object with faceValue = 1
```

`die1` 本身不 contain `faceValue`。

它 contain address/reference to the object。

### 2.3 Instance variable belongs to object

In:

```java
Die die1 = new Die();
Die die2 = new Die(2);
```

There are two different Die objects：

```text
die1 -> object A: faceValue = 1
die2 -> object B: faceValue = 2
```

Each object has own instance variable `faceValue`。

---

## 3. Assignment Revisited

### 3.1 Primitive assignment copies value

```java
int num1 = 38;
int num2 = 96;
num2 = num1;
```

Before：

```text
num1 = 38
num2 = 96
```

After：

```text
num1 = 38
num2 = 38
```

`num1` and `num2` are independent variables。

If later：

```java
num1 = 100;
```

then：

```text
num2 stays 38
```

### 3.2 Reference assignment copies address

```java
die2 = die1;
```

This copies reference/address。

Before：

```text
die1 -> object A: faceValue = 1
die2 -> object B: faceValue = 2
```

After：

```text
die1 -> object A
die2 -> object A
object B has no reference
```

Now `die1` and `die2` are aliases。

### 3.3 Key difference

| Assignment type | What is copied? | Effect |
|---|---|---|
| primitive assignment | value | two independent primitive values |
| reference assignment | address/reference | two references may point to same object |

---

## 4. Aliases

### 4.1 Definition

`aliases`：

```text
Two or more references that refer to the same object.
```

Example：

```java
MyInteger obj1 = new MyInteger(100);
MyInteger obj2 = new MyInteger(200);
obj2 = obj1;
```

After `obj2 = obj1`：

```text
obj1 and obj2 refer to same MyInteger object.
```

### 4.2 Alias effect

If：

```java
obj1.changeValue(300);
System.out.println(obj1);
System.out.println(obj2);
```

Both print：

```text
num = 300
num = 300
```

Reason：

```text
There is really only one object.
```

Changing it through `obj1` changes what `obj2` sees。

### 4.3 Alias can be useful but dangerous

Aliases allow multiple names for same object。

But they can cause surprising changes：

```text
Changing an object through one reference changes it for all aliases.
```

考试 tracing reference code 时，画 arrows 最稳。

---

## 5. Garbage Collection

### 5.1 What is garbage?

If an object no longer has any valid references to it：

```text
it can no longer be accessed by the program
```

That object is called：

```text
garbage
```

### 5.2 Example

```java
MyInteger obj1 = new MyInteger(100);
MyInteger obj2 = new MyInteger(200);
obj2 = obj1;
```

The original object `new MyInteger(200)` no longer has a reference。

It is garbage。

### 5.3 Java garbage collection

Java performs automatic garbage collection periodically。

Meaning：

```text
Java returns garbage object's memory to the system for future use.
```

In C/C++，programmer is responsible for memory management。

In Java，usually no explicit free/delete。

---

## 6. The `this` Reference

### 6.1 Meaning of `this`

`this` allows an object to refer to itself。

Inside a method/constructor：

```text
this refers to the object through which the method is being executed.
```

### 6.2 Distinguish parameters and instance variables

If parameter names same as instance variable names：

```java
public class Account {
    String name;
    long acctNumber;
    double balance;

    public Account(String name, long acctNumber, double balance) {
        this.name = name;
        this.acctNumber = acctNumber;
        this.balance = balance;
    }
}
```

Here：

```text
this.name        -> instance variable
name             -> constructor parameter
this.acctNumber  -> instance variable
acctNumber       -> parameter
```

### 6.3 Without `this`

If write：

```java
public Account(String name) {
    name = name;
}
```

This assigns parameter to itself。

Instance variable remains unchanged/default。

Correct：

```java
this.name = name;
```

### 6.4 `this` as returned object

Lecture slide has:

```java
public Die copy() {
    return this;
}
```

Important：

```text
return this does not create a copy.
It returns the same object reference.
```

So caller gets an alias。

If actual copy is needed：

```java
public Die copy() {
    return new Die(this);
}
```

assuming copy constructor exists。

---

## 7. Copy Constructor vs Alias

### 7.1 Copy constructor example

Slide 11：

```java
public Die(Die die) {
    faceValue = die.getFaceValue();
}
```

Then：

```java
Die die2 = new Die(3);
Die die3 = new Die(die2);
die2.setFaceValue(5);
```

`die3` is separate object。

If `die3` copied faceValue before change，then later changing `die2` does not change `die3`。

### 7.2 `copy()` returning `this`

Slide 12：

```java
public Die copy() {
    return this;
}
```

Then：

```java
Die die2 = new Die(3);
Die die3 = die2.copy();
die2.setFaceValue(5);
```

`die3` aliases `die2`。

Both refer to same object。

So both show updated faceValue。

### 7.3 Exam memory

| Code | Creates new object? | Result |
|---|---|---|
| `new Die(die2)` | yes | separate copy |
| `die3 = die2` | no | alias |
| `return this` | no | alias |
| `new Die()` | yes | new object |

---

## 8. 考试 tracing 模板

### 8.1 Primitive assignment

```text
Write value inside variable box.
Assignment copies value.
Later changes independent.
```

### 8.2 Reference assignment

```text
Draw object boxes separately.
Draw arrows from references to objects.
Assignment copies arrow target.
If an object has no arrows, it is garbage.
If two arrows point to same object, they are aliases.
```

### 8.3 Method call with aliases

If：

```java
obj2 = obj1;
obj1.changeValue(300);
```

Then：

```text
obj2 sees 300 too.
```

Because same object。

### 8.4 `this` question

If constructor parameter same name as field：

```java
this.field = field;
```

Left side is instance variable；right side is parameter。

---

## 9. 最容易错的点

### 9.1 Declaration 不创建 object

```java
Die die1;
```

No object yet。

Need：

```java
die1 = new Die();
```

### 9.2 Reference assignment 不复制 object

```java
die2 = die1;
```

does not copy Die object。

It copies reference。

### 9.3 `return this` 不是 deep copy

```java
return this;
```

returns same current object。

Creates alias。

### 9.4 Garbage is inaccessible object, not null reference

`null` reference itself is not object。

Garbage is an object with no valid references。

### 9.5 Parameter shadowing instance variable

```java
name = name;
```

almost certainly wrong when both parameter and field named `name`。

Use：

```java
this.name = name;
```

---

## 10. 最后 30 秒记忆版

```text
Variable holds primitive value or object reference.
Die die1; declares reference only, no object.
new creates object and calls constructor.
Instantiation = creating an object.
Object = instance of class.

Primitive variable contains value itself.
Object variable contains reference/address.

Primitive assignment copies value.
Reference assignment copies address.

Aliases = multiple references to same object.
Change object through one alias, all aliases see change.

Garbage = object with no valid references.
Java garbage collection is automatic.

this = current object reference.
Use this.field = field to distinguish instance variable from parameter.

new Die(die2) can create separate copy.
return this returns same object, not a copy.
```
