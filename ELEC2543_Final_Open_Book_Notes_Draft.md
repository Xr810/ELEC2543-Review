# ELEC2543 Final 开卷考试复习资料草稿

> 状态：可继续修改的打印草稿。
> 范围：Past Paper 2025 + Chapters 13-21 的复习整理。
> 风格：解释性文字用简体中文；Java 关键字、class、object、sorting、Big-O 等考试术语保留英文。

---

## 0. 使用方式

- 先看 **Past Paper 速查表**，快速定位题目对应的知识点。
- 再到对应的 **Part** 查看完整解释、代码和手算步骤。
- 代码块、输出结果、Java 关键字和算法名称保持英文，方便考试时直接对照题目。
- 标记为 **需要复核** 的地方，表示原始资料里前后有不一致，最终版应再对照 Past Paper 或 lecture slides。

---

## 1. 整体考试地图

### 1.1 Past Paper 2025 考点分布

| 题号 | 主题 | 需要掌握 |
|---|---|---|
| Q1 | Java MCQ 基础 | JVM、数据类型、`final`、`super`、`String`、exception 类型 |
| Q2a | Array + method 参数传递 | reference 行为、array 被 method 修改后的结果 |
| Q2b/c | `try-catch-finally` | 精确判断执行顺序和输出 |
| Q2d | `Integer` wrapper cache | `==` vs `equals` |
| Q3 | Binary Search Tree | 插入、Inorder、Preorder、Postorder |
| Q4 | AVL Tree | 最小高度、操作复杂度、rotation |
| Q5 | Merge Sort + Quick Sort | 手算步骤、average/worst complexity |
| Q6 | Inheritance + Interface | `extends`、`implements`、abstract class vs interface |

### 1.2 10 个 Part 总览

| Part | 内容 | 考试重要性 |
|---|---|---|
| Part 1 | Interface：定义、`implements`、`Comparable` | 高 |
| Part 2 | Inheritance：`extends`、`protected`、`super`、abstract class | 很高 |
| Part 3 | Polymorphism：late binding、inheritance/interface polymorphism | 高 |
| Part 4 | Exceptions：`try-catch-finally`、propagation、checked vs unchecked | 很高 |
| Part 5 | Recursion：base case、recursive case、Towers of Hanoi | 中 |
| Part 6 | Sorting I：Bubble、Insertion、Selection Sort + Big-O | 很高 |
| Part 7 | Sorting II：Merge Sort + Quick Sort | 很高 |
| Part 8 | Data Structures：Linked List、Stack、Queue | 高 |
| Part 9 | Trees & BST | 很高 |
| Part 10 | Advanced Trees：AVL Tree + Heap | 很高 |

---

## 2. Past Paper 速查表

| 题号 | 考点 | 关键答案 |
|---|---|---|
| Q1(i) | JVM | Java Virtual Machine |
| Q1(ii) | Boolean 默认值 | `false` |
| Q1(iii) | Java 不支持什么 | class 的 multiple inheritance |
| Q1(iv) | `System` / `String` 在哪个 package | `java.lang` |
| Q1(v) | 不存在的 exception 类型 | Compile-time exception |
| Q1(vi) | `final` 关键字用途 | All of the above |
| Q1(vii) | 没有 `main` method | Runtime error |
| Q1(viii) | `super` 关键字 | 引用直接 parent class |
| Q1(ix) | `String.concat()` | `String` 不可变，原 object 不变，例如 `blue` |
| Q1(x) | 字符串 + 数字拼接 | `Result: 1020` |
| Q2a | Array + method 参数传递输出 | `2 3 0 0 10 0` / `1 0 10 0 10 0` |
| Q2b | 有 exception 的 `try-catch-finally` | `abcde` |
| Q2c | 没有 exception 的 `try-catch-finally` | `abde` |
| Q2d | `Integer` cache | 小整数可能 `==` 为 true；大整数通常比较的是不同 reference |
| Q3b | BST 哪种 traversal 得到升序 | Inorder traversal |
| Q3c | BST preorder | `60, 41, 16, 25, 53, 46, 55, 74, 65, 63, 70` |
| Q3d | BST postorder | `25, 16, 46, 55, 53, 41, 63, 70, 65, 74, 60` |
| Q4a | AVL Tree 有 `n` 个 nodes 的最小高度 | `floor(log2 n)` |
| Q4b | AVL Tree 操作复杂度 | Search / Insert / Delete 都是 `O(log n)` |
| Q5c | Merge Sort complexity | Average = Worst = `O(n log n)` |
| Q5d | Quick Sort complexity | Average = `O(n log n)`，Worst = `O(n^2)` |

---

## 3. Java Basics / Output Tracing 补充速查

这一节放的是 Past Paper MCQ 和 output tracing 里容易散落出现的小规则。考试时如果题目不是某个大算法，而是在问 Java 基础语法，优先查这里。

### 3.1 Java 执行流程和 JVM

```text
.java source code
-> javac compiler
-> .class bytecode
-> JVM executes bytecode
```

记忆：

```text
Compiler translates; JVM executes.
```

`JVM` = `Java Virtual Machine`，作用是在具体平台上执行 Java bytecode。

### 3.2 Default Values

Java 的 instance variables 有 default value，但 local variables 没有自动 default value。

| Type | Default value |
|---|---|
| `int` | `0` |
| `double` | `0.0` |
| `boolean` | `false` |
| `char` | `'\u0000'` |
| object reference | `null` |

重点坑：

```java
class Test {
    boolean b;  // primitive, default false
    Boolean B;  // wrapper object, default null
}
```

记忆：

```text
primitive 有自己的 default value；object reference 默认 null。
```

### 3.3 Package 记忆

| Package | 常见 classes | 记法 |
|---|---|---|
| `java.lang` | `String`, `System`, `Math`, `Object` | Java 最基础的东西，自动 import |
| `java.util` | `Scanner`, `Random`, `ArrayList`, `HashMap` | 工具箱 |
| `java.io` | `File`, `FileReader`, `IOException`, `FileNotFoundException` | 文件 / 输入输出 |
| `java.text` | `NumberFormat`, `DecimalFormat` | 数字、钱、百分比格式化 |

`java.lang` 会自动 import，所以不用写：

```java
import java.lang.String;
import java.lang.System;
```

记忆：

```text
String/System/Math/Object -> java.lang
Scanner/Random/ArrayList -> java.util
File/IOException -> java.io
NumberFormat/DecimalFormat -> java.text
```

### 3.4 `final` Keyword

`final` 常见用途：

```text
final variable  -> constant，不能重新 assign
final method    -> 不能被 override
final class     -> 不能被 extended
```

所以 Past Paper MCQ 里如果问 `final` 的用途，通常是 `All of the above`。

### 3.5 `String` Immutable、`concat()` 和拼接

