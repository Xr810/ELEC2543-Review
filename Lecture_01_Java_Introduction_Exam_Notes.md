# Lecture 1: Java Introduction - Exam Notes

> 范围：`Lecture/01_Java_Introduction.pdf`，主要是 Java language、compiler/interpreter、bytecode/JVM、program structure、`main` method、OOP、objects/classes、identifiers、reserved words。
> 读法：先看第 0 节速查，再重点看第 2 节 Java translation、第 3 节 `main` method、第 4 节 class/object。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Java Introduction 必背

| 问题 | 答案 |
|---|---|
| Java 最初由谁开发？ | Sun Microsystems，1990s early，1995 public release |
| Java 现在属于谁？ | Oracle bought Sun in 2010 |
| 本课程重点 Java platform | Java SE |
| Java 是什么类型语言？ | object-oriented programming language |
| CPU 理论上只懂什么？ | machine language |
| compiler 做什么？ | translates whole high-level program into machine language / intermediate code before execution |
| interpreter 做什么？ | translates and executes smaller parts one at a time |
| Java compiler 产生什么？ | bytecode |
| JVM 做什么？ | translates bytecode into machine language and executes it |
| Java 为什么 architecture-neutral？ | bytecode 不绑定特定 CPU，由 JVM 在各平台执行 |
| Java source file extension | `.java` |
| Java compiler output | `.class` bytecode |
| compile command | `javac HelloWorld.java` |
| run command | `java HelloWorld` |
| Java application entry point | `public static void main(String[] args)` |
| class 是什么？ | blueprint / concept |
| object 是什么？ | instance / realization of class |
| object 有什么？ | state + behaviors |
| identifier 能不能以 digit 开头？ | 不能 |
| Java identifiers case-sensitive 吗？ | yes |
| reserved word 能不能当 identifier？ | 不能 |

### `main` method 速查

| Part | Meaning |
|---|---|
| `public` | JVM can invoke it from outside the class |
| `static` | JVM can call it without creating an object |
| `void` | returns nothing |
| `main` | method name JVM looks for |
| `String[] args` | command-line arguments |

---

## 1. Java Language and Programming Levels

### 1.1 Java language

Java 是 object-oriented programming language。

平台配置：

| Platform | Meaning |
|---|---|
| Java SE | Standard Edition，本课程重点 |
| Java EE | Enterprise Edition |
| Java ME | Micro Edition |

Java 的核心特点之一：

```text
Write once, run anywhere
```

更准确地说：

```text
Java source code -> bytecode -> JVM on each platform executes bytecode.
```

### 1.2 Machine language

CPU 理论上只理解：

```text
machine language
```

特点：

- binary digits。
- tied to CPU architecture。
- Intel machine code 不能直接当 AMD / other architecture code 用。
- each instruction usually does a very simple task。

例子：

```text
000000 00001 00010 00110 00000 100000
```

### 1.3 Assembly language

Assembly language 用 symbols / mnemonics 替代 binary instruction。

特点：

```text
strong correspondence with machine code
```

也就是说 assembly 比 machine code 好读，但仍然接近 hardware。

### 1.4 3rd / 4th generation languages

High-level languages 更接近 human reasoning。

特点：

- one statement can do more work。
- require compiler or interpreter。
- 4GL offers higher abstraction。

Java 是 high-level language，需要 translation 才能执行。

---

## 2. Compiler, Interpreter, JVM

### 2.1 Compiler vs interpreter

| 对比 | Interpreter | Compiler |
|---|---|---|
| Translation unit | one statement / small part at a time | whole program at once |
| Analysis time | less upfront | more upfront |
| Overall execution | often slower | often faster after compilation |
| Intermediate code | usually no generated object code | generates intermediate/object code |
| Error reporting | stops when first error is confronted | reports after scanning whole program |
| Examples in slides | Python / Ruby | C / C++ |

### 2.2 Java translation

Java 不是直接 compile 成某个 CPU 的 machine code。

流程：

```text
Java source code (.java)
        |
        | javac
        v
Java bytecode (.class)
        |
        | JVM
        v
Machine code / execution
```

关键概念：