`String` 是 immutable。`concat()` 会返回一个新的 `String`，但不会改变原来的 object。

```java
String s = "blue";
s.concat("berry");
System.out.println(s);
```

输出仍然是：

```text
blue
```

要得到 `"blueberry"`，必须保存返回值：

```java
s = s.concat("berry");
```

也可以写：

```java
s = s + "berry";
```

字符串和数字拼接时，一旦左边已经是 `String`，后面的 `+` 会继续做 string concatenation。

```java
System.out.println("Result: " + 10 + 20);
```

输出：

```text
Result: 1020
```

Java 大小写敏感：

```java
System.out.println(s);  // correct
system.out.println(s);  // wrong
```

### 3.6 Method 参数、Primitive 和 Array Reference

method header：

```java
public static void mystery(int[] a, int x, int y)
```

意思是：

```text
第 1 个参数：int[] a，int array
第 2 个参数：int x，整数
第 3 个参数：int y，整数
```

如果调用：

```java
mystery(a, y, x);
```

对应关系是：

```text
method 的 a = main 的 a
method 的 x = main 的 y
method 的 y = main 的 x
```

核心规则：

- `int` 是 primitive，传进 method 是复制 value。method 里改 `x`、`y` 不影响 main 里的 `x`、`y`。
- `int[]` 是 array object，传进 method 是复制 reference。method 里改 `a[2]` 会影响 main 里的同一个 array。

输出代码：

```java
System.out.print(x + " " + y + " ");

for (int element: a)
    System.out.print(element + " ");

System.out.println();
```

逻辑：

```text
先打印 x 和 y
再依次打印 array a 里的每个 element
最后 println() 换行
```

Past Paper Q2(a) 的答案输出：

```text
2 3 0 0 10 0
1 0 10 0 10 0
```

记忆：

```text
primitive parameter = copy value
array parameter = copy reference to same array object
```

---

# Part 1 / 10：Interface

## 1.1 什么是 Interface？

Interface 是一组行为要求。它告诉一个 class 必须实现哪些 methods，但它自己通常不写 method body。

可以把 interface 理解成一份合同：

- Interface = 合同 / 行为规范。
- `implements` = 某个 class 接受这份合同。
- 只要 class implements 某个 interface，就必须实现 interface 里的所有 required methods。

```java
public interface Doable {
    public void doThis();
    public int doThat();
    public boolean doTheOther(int num);
}
```

重点：

- 用 `interface`，不是 `class`。
- interface 里的 methods 默认是 `public` 和 `abstract`。
- method header 后面直接写 `;`，不能写 `{}`。
- interface 不能直接 instantiate。
- 传统 interface 不能有普通 instance variables，但可以有 constants。

interface method 的默认修饰符是：

```text
public abstract
```

所以下面三种写法等价：

```java
public interface Animal {
    public abstract void eat();
}
```

```java
public interface Animal {
    public void eat();
}
```

```java
public interface Animal {
    void eat();
}
```

考试里建议写得最清楚：

```java
public interface Animal {
    public void eat();
}
```

interface 里的 variable 默认是：

```text
public static final
```

所以：

```java
public interface Constants {
    int MAX_SIZE = 100;
}
```

等价于：

```java
public interface Constants {
    public static final int MAX_SIZE = 100;
}
```

记忆：

```text
interface method = public abstract by default
interface variable = public static final by default
interface defines behavior, not object state
```

## 1.2 怎样 implement 一个 Interface？

class 用 `implements` 实现 interface。

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

如果 class 没有实现 interface 里的所有 methods，编译会报错，除非这个 class 本身是 abstract class。

## 1.3 Interface 可以当作 Type 使用

interface 不能直接 new object，但可以作为 reference type。

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

这里 `Hello` 这个 interface type 可以指向 `Chinese` object，也可以指向 `English` object。这就是 polymorphism 的一种表现。

## 1.4 Interface 可以当作 Parameter

interface type 可以作为 method 参数，这样一个 method 就能接收任何 implements 了这个 interface 的 object。

```java
public static void invoke_method(Hello person) {
    person.sayHello();
}

invoke_method(new Chinese("Liu", "Bang"));
invoke_method(new English("Obama", "Barack"));
```

好处：method 不需要关心具体 class，只需要知道传进来的 object 一定有 interface 规定的 methods。

## 1.5 一个 Class 可以 implement 多个 Interfaces

Java 不支持 class 的 multiple inheritance，但一个 class 可以 implement 多个 interfaces。

```java
class ManyThings implements Interface1, Interface2, Interface3 {
    // implement all methods from all interfaces
}
```

Past Paper 相关例子：

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

`Eagle` 同时 implements `Animal` 和 `Wings`，所以必须写出 `eat()` 和 `fly()`。

Inheritance / interface 关系总结：

```text
class extends class: 只能一个
class implements interface: 可以多个
interface extends interface: 可以多个
```

错误：

```java
class C extends A, B {
}
```

正确：

```java
class C extends A {
}

class C implements A, B {
}
```

## 1.6 `Comparable`

`Comparable` 是 Java 标准库中用来定义 object 比较规则的 interface。

`Comparable` 已经由 Java 标准库提供，不需要自己重新写这个 interface。你需要做的是让自己的 class `implements Comparable`，然后写出 `compareTo(Object other)` 的 method body。

```java
public interface Comparable {
    int compareTo(Object other);
}
```

`compareTo` 的返回值含义：

| 返回值 | 含义 |
|---|---|
| 负数 | `this < other` |
| `0` | `this == other` |
| 正数 | `this > other` |

例子：

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

这里先把 `other` cast 成 `Salary`，再比较两个 salary object 的总收入。

另一个简单例子：

```java
public class Student implements Comparable {
    private int score;

    public int compareTo(Object other) {
        Student s = (Student) other;
        return this.score - s.score;
    }
}
```

如果写：

```java
return this.score - s.score;
```

就是按 `score` 从小到大比较。

如果写：

```java
return s.score - this.score;
```

就是按 `score` 从大到小比较。

总结：

```text
Comparable 已经写好的是：compareTo 的 method 要求。
Comparable 没有写好的是：具体比较逻辑。
implements Comparable 后：必须自己写 compareTo 的 method body。
```

`Comparable` 也可以作为参数或返回类型：

```java
public static Comparable largest(Comparable c1, Comparable c2, Comparable c3) {
    if (c1.compareTo(c2) > 0 && c1.compareTo(c3) > 0) return c1;
    if (c2.compareTo(c1) > 0 && c2.compareTo(c3) > 0) return c2;
    return c3;
}
```

例子：

```java
largest(1, 8, 6);                      // 8
largest("apple", "bye", "ELEC2543");   // "bye"，按 dictionary order
```

## 1.7 Interface 之间也可以继承

interface 可以用 `extends` 继承其他 interfaces。

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

如果 class implements `Calculator`，它必须实现 `Calculator` 继承到的所有 methods。

## 1.8 小结

- Interface 是行为规范，不是普通 class。
- class 用 `implements` 实现 interface。
- implement interface 后必须实现所有 methods。
- interface 不能 instantiate，但可以作为 variable type 和 parameter type。
- 一个 class 可以 implement 多个 interfaces。
- `Comparable` 用 `compareTo` 定义 object 比较规则。
- interface 可以 `extends` 其他 interfaces。

---

# Part 2 / 10：Inheritance

## 2.1 什么是 Inheritance？

Inheritance 让一个新 class 从已有 class 派生出来。child class 会自动得到 parent class 中可访问的 variables 和 methods。

常用术语：

| 英文 | 同义词 | 中文 |
|---|---|---|
| parent class | superclass / base class | 父类 |
| child class | subclass / derived class | 子类 |

Inheritance 应该表示 **is-a** 关系。

```text
Vehicle
   ^
   |
  Car
```

这个关系表示：Car is a Vehicle。

## 2.2 `extends`

Java 用 `extends` 表示 class 继承。

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

使用：

```java
Dictionary webster = new Dictionary();
System.out.println(webster.getPages());
System.out.println(webster.getDefinitions());
System.out.println(webster.computeRatio());
```

`Dictionary` object 可以调用 `getPages()`，因为这个 method 是从 `Book` 继承来的。

## 2.3 `protected`

| Modifier | 同一个 class | subclass | 其他 class |
|---|---:|---:|---:|
| `private` | 可以 | 不可以 | 不可以 |
| `protected` | 可以 | 可以 | 通常不可以，同 package 除外 |
| `public` | 可以 | 可以 | 可以 |

重点：

- `private` member 仍然存在于 object 内部，但 subclass 不能直接用名字访问。
- `protected` member 可以被 subclass 直接访问。
- UML 中 `#` 表示 protected，`+` 表示 public。

```text
Book
# pages : int
+ setPages() : void
+ getPages() : int
```

## 2.4 `super`

先区分 `this` 和 `super`：

```text
this = current object
super = parent part of current object
```

- `this` 表示当前 object。
- `super` 表示当前 object 的 immediate superclass 部分。
- `super.eat()` 是调用 parent class 版本的 `eat()`。
- `super()` 是调用 parent constructor，但 `super` 本身不等于 constructor。

### 用法一：调用 parent constructor

`super(...)` 必须写在 child constructor 的第一行。

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

### 用法二：调用 parent method

如果 child class override 了 parent method，但仍想调用 parent 版本，就用 `super.methodName()`。

```java
public class Advice extends Thought {
    public void message() {
        System.out.println("Warning: Dates in calendar are closer than they appear.");
        super.message();
    }
}
```

## 2.5 Method Overriding

Overriding 指 child class 重新定义 parent class 已有的 method。要求 method name 和 parameter list 完全相同。

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

当调用 `Advice` object 的 `message()` 时，执行的是 child class 版本。这个 child class 版本里面又用 `super.message()` 调用了 parent class 版本。

## 2.6 Overloading vs Overriding

| | Overloading | Overriding |
|---|---|---|
| 发生位置 | 同一个 class 内 | parent class 和 child class 之间 |
| method name | 相同 | 相同 |
| parameter list | 不同 | 相同 |
| 作用 | 同名 method 处理不同参数 | child class 改写 parent behavior |

Overloading：

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

Overriding：

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

规则：

- `final` method 不能被 override。
- `final` class 不能被继承。
- constructor 不会被继承，所以 constructor 不存在 overriding。
- child class 可以定义和 parent class 同名的 variable，但这叫 shadowing，通常不推荐。

## 2.7 Class Hierarchy

Inheritance 可以形成多层 class hierarchy。

```text
        HKUPerson
        /      \
   Student    Staff
```

- 一个 subclass 可以继续作为其他 class 的 superclass。
- 同一个 parent class 的多个 child classes 叫 siblings。
- child class 会继承所有 ancestor classes 中可访问的 members。

## 2.8 `Object` Class

所有 Java class 都直接或间接继承自 `Object`。

常见 inherited methods：

| Method | 作用 |
|---|---|
| `String toString()` | 返回 object 的 string 表示 |
| `boolean equals(Object obj)` | 默认比较 reference |
| `Object clone()` | 复制 object |

我们写 `toString()` 时，其实是在 override `Object.toString()`。

## 2.9 Abstract Class

abstract class 不能 instantiate，通常用来表示一个抽象概念。

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

child class 必须实现所有 abstract methods，否则 child class 也必须声明成 abstract class。

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
| 关键字 | `abstract class` | `interface` |
| 可以有 implemented methods | 可以 | 传统上不可以 |
| 可以有 instance variables | 可以 | 不可以 |
| 关系关键字 | `extends` | `implements` |
| 多继承 | class 只能 extends 一个 class | class 可以 implements 多个 interfaces |
| 能否 instantiate | 不可以 | 不可以 |
| 主要用途 | 有共同变量或共同实现的父类 | 行为合同 / 规范 |

## 2.10 Visibility 和 Indirect Access

subclass 不能直接访问 parent class 的 `private` members，但可以通过 public/protected methods 间接访问。

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

这里 `calories()` 是 private method，subclass 不能直接调用，但可以调用 public method `caloriesPerServing()` 间接使用它。

## 2.11 小结

- `extends` 用来建立 inheritance。
- inheritance 应该表示 is-a 关系。
- `protected` 让 subclass 可以访问，同时限制外部 class。
- `super(...)` 调用 parent constructor，必须在 constructor 第一行。
- `super.method()` 调用 parent method。
- Overriding：child class 中同名同参数。
- Overloading：同一个 class 中同名不同参数。
- abstract class 不能 instantiate。
- 所有 Java class 都继承自 `Object`。

---

# Part 3 / 10：Polymorphism

## 3.1 Polymorphic Reference

polymorphic reference 指一个 reference variable 可以在不同时刻指向不同类型的 object。

```java
Holiday day;
day = new Christmas();
```

parent class type 的 reference 可以指向 child class object。

## 3.2 Late Binding

Binding 指把 method call 和具体 method implementation 连接起来。

| 类型 | 决定时间 |
|---|---|
| Early binding | compile time |
| Late binding / dynamic binding | runtime |

Java 对 overridden methods 使用 late binding。真正调用哪个 method 版本，取决于 object 的 actual runtime type，而不是 reference 的 declared type。

```java
Holiday day = new Christmas();
day.celebrate();  // 如果 Christmas override 了 celebrate()，就调用 Christmas 版本
```

重要规则：compiler 会根据 declared type 检查 method 是否存在。

```java
Holiday day = new Christmas();
day.getTree();  // 如果 Holiday 没有 getTree()，这里 compile error
```

如果要调用 child class 独有 method，需要 cast。