```text
The Java compiler translates Java source code into bytecode.
The JVM translates bytecode into machine language and executes it.
```

### 2.3 Architecture-neutral

Bytecode 不是传统 CPU 的 machine language。

所以：

```text
The Java compiler is not tied to any particular machine.
```

不同平台只要有对应 JVM，就可以执行同一份 bytecode。

这就是 Java 被称为：

```text
architecture-neutral
```

### 2.4 Java 必须先完整 compile

课件强调：

```text
As compilation is the first step for executing a Java program,
a complete Java program has to be developed before running any line of code.
```

不像 Python 那样逐行解释执行。

---

## 3. Java Program Structure

### 3.1 Structure hierarchy

Java program structure：

```text
program
  -> one or more classes
      -> one or more methods
          -> program statements
```

Java application 必须有一个：

```java
public static void main(String[] args)
```

作为起点。

### 3.2 File and class

Java 中：

```text
Each class is put in a .java file.
```

通常 public class name 和 file name 必须一致：

```text
HelloWorld.java contains public class HelloWorld
```

编译：

```text
javac HelloWorld.java
```

产生：

```text
HelloWorld.class
```

运行：

```text
java HelloWorld
```

注意运行时不用写 `.class`。

### 3.3 Class body and method body

Java 用 braces 定义 scope：

```java
public class MyProgram {
    public static void main(String[] args) {
        // statements
    }
}
```

重点：

```text
Braces define scope.
Indentation is for readability only.
```

这和 Python 不同。

### 3.4 Comments

Comments 可以放在几乎任何地方，不参与执行。

常见：

```java
// single-line comment

/*
 multi-line comment
*/
```

课件例子常用大块注释说明 author / class purpose。

---

## 4. `public static void main(String[] args)`

### 4.1 `public`

`public` 是 access modifier。

这里必须 public 的原因：

```text
JVM needs to invoke main() from outside the class.
```

如果写成 private，可能报：

```text
Main method not found in class, please define the main method as:
public static void main(String[] args)
```

### 4.2 `static`

`static` 使 method 与 class 关联，而不是与 object 关联。

`main` 必须 static，因为：

```text
JVM invokes main without instantiating the class.
```

如果没有 static，可能报：

```text
Main method is not static in class...
```

### 4.3 `void`

`void` 表示 method 不 return value。

`main` 结束时 program 结束，JVM 不需要 main 返回值。

如果写：

```java
public static int main(String[] args)
```

JVM 不会把它当标准 entry point。

### 4.4 `main`

`main` 是 JVM 寻找的 method name。

它不是 keyword。

如果写成：

```java
public static void myMain(String[] args)
```

也不是 Java application entry point。

### 4.5 `String[] args`

`args` 是 command-line arguments。

后面 Lecture 9 会继续讲。

基础记忆：

```text
String[] args means an array of String objects passed from command line.
```

---

## 5. Object-Oriented Programming

### 5.1 Object

Object 是 Java program 的 fundamental entity。

Object 可以代表 real-world entity，例如：

```text
bank account
```

Object 有：

1. `state`：descriptive characteristics。
2. `behaviors`：what it can do / what can be done to it。

例子：bank account

| Part | Example |
|---|---|
| state | account number, current balance |
| behavior | deposit, withdraw |

Behavior 可以改变 state。

### 5.2 Class

Class 是 object 的 blueprint。

```text
A class represents a concept.
An object represents the embodiment / realization of that concept.
```

例子：

```text
Class: BankAccount
Objects: John's Bank Account, Bill's Bank Account, Mary's Bank Account
```

同一个 class 可以创建多个 objects。

每个 object 可以有不同 state。

### 5.3 Methods

Class 用 methods 定义 object behaviors。

例子：

```java
deposit(...)
withdraw(...)
```

Methods 会在后面 lectures 详细讲。

### 5.4 Inheritance preview

One class can be used to derive another via inheritance。

例子 hierarchy：

```text
Account
  -> Credit Card Account
  -> Savings Account
  -> Checking Account
```

这里只是预告；Lecture 14 详细讲。

---

## 6. Identifiers and Reserved Words