```java
((Christmas) day).getTree();
```

## 3.3 通过 Inheritance 实现 Polymorphism

class hierarchy：

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

不同 child classes 用不同方式实现 `pay()`：

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

用 `StaffMember[]` 统一管理不同类型的 staff object：

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

`staffList[count].pay()` 是 polymorphic method call：

- actual object 是 `Executive`，就调用 `Executive.pay()`。
- actual object 是 `Employee`，就调用 `Employee.pay()`。
- actual object 是 `Hourly`，就调用 `Hourly.pay()`。
- actual object 是 `Volunteer`，就调用 `Volunteer.pay()`。

## 3.4 References 和 Inheritance

Upcasting：child type 到 parent type，安全，不需要显式 cast。

```java
MusicPlayer mplayer = new CDPlayer();
```

Downcasting：parent type 到 child type，需要显式 cast，有 runtime 风险。

```java
CDPlayer cdplayer = (CDPlayer) mplayer;
```

危险例子：

```java
CDPlayer cdplayer = (CDPlayer) new MusicPlayer();  // 可能 compile，但 runtime error
```

原因：所有 `CDPlayer` 都是 `MusicPlayer`，但不是所有 `MusicPlayer` 都是 `CDPlayer`。

## 3.5 通过 Interface 实现 Polymorphism

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

限制仍然一样：declared type 是 `Speaker`，所以只能调用 `Speaker` interface 里声明过的 methods。

```java
Speaker special = new Philosopher();
special.pontificate();  // error: Speaker 里没有 pontificate()
```

Quick Check：

```java
Speaker first = new Dog();              // legal
Philosopher second = new Philosopher(); // legal
second.pontificate();                   // legal
first = second;                         // legal
```

## 3.6 Polymorphism 在 Sorting 中的用途

用 `Comparable[]` 可以写出通用 selection sort。

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

`compareTo` 的实际调用版本由 actual object type 决定：

- 如果 array 里是 `String` objects，就调用 `String.compareTo()`。
- 如果 array 里是 `Contact` objects，就调用 `Contact.compareTo()`。
- 如果 array 里是 `Salary` objects，就调用 `Salary.compareTo()`。

`Contact` 例子：

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

这个 `compareTo` 先按 last name 比；last name 一样时，再按 first name 比。

## 3.7 小结

- Polymorphism 表示同一个 reference 可以指向不同类型的 object。
- Late binding 在 runtime 决定调用哪个 overridden method。
- compiler 用 declared type 检查能不能调用某个 method。
- actual runtime object 决定 method 的真正执行版本。
- Upcasting 安全；downcasting 需要显式 cast，可能 runtime error。
- Polymorphism 可以通过 inheritance，也可以通过 interface 实现。
- `Comparable` + polymorphism 可以实现通用 sorting method。

---

# Part 4 / 10：Exceptions

## 4.1 什么是 Exception？

exception 是一个描述异常或错误情况的 object。

遇到 exception 后有三种处理方式：

- 忽略它：程序终止并打印 stack trace。
- 在发生处用 `try-catch` 处理。
- 不在当前 method 处理，让 exception 向上层 caller propagation。

## 4.2 不处理 Exception 的结果

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

输出包含：

```text
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at Zero.main(Zero.java:17)
```

第二个 `println` 不会执行，因为程序已经在除以 0 的地方终止。

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

规则：

- `try` 里一旦出现 exception，马上跳出 `try` block。
- Java 只执行第一个匹配的 `catch` block。
- 如果没有 exception，所有 `catch` blocks 都跳过。

## 4.4 `finally`

`finally` 一定会执行，不管有没有 exception。

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

执行顺序：

- 有 exception：`try` 部分执行 -> 匹配的 `catch` -> `finally` -> 后续代码。
- 无 exception：`try` 完整执行 -> 跳过 `catch` -> `finally` -> 后续代码。

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

有 `throw` 时：

```text
a -> b -> exception -> catch RuntimeException -> c -> finally d -> e
```

输出：

```text
abcde
```

原因：`IllegalArgumentException` 是 `RuntimeException` 的 subclass，所以 `catch (RuntimeException e)` 能接住它。

如果把 `throw` 那行注释掉：

```text
a -> b -> no exception -> skip catch -> finally d -> e
```

输出：

```text
abde
```

## 4.6 Exception Propagation

如果 method 里出现 exception 但不处理，exception 会沿着 method call chain 向上传播。

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

exception 在 `level3` 出现后：

- `level3` 中 exception 后面的语句不执行。
- `level2` 中调用 `level3()` 后面的语句不执行。
- exception 一直向上传到 `level1`，被 `catch` 处理后程序继续。

如果传播到 `main` 仍没人处理，程序终止。

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

`Error` 通常表示严重问题，一般程序不主动 catch。

## 4.8 Checked vs Unchecked Exceptions

| | Checked Exception | Unchecked Exception |
|---|---|---|
| 定义 | `Exception` subclass，但不是 `RuntimeException` subclass | `RuntimeException` 及其 subclasses |
| compiler 要求 | 必须 catch 或用 `throws` 声明 | compiler 不强制 |
| 例子 | `IOException`, `FileNotFoundException`, `ClassNotFoundException`, `NoSuchMethodException` | `NullPointerException`, `ArithmeticException`, `ArrayIndexOutOfBoundsException`, `NumberFormatException` |

Past Paper Q1(v)：

- Checked exception：存在。
- Unchecked exception：存在。
- Runtime exception：存在。
- Compile-time exception：不存在。

注意：正确说法是 `compile-time error`，不是 `compile-time exception`。

例子：

```java
int x = ;              // syntax error
System.out.println(y); // cannot find symbol
```

## 4.9 `throw`

`throw` 用来主动抛出 exception。

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

在同一条执行路径上，`throw` 后面的代码 unreachable。

## 4.10 I/O 和 `IOException`

文件 I/O 可能抛出 checked exception，所以要处理或声明 `throws IOException`。

```java
import java.io.*;

public static void main(String[] args) throws IOException {
    PrintWriter outFile = new PrintWriter("test.txt");
    outFile.println("Hello, file!");
    outFile.close();
}
```

标准 I/O streams：

- `System.out`：standard output。
- `System.in`：standard input。
- `System.err`：standard error output。

## 4.11 小结

- `try` 放可能出错的代码。
- `catch` 处理匹配的 exception。
- `finally` 一定执行。
- checked exception 必须 catch 或 `throws`。
- unchecked exception 是 `RuntimeException` subclasses。
- exception propagation 会把未处理的 exception 向 caller 传递。
- Q2b 输出：`abcde`。
- Q2c 输出：`abde`。

---

# Part 5 / 10：Recursion

## 5.1 什么是 Recursion？

recursion 指用自己定义自己，或者 method 调用自己。

递归定义一个 list：