### 6.1 Identifier 是什么？

Identifier 是 programmer 在 program 中使用的名字。

例子：

- class name：`Lincoln`
- variable name：`total`
- method name：`println`

### 6.2 Identifier rules

Java identifiers：

- can include letters。
- can include digits。
- can include underscore `_`。
- cannot begin with digit。
- are case-sensitive。

所以：

```text
Total, total, TOTAL are different identifiers.
```

### 6.3 Naming convention

常见 convention：

| Identifier type | Convention | Example |
|---|---|---|
| class name | TitleCase / PascalCase | `Lincoln`, `BankAccount` |
| variable/method | lowerCamelCase | `totalCount`, `getBalance` |
| constants | UPPER_CASE | `MAXIMUM`, `MIN_HEIGHT` |

### 6.4 Identifier quick check

题目：

```text
Remainder
9_multiple
addTwoIntegers
PI_VALUE
highestof3
sum&product
Diff_&_div
```

判断：

| Identifier | Valid? | Reason |
|---|---|---|
| `Remainder` | valid | letters only |
| `9_multiple` | invalid | begins with digit |
| `addTwoIntegers` | valid | letters |
| `PI_VALUE` | valid | letters + underscore |
| `highestof3` | valid | digit allowed after first char |
| `sum&product` | invalid | `&` not allowed in identifier |
| `Diff_&_div` | invalid | `&` not allowed |

### 6.5 Reserved words

Reserved words already have predefined meanings。

不能用作 identifier。

例子：

```text
class, public, static, void, int, if, else, while, return, new, this, super
```

如果写：

```java
int class = 5; // invalid
```

会 compile error。

---

## 7. 考试答题模板

### 7.1 Explain Java execution

```text
Java source code is compiled by javac into bytecode.
The JVM translates bytecode into machine language and executes it.
Because bytecode is not tied to a specific CPU, Java is architecture-neutral.
```

### 7.2 Write minimal Java program

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

Compile/run：

```text
javac HelloWorld.java
java HelloWorld
```

### 7.3 Explain class vs object

```text
A class is a blueprint or concept.
An object is an instance of that class.
Objects have state and behaviors.
Multiple objects can be created from the same class, each with its own state.
```

### 7.4 Identify valid identifiers

Checklist：

```text
Does it start with a digit? invalid.
Does it contain illegal symbols such as &? invalid.
Is it a reserved word? invalid.
Otherwise valid, case-sensitive.
```

---

## 8. 最容易错的点

### 8.1 `java HelloWorld.class` 是错的

Compile：

```text
javac HelloWorld.java
```

Run：

```text
java HelloWorld
```

不是：

```text
java HelloWorld.class
```

### 8.2 Java indentation 不定义 scope

错误理解：

```text
Indentation controls blocks like Python.
```

正确：

```text
Braces { } define scope.
Indentation only improves readability.
```

### 8.3 `main` signature 要准确

标准：

```java
public static void main(String[] args)
```

常见错误：

- missing `public`
- missing `static`
- return type not `void`
- method name not `main`
- wrong parameter shape

### 8.4 Class 和 object 不要混

错误：

```text
BankAccount is an object.
```

更准确：

```text
BankAccount is a class.
John's account is an object of that class.
```

### 8.5 Reserved word 不能当变量名

```java
int static = 3; // invalid
```

---

## 9. 最后 30 秒记忆版

```text
Java is object-oriented.
Java SE is the course focus.

Machine language is CPU-specific binary code.
Assembly uses mnemonics close to machine code.
High-level languages need compiler/interpreter.

Compiler: whole program.
Interpreter: smaller parts one at a time.

Java source (.java) -> javac -> bytecode (.class) -> JVM -> execution.
Bytecode + JVM makes Java architecture-neutral.

Program -> classes -> methods -> statements.
Java application entry point:
public static void main(String[] args)

public: JVM can access.
static: no object needed.
void: no return value.
main: JVM entry method name.

Class = blueprint/concept.
Object = instance/realization.
Object has state + behaviors.

Identifiers cannot start with digit, are case-sensitive, and cannot be reserved words.
```