```text
A List is:
  a number
  or a number comma List
```

例子：

```text
24, 88, 40, 37
= 24, (88, (40, (37)))
```

## 5.2 两个必要部分

每个 recursive method 都必须有：

| 部分 | 含义 |
|---|---|
| Base case | 停止条件，不再 recursive call |
| Recursive case | 调用自己，并且每次更接近 base case |

没有 base case 会导致 infinite recursion，最后 stack overflow。

## 5.3 Factorial

数学定义：

```text
1! = 1
N! = N * (N - 1)!
```

Java：

```java
public int factorial(int n) {
    if (n == 1)
        return 1;
    else
        return n * factorial(n - 1);
}
```

追踪：

```text
factorial(5)
= 5 * factorial(4)
= 5 * 4 * factorial(3)
= 5 * 4 * 3 * factorial(2)
= 5 * 4 * 3 * 2 * factorial(1)
= 5 * 4 * 3 * 2 * 1
= 120
```

每次 recursive call 都有自己的 execution environment，包括自己的参数和 local variables。

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

追踪：

```text
sum(4)
= 4 + sum(3)
= 4 + 3 + sum(2)
= 4 + 3 + 2 + sum(1)
= 10
```

注意：能用 recursion 不代表一定该用 recursion。像简单求和，loop 通常更清楚。

## 5.5 Direct vs Indirect Recursion

Direct recursion：method 直接调用自己。

```java
public void methodA() {
    methodA();
}
```

Indirect recursion：多个 methods 之间绕一圈再回到自己。

```text
m1() -> m2() -> m3() -> m1()
```

两种 recursion 都需要 base case。

## 5.6 Maze Traversal

递归走迷宫通常使用 backtracking。

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

逻辑：当前位置无效就返回 false；到终点就返回 true；否则向四个方向继续 recursive search。

## 5.7 Towers Of Hanoi

规则：

- 3 根柱子。
- N 个 disks 起始都在第 1 根柱子。
- 目标是全部移动到第 3 根柱子。
- 每次只能移动一个 disk。
- 大 disk 不能放在小 disk 上。

递归思路：

1. 把上面 `N - 1` 个 disks 从 start 移到 temp。
2. 把最大的 disk 从 start 移到 end。
3. 把 `N - 1` 个 disks 从 temp 移到 end。

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

N 个 disks：

```text
number of moves = 2^N - 1
time complexity = O(2^N)
```

## 5.8 小结

- Recursion = method 调用自己。
- 必须有 base case 和 recursive case。
- 没有 base case 会 infinite recursion / stack overflow。
- Factorial、sum、maze traversal、Towers of Hanoi 是重点例子。
- Towers of Hanoi 需要 `2^N - 1` 步。
- recursion 适合天然能拆成同结构小问题的问题。

---

# Part 6 / 10：Sorting I - Simple Sorting Algorithms + Big-O

## 6.1 Sorting 基础

sorting 是把元素按照某种顺序排列，通常默认升序。

Bubble Sort、Insertion Sort、Selection Sort 的共同点：

- 都用多轮 passes。
- 每一轮让 array 更接近 sorted。
- time complexity 都是 `O(n^2)`。

## 6.2 Bubble Sort

策略：相邻元素两两比较，需要时交换，让较小元素一步步向前移动。

例子：对 `[2, 4, 6, 1, 3, 5]` 做第一轮，从底部开始扫描。

```text
[2, 4, 6, 1, 3, 5]
compare 3,5 -> no swap
compare 1,3 -> no swap
compare 6,1 -> swap -> [2, 4, 1, 6, 3, 5]
compare 4,1 -> swap -> [2, 1, 4, 6, 3, 5]
compare 2,1 -> swap -> [1, 2, 4, 6, 3, 5]
```

代码：

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

策略：左边维护一个 sorted region，每次把下一个元素插入到正确位置。

例子：

```text
初始: [2] | [4, 6, 1, 3, 5]
Insert 4 -> [2, 4] | [6, 1, 3, 5]
Insert 6 -> [2, 4, 6] | [1, 3, 5]
Insert 1 -> [1, 2, 4, 6] | [3, 5]
Insert 3 -> [1, 2, 3, 4, 6] | [5]
Insert 5 -> [1, 2, 3, 4, 5, 6]
```

代码：

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

策略：每一轮在 unsorted region 找最小值，把它 swap 到当前第一个 unsorted position。

例子：

```text
[2, 4, 6, 1, 3, 5]
round 1: min 1, swap with 2 -> [1, 4, 6, 2, 3, 5]
round 2: min 2, swap with 4 -> [1, 2, 6, 4, 3, 5]
round 3: min 3, swap with 6 -> [1, 2, 3, 4, 6, 5]
round 4: min 4, already correct
round 5: min 5, swap with 6 -> [1, 2, 3, 4, 5, 6]
```

代码：

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

## 6.5 三种简单 sorting 对比

| Algorithm | 核心思路 | 每轮结果 | Complexity |
|---|---|---|---|
| Bubble Sort | 相邻比较和 swap | 最小值逐步到正确位置 | `O(n^2)` |
| Insertion Sort | 插入到 sorted region | sorted region 扩大 | `O(n^2)` |
| Selection Sort | 找最小值并 swap | 一个元素固定 | `O(n^2)` |

## 6.6 Running Time Analysis

常见分析角度：

- Worst case：最多操作次数。
- Average case：平均操作次数。
- Best case：最少操作次数。

## 6.7 Big-O

Big-O 描述 input size `n` 变大时，algorithm running time 的增长趋势。它忽略常数和低阶项。

```text
3n^3 + 90n^2 - 2n + 5 = O(n^3)
```

正式含义：

```text
f(n) = O(g(n))
if there exist constants c and n0 such that
for all n >= n0, f(n) <= c * g(n)
```

常见复杂度从快到慢：

| Big-O | 名称 | 例子 |
|---|---|---|
| `O(1)` | constant | array random access |
| `O(log n)` | logarithmic | binary search |
| `O(n)` | linear | scanning array |
| `O(n log n)` | linearithmic | Merge Sort，average Quick Sort |
| `O(n^2)` | quadratic | Bubble，Insertion，Selection |
| `O(2^n)` | exponential | Towers of Hanoi |

例子：

```text
2n^2 = O(n^2)  正确，而且是 tight bound
2n^2 = O(n^3)  也正确，但不 tight
2n^2 = O(n)    错误
```

考试通常写最紧的上界。

## 6.8 为什么简单 sorting 是 `O(n^2)`？

以 Bubble Sort 为例：

```text
(n - 1) + (n - 2) + ... + 2 + 1
= n(n - 1) / 2
= 0.5n^2 - 0.5n
= O(n^2)
```

Insertion Sort 和 Selection Sort 的分析类似。

## 6.9 小结

- Bubble Sort：相邻比较和 swap，`O(n^2)`。
- Insertion Sort：插入到左侧 sorted region，`O(n^2)`。
- Selection Sort：找最小值并 swap，`O(n^2)`。
- Big-O 看增长趋势，不看常数和低阶项。
- 复杂度顺序：`O(1) < O(log n) < O(n) < O(n log n) < O(n^2) < O(2^n)`。

---

# Part 7 / 10：Sorting II - Merge Sort + Quick Sort

## 7.1 Divide And Conquer

Merge Sort 和 Quick Sort 都使用 divide-and-conquer。

- Divide：把大问题拆成小问题。
- Conquer：解决小问题。
- Combine：把小问题结果合并成大问题结果。

---

## 7A. Merge Sort

## 7.2 Merge Sort 思路

```text
Split array into two halves
Sort left half recursively
Sort right half recursively
Merge two sorted halves
```

关键：merge 两个已经 sorted 的 arrays 是 `O(n)`。

## 7.3 Past Paper 数组：`[2, 6, 5, 7, 9, 8, 3, 4]`

Divide：

```text
[2, 6, 5, 7, 9, 8, 3, 4]
-> [2, 6, 5, 7]    [9, 8, 3, 4]
-> [2, 6] [5, 7]   [9, 8] [3, 4]
-> [2] [6] [5] [7] [9] [8] [3] [4]
```

第一层 merge：

```text
[2] + [6] -> [2, 6]
[5] + [7] -> [5, 7]
[9] + [8] -> [8, 9]
[3] + [4] -> [3, 4]
```

第二层 merge：

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

最后 merge：

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

## 7.4 Merge Sort 代码

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

递推式：

```text
T(1) = c
T(n) = 2T(n/2) + kn
```

展开：

```text
T(n) = 2[kn/2 + 2T(n/4)] + kn
     = 2kn + 4T(n/4)
     = 3kn + 8T(n/8)
     = i*kn + 2^i*T(n/2^i)
```

当 `n / 2^i = 1`，有 `i = log2 n`。

```text
T(n) = kn log n + cn
     = O(n log n)
```

| 情况 | Complexity |
|---|---|
| Best | `O(n log n)` |
| Average | `O(n log n)` |
| Worst | `O(n log n)` |

Merge Sort 的 running time 很稳定，best、average、worst 都是 `O(n log n)`。

---

## 7B. Quick Sort

## 7.6 Quick Sort 思路

```text
Choose pivot
Partition:
  left side <= pivot
  right side > pivot
Put pivot in final correct position
Recursively sort both sides
```

关键：partition 后，pivot 已经在它最终正确的位置。

## 7.7 Past Paper 数组，选择第一个元素作为 pivot

初始：

```text
[2, 6, 5, 7, 9, 8, 3, 4]
```

第一次 partition，pivot = `2`：

```text
右边没有比 2 更小或等于 2 的元素。
结果: [2] | [6, 5, 7, 9, 8, 3, 4]
```

对右边继续 partition，pivot = `6`：

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

现在：

```text
[2] [3, 5, 4] [6] [8, 9, 7]
```

继续 recursive sorting：

```text
[2] [3] [4, 5] [6] [7] [8, 9]
=> [2, 3, 4, 5, 6, 7, 8, 9]
```

## 7.8 Quick Sort 代码

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

Best / average case：

```text
pivot 每次大致把 array 分成两半
number of levels = log2 n
work per level = n
total = O(n log n)
```

Worst case：

```text
pivot 每次都是最小值或最大值
n + (n - 1) + (n - 2) + ... + 1
= O(n^2)
```

常见 worst case：array 已经 sorted，同时每次选第一个元素作为 pivot。

## 7.10 Merge Sort vs Quick Sort

| | Merge Sort | Quick Sort |
|---|---|---|
| 核心策略 | split 后 merge | around pivot partition |
| Pivot | 没有 | 有 |
| Average | `O(n log n)` | `O(n log n)` |
| Worst | `O(n log n)` | `O(n^2)` |
| Extra space | `O(n)` | 通常 in-place |
| Stability | stable | not stable |
| 实际特点 | 稳定但需要额外 memory | 通常很快，memory efficient |

## 7.11 小结

- Merge Sort：divide、recursive sort、merge。
- Quick Sort：choose pivot、partition、recursive sort。
- Merge Sort average 和 worst 都是 `O(n log n)`。
- Quick Sort average 是 `O(n log n)`，worst 是 `O(n^2)`。
- Quick Sort worst case 常见于 already sorted array + first element as pivot。

---

# Part 8 / 10：Data Structures - Linked List, Stack, Queue

## 8.1 Static vs Dynamic Data Structures

| | Static | Dynamic |
|---|---|---|
| size | compile time 固定 | runtime 可增减 |
| 例子 | array | linked list、stack、queue、tree |
| 优点 | 简单、高效 | 灵活 |
| 成本 | 大小不灵活 | 需要额外 reference |

这里的 static 指 data structure size 是否固定，不是 Java 的 `static` keyword。

## 8.2 Abstract Data Type

ADT 把以下内容打包在一起：

- 数据组织方式。
- 对数据的操作。
- 隐藏内部 implementation details。

Java collection ADTs 包括：

- `List`
- `Stack`
- `Queue`
- `Set`
- `Map`

使用者只关心能做什么 operations，不关心内部如何实现。

## 8.3 Object References As Links

动态 data structure 的核心是用 object reference 连接 nodes。

```java
class Node {
    int info;
    Node next;
}
```

`next` 存的是下一个 `Node` object 的 reference。

## 8.4 Linked List

Singly linked list：

```text
head -> [data|next] -> [data|next] -> [data|null]
```

Doubly linked list：

```text
null <- [prev|data|next] <-> [prev|data|next] <-> [prev|data|next] -> null
```

## 8.5 Linked List 基本操作

在 `current` 后插入 `newNode`：

```java
newNode.next = current.next;
current.next = newNode;
```

顺序很重要。必须先让 `newNode.next` 指向原来的下一个 node，再让 `current.next` 指向 `newNode`。如果顺序反了，原来的下一个 node 可能会丢失。

删除 `current` 后面的 node：

```java
if (current.next != null)
    current.next = current.next.next;
```

Doubly linked list 删除：

```java
if (current.next != null) {
    current.next = current.next.next;
    if (current.next != null)
        current.next.prev = current;
}
```

## 8.6 MagazineList 例子

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

这里 `MagazineNode` 是 inner class，用来表示 linked list 里的 node。

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

Queue 是 FIFO：First-In, First-Out。

类比：排队。先进入队伍的人先被服务。

操作：

| Operation | 含义 |
|---|---|
| `enqueue(item)` | 从 rear 加入 item |
| `dequeue()` | 从 front 移除并返回 item |
| `empty()` | 判断 queue 是否为空 |

练习：

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

Linked-list implementation：

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

ArrayList implementation：

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

Stack 是 LIFO：Last-In, First-Out。

类比：一叠盘子，最后放上去的最先拿走。

操作：

| Operation | 含义 |
|---|---|
| `push(item)` | 把 item 放到 top |
| `pop()` | 移除并返回 top item |
| `peek()` | 查看 top item，但不移除 |
| `empty()` | 判断 stack 是否为空 |

练习：

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

用 stack 反转 string 的例子：

```java
Stack word = new Stack();
```

Input：

```text
artxE eseehc esaelp
```

输出：

```text
Extra cheese please
```

Java 有 `java.util.Stack`，内置 `push`、`pop`、`peek`、`empty`。

## 8.10 Graph

Graph 的特点：

- nodes 通过 edges 连接。
- 不一定有 root。
- nodes 之间可以任意连接。

Directed graph / digraph：

- edge 有方向。
- directed edge 也叫 arc。

Undirected graph：

- edge 没有方向。

Tree 是 graph 的特殊情况：有 root、有层次结构、没有 cycle。

## 8.11 Java Collections 和 Generics

```java
LinkedList<Book> myList = new LinkedList<Book>();
ArrayList<String> myArray = new ArrayList<String>();
Stack<Integer> myStack = new Stack<Integer>();
```

Generics：

```java
LinkedList<Book> list = new LinkedList<Book>();
list.add(new Book("Java"));
Book b = list.get(0);
```

好处：type safe，取出 object 时不用 cast。

## 8.12 小结

- Dynamic data structures 用 references 连接 nodes，size 可以 runtime 改变。
- Linked list node 包含 data 和 next reference。
- 插入时先设置 `newNode.next`，再改 `current.next`。
- Queue：FIFO，`enqueue` 从 rear 进，`dequeue` 从 front 出。
- Stack：LIFO，`push` 和 `pop` 都在 top。
- Queue 练习最终：`28, 69, 27`。
- Stack 练习最终：`45, 69, 27, 99`。
- Java Collections 包括 `LinkedList`、`ArrayList`、`Stack` 等。

---

# Part 9 / 10：Trees & BST

## 9.1 Tree Terminology（树的术语）

例子：

```text
            10
           /  \
          6    15
         / \   /
        2   8 13
         \   \
          5   11
```

| Term | 含义 |
|---|---|
| root | 最顶部 node，没有 parent |
| leaf | 没有 children 的 node |
| internal node | 有一个或两个 children 的 node |
| parent / child | 上下直接相连的 nodes |
| siblings | 有同一个 parent 的 nodes |
| subtree | 某个 node 和它所有 descendants |
| ancestor | 从 root 到某 node 路径上的上层 nodes |
| descendant | 某 node 下面的 nodes |
| height of tree | root 到最远 leaf 的边数 |

## 9.2 Binary Tree

Binary tree 是每个 node 最多有两个 children 的 tree：left child 和 right child。

## 9.3 Tree Traversal

| Traversal | 顺序 | 记忆 |
|---|---|---|
| Preorder | Root -> Left -> Right | root 先 |
| Inorder | Left -> Root -> Right | root 中 |
| Postorder | Left -> Right -> Root | root 后 |

例子：

```text
        10
       /  \
      6    15
     / \   /
    2   8 13
     \   \
      5   11
```

Preorder：

```text
10, 6, 2, 5, 8, 11, 15, 13
```

Inorder：

```text
2, 5, 6, 8, 11, 10, 13, 15
```

Postorder：

```text
5, 2, 11, 8, 6, 13, 15, 10
```

注意：如果是 valid BST，Inorder traversal 会得到 ascending order。

## 9.4 Binary Search Tree

BST property：

```text
For any node N:
all values in left subtree <= N
all values in right subtree > N
```

因此，BST 的 Inorder traversal 一定是 ascending order。

记忆：

```text
BST + sorted order = inorder
```

## 9.5 BST Insertion

插入规则：

```text
if new value <= current node value -> go left
if new value > current node value -> go right
if current position is null -> insert new node there
```

## 9.6 Past Paper Q3 BST 构造

插入顺序：

```text
60, 41, 74, 16, 53, 65, 25, 46, 55, 63, 70
```

步骤：

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

正确 final BST：

```text
            60
          /    \
        41      74
       /  \     /
     16    53  65
       \   / \ /  \
       25 46 55 63 70
```

**需要复核：** 原始整理中曾把 `63` 放在 `55` 下面，但按照 insertion rule 和 traversal answers，`63` 应该是 `65` 的 left child。

## 9.7 Past Paper Q3 Traversal Answers

Q3b：哪种 traversal 得到 sorted order？

```text
Inorder traversal
```

Inorder：

```text
16, 25, 41, 46, 53, 55, 60, 63, 65, 70, 74
```

Preorder：

```text
60, 41, 16, 25, 53, 46, 55, 74, 65, 63, 70
```

Postorder：

```text
25, 16, 46, 55, 53, 41, 63, 70, 65, 74, 60
```

## 9.8 BST Deletion

删除分三种情况：

1. 删除 leaf node：直接删。
2. 删除只有一个 child 的 node：用 child 替代被删 node。
3. 删除有两个 children 的 node：用 right subtree 中最小的 node 替代。

找 right subtree 中最小值：

```java
private int findSmallestValue(Node root) {
    if (root.left == null)
        return root.value;
    else
        return findSmallestValue(root.left);
}
```

## 9.9 BST Java Implementation

Insertion：

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

Inorder traversal：

```java
public void inorder(Node root) {
    if (root == null) return;

    inorder(root.left);
    System.out.print(root.value + " ");
    inorder(root.right);
}
```

## 9.10 小结

- BST left subtree values `<=` node value。
- BST right subtree values `>` node value。
- insertion 按比较结果一直走到 `null`。
- Inorder 得到 sorted order。
- Preorder：root-left-right。
- Postorder：left-right-root。
- 删除 leaf 直接删；删除 one-child node 用 child 替代；删除 two-child node 用 right subtree minimum 替代。

---

# Part 10 / 10：Advanced Trees - AVL Tree + Heap

---

## 10A. AVL Tree

## 10.1 为什么需要 AVL Tree？

普通 BST 如果插入顺序已经 sorted，可能退化成 linked list。

例子：

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

这时 search 会从 `O(log n)` 退化成 `O(n)`。

AVL Tree 通过 rotation 保持平衡。

## 10.2 AVL Definition（AVL 定义）

AVL Tree 是 self-balancing BST。

对每个 node 都要求：

```text
height difference between left subtree and right subtree <= 1
```

高度定义：

- empty tree height = `-1`。
- single node height = `0`。

不是 AVL 的例子：

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

是 AVL 的例子：

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

## 10.3 Past Paper Q4a：Minimum Height

AVL Tree 有 `n` 个 nodes 时，minimum height：

```text
floor(log2 n)
```

等价写法：

```text
ceil(log2(n + 1)) - 1
```

常见值：

| Nodes `n` | Minimum Height |
|---|---|
| 1 | 0 |
| 2-3 | 1 |
| 4-7 | 2 |
| 8-15 | 3 |

高度为 `h` 的 binary tree 最多有：

```text
2^(h + 1) - 1
```

## 10.4 Past Paper Q4b：Operation Complexity

| Operation | Complexity |
|---|---|
| Search | `O(log n)` |
| Insert | `O(log n)` |
| Delete | `O(log n)` |

原因：AVL Tree 的 height 始终保持在 `O(log n)`。

## 10.5 AVL Rotations

插入后，如果某个 node 不平衡，就用 rotation 修复。

### Single Rotation

同侧不平衡用 single rotation：

- left-left 太高 -> right rotation。
- right-right 太高 -> left rotation。

Right-right 例子，做 left rotation：

```text
before:              after:
    a                   b
     \                 / \
      b       ->      a   Z
     / \             / \
    Y   Z           X   Y
```

具体例子：

```text
before:        after:
    5             7
     \           / \
      7    ->   5   8
     / \         \
    6   8         6
```

### Double Rotation

异侧不平衡用 double rotation：

- left-right 太高 -> 先对 left child 做 left rotation，再对当前 node 做 right rotation。
- right-left 太高 -> 先对 right child 做 right rotation，再对当前 node 做 left rotation。

记忆规则：

```text
same side -> single rotation
opposite side -> double rotation
```

表格：

| Imbalance | Rotation |
|---|---|
| left-left | single |
| right-right | single |
| left-right | double |
| right-left | double |

Right-left 例子：

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

## 10.6 Past Paper Q4c：Insert And Rotate

Sample Final 里插入 `70` 前的 AVL Tree：

```text
        60
       /  \
     20    100
           /  \
         80    120
```

按 BST insertion 规则插入 `70`：

- `70 > 60`，去 right subtree。
- `70 < 100`，去 left subtree。
- `70 < 80`，成为 `80` 的 left child。

```text
        60
       /  \
     20    100
           /  \
         80    120
        /
      70
```

此时 `60` 的 right-left side 太高，所以是 **RL case**。

处理步骤：

1. 对 `100` 做 right rotation。
2. 对 `60` 做 left rotation。

最终结果：

```text
        80
       /  \
     60    100
    / \      \
  20  70     120
```

判断逻辑：

```text
60 的 right subtree 太高
new node 70 在 60 的 right child 100 的 left subtree
=> right-left case
=> double rotation
```

考试时按下面方法处理 AVL 插入题：

1. 先按普通 BST insertion 插入新 value。
2. 从 inserted node 往上检查每个 ancestor。
3. 找到第一个 left/right subtree height difference 超过 1 的 node。
4. 判断是 LL、RR、LR、RL。
5. 按类型做 single 或 double rotation。
6. 画出最后 balanced AVL tree。

Rotation 必背规则：

```text
LL -> right single rotation
RR -> left single rotation
LR -> left rotation on child, then right rotation on node
RL -> right rotation on child, then left rotation on node
```

---

## 10B. Heap

## 10.7 什么是 Heap？

Heap 是 complete binary tree，并满足 heap property。

Complete binary tree：

- 除了最底层，每一层都填满。
- 最底层从左到右填。

Min-heap property：

```text
each node value <= each child value
```

合法 min-heap：

```text
        4
       / \
      5   7
     / \ /
    16 10 15
```

不合法 heap：

```text
        4
       / \
      15  7
     / \ /
    5  10 16
```

min-heap 的 root 永远是 minimum value。

## 10.8 Heap Operations

### Delete Minimum

步骤：

1. 把最后一个 node 的 value 复制到 root。
2. 删除最后一个 node。
3. 从 root 向下 re-heapify：和较小 child 交换，直到恢复 heap property。

例子：

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

步骤：

1. 把新 node 加到最底层最左边可用位置，也就是 array 表示里的末尾。
2. 从新 node 向上 re-heapify：如果新 value 比 parent 小，就交换。

例子：把 `4` 插入 heap `[5, 7, 15, 16, 10]`。

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

思路：

1. 把所有 numbers insert 进 heap：`O(n log n)`。
2. 重复 delete minimum：`O(n log n)`。
3. delete 出来的顺序就是 ascending order。

总 complexity：

```text
O(n log n)
```

## 10.10 Heap 的 Array Implementation

按 level-order 存入 array。

```text
        4
       / \
      5   7
     / \ /
    16 10 15

array: {4, 5, 7, 16, 10, 15}
index:  0  1  2   3   4   5
```

对 index `i` 的 node：

```text
left child index  = 2*i + 1
right child index = 2*i + 2
parent index      = (i - 1) / 2
```

## 10.11 小结

AVL Tree：

- Self-balancing BST。
- 每个 node 的 left/right subtree height difference 最多为 1。
- empty tree height = `-1`。
- single node height = `0`。
- `n` 个 nodes 的 minimum height = `floor(log2 n)`。
- Search、Insert、Delete 都是 `O(log n)`。
- same-side imbalance -> single rotation。
- opposite-side imbalance -> double rotation。

Heap：

- Complete binary tree + heap property。
- min-heap 的 root 是 minimum。
- Delete min：最后 node 替换 root，然后向下 re-heapify。
- Insert：加到末尾，然后向上 re-heapify。
- Heap Sort = `O(n log n)`。
- Array index：left `2i + 1`，right `2i + 2`，parent `(i - 1) / 2`。

---

# 最后一页速查

## Java Basics

```text
.java -> javac -> .class bytecode -> JVM executes
java.lang: String, System, Math, Object
java.util: Scanner, Random, ArrayList
java.io: FileReader, IOException
java.text: NumberFormat, DecimalFormat
String is immutable: s.concat("berry") 不改变原来的 s
primitive parameter = copy value
array parameter = copy reference
```

## Java OOP

```text
interface: 行为合同
implements: class 实现 interface
extends: class 继承 class / interface 继承 interface
super(...): 调用 parent constructor，必须是第一行
super.method(): 调用 parent method
overloading: 同一个 class，同名 method，不同 parameters
overriding: child class，同名同 parameters，改写 body
abstract class: 不能 instantiate，可以有 variables 和 implemented methods
```

## Polymorphism

```text
declared type 决定 compiler 允许调用哪些 methods
actual runtime object 决定 overridden method 的执行版本
upcasting: child -> parent，安全
downcasting: parent -> child，需要 explicit cast，有 runtime risk
```

## Exceptions

```text
try with exception: try partial -> catch -> finally -> after
try without exception: try full -> skip catch -> finally -> after
checked: must catch or throws
unchecked: RuntimeException subclasses
```

Past Paper 输出：

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
