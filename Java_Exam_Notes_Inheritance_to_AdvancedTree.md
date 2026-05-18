# ELEC 2543 Java 期末开卷考试笔记

**覆盖范围**：Lecture 14 Inheritance ~ Lecture 21 Advanced Tree Structures（重点）+ Lecture 10~13 前置概念（简要回顾）

**语言规范**：术语用英文（Class、Object、Inheritance...），解释用简体中文。

**使用说明**：本笔记按 `Part A → Part J` 顺序组织。`Part A` 是“易混淆概念专区”，应该最先翻看；遇到具体知识点再跳到对应 Part。

---

## 目录 / Table of Contents

- [Part A. 易混淆概念专区（最常出错）](#part-a-易混淆概念专区最常出错)
  - [A.1 Overriding vs Overloading（重写 vs 重载）⭐](#a1-overriding-vs-overloading重写-vs-重载-)
  - [A.2 `super` vs `this`](#a2-super-vs-this)
  - [A.3 `==` vs `equals()` vs `compareTo()`](#a3--vs-equals-vs-compareto)
  - [A.4 Checked Exception vs Unchecked Exception](#a4-checked-exception-vs-unchecked-exception)
  - [A.5 Stack (LIFO) vs Queue (FIFO)](#a5-stack-lifo-vs-queue-fifo)
  - [A.6 Preorder / Inorder / Postorder Traversal](#a6-preorder--inorder--postorder-traversal)
  - [A.7 AVL Tree 四种旋转的判定速查表](#a7-avl-tree-四种旋转的判定速查表)
  - [A.8 Upcasting vs Downcasting（向上/向下转型）](#a8-upcasting-vs-downcasting向上向下转型)
  - [A.9 Abstract Class vs Interface](#a9-abstract-class-vs-interface)
  - [A.10 Visibility Modifiers 速查](#a10-visibility-modifiers-速查)
- [Part B. 继承之前的核心概念（简要回顾）](#part-b-继承之前的核心概念简要回顾)
- [Part C. Inheritance（Lecture 14）](#part-c-inheritancelecture-14)
- [Part D. Polymorphism（Lecture 15）](#part-d-polymorphismlecture-15)
- [Part E. Exception（Lecture 16）](#part-e-exceptionlecture-16)
- [Part F. Recursion（Lecture 17）](#part-f-recursionlecture-17)
- [Part G. Sorting（Lecture 18）](#part-g-sortinglecture-18)
- [Part H. Data Structures（Lecture 19）](#part-h-data-structureslecture-19)
- [Part I. Intro to Tree（Lecture 20）](#part-i-intro-to-treelecture-20)
- [Part J. Advanced Tree Structures（Lecture 21）⭐](#part-j-advanced-tree-structureslecture-21-)

---

## Part A. 易混淆概念专区（最常出错）

### A.1 Overriding vs Overloading（重写 vs 重载）⭐

> 这是最容易混淆、出题率极高的概念，务必熟练区分。

| 对比项 | Overriding（重写） | Overloading（重载） |
|---|---|---|
| 中文 | 覆盖、改写 | 重载 |
| 发生位置 | 父类 vs 子类（**两个不同的类**） | **同一个类**（或继承结构里都可） |
| Method signature | 必须**完全相同**（方法名 + 参数列表 + 顺序 + 类型） | 方法名相同，但**参数列表必须不同**（个数 / 类型 / 顺序之一不同） |
| 返回值 | 必须相同（或子类返回类型） | 可相同可不同（不能仅靠返回值区分） |
| 关键字 | 用 `extends`；可用 `@Override` 标注 | 无特殊关键字 |
| 决定调用哪个的时机 | **Runtime**（动态绑定 / late binding，由对象实际类型决定） | **Compile time**（编译期由参数列表决定） |
| 目的 | 在子类中为相同行为定义**不同实现**（针对不同 Object type） | 在同一名字下提供**多种参数形式**（针对不同 parameters） |
| `final` 方法 | **不能**被 override | 可以 overload |
| Constructor | **不能** override（构造方法不被继承） | **可以** overload |

**Overriding 例子**：

```java
class Thought {
    public void message() {
        System.out.println("I'm a thought.");
    }
}

class Advice extends Thought {
    // Override：和父类签名完全一致
    public void message() {
        System.out.println("Take some advice.");
        super.message();   // 显式调父类版本
    }
}
```

**Overloading 例子**：

```java
class Calc {
    public int add(int a, int b) { return a + b; }
    public double add(double a, double b) { return a + b; }   // 参数类型不同
    public int add(int a, int b, int c) { return a + b + c; } // 参数个数不同
}
```

**口诀**：
- Overriding：**Same signature, different class (parent/child)** — 同名同参，跨父子类。
- Overloading：**Same name, different parameters** — 同名不同参，一个类内。

**易错点**：
- 仅返回值类型不同，**不算 overload**（编译错误）。
- 子类不能 override 父类的 `final` 方法。
- 子类**不能 override constructor**（但可以用 `super(...)` 调用父类 constructor）。

---

### A.2 `super` vs `this`

| 关键字 | 指向 | 常见用途 |
|---|---|---|
| `this` | **当前对象**自己 | `this.field` 区分同名 instance variable；`this(...)` 调用本类其他 constructor |
| `super` | **父类部分** | `super.method()` 调父类版本（即使被 override 了）；`super(...)` 调父类 constructor（**必须放在子类 constructor 的第一行**） |

```java
public class Dictionary2 extends Book2 {
    private int definitions;
    public Dictionary2(int numPages, int numDefinitions) {
        super(numPages);              // 调父类 constructor，必须第一行
        this.definitions = numDefinitions;
    }
}
```

---

### A.3 `==` vs `equals()` vs `compareTo()`

| 操作 | 比较的内容 | 返回值 | 备注 |
|---|---|---|---|
| `==` | 对于 primitive：值；对于 Object：**引用（地址）是否相同（aliases）** | `boolean` | 默认情况下两个 `new` 出来的对象 `==` 一定 `false` |
| `equals(Object)` | 默认与 `==` 行为相同；**重写后**可比较内容 | `boolean` | `String`、`Integer` 等已重写为按内容比较 |
| `compareTo(Object)` | 比较“顺序”大小（lexicographic 或自定义） | `int`：负 / 0 / 正 | 来自 `Comparable` interface |

**关键点**：
- `String s1 = new String("abc"); String s2 = new String("abc");` 时，`s1 == s2` 为 `false`，但 `s1.equals(s2)` 为 `true`。
- `Integer` 的 `==`：在 **-128 ~ 127** 之间因为 `Integer.valueOf()` 缓存，结果可能为 `true`；超出范围则为 `false`。所以 `Integer` 一定要用 `.equals()` 比较。
- `compareTo` 的返回值：`obj1.compareTo(obj2) < 0` 表示 obj1 < obj2。

---

### A.4 Checked Exception vs Unchecked Exception

| 类型 | 父类 | 必须处理? | 例子 |
|---|---|---|---|
| **Checked Exception** | `Exception`（除 `RuntimeException` 外的子类） | **必须**：要么 `try-catch`，要么在方法签名加 `throws` | `IOException`、`ClassNotFoundException`、`NoSuchMethodException` |
| **Unchecked Exception** | `RuntimeException` 及其子类 | 不强制处理 | `NullPointerException`、`ArithmeticException`、`IndexOutOfBoundsException`、`NumberFormatException`、`ArrayIndexOutOfBoundsException` |
| **Error** | `Error` | 不应被 catch | `OutOfMemoryError`、`StackOverflowError` |

**判断口诀**：是 `RuntimeException` 的后代 → unchecked；不是 → checked。

**Quick Check 答案（讲义原题）**：
- NullPointerException → **Unchecked**
- IndexOutOfBoundsException → **Unchecked**
- ClassNotFoundException → **Checked**
- NoSuchMethodException → **Checked**
- ArithmeticException → **Unchecked**

---

### A.5 Stack (LIFO) vs Queue (FIFO)

| 数据结构 | 顺序 | 操作 | 实际例子 |
|---|---|---|---|
| **Stack**（栈） | **LIFO**：Last-In, First-Out | `push` 加到顶 / `pop` 从顶移除 / `peek`(`top`) 看顶不移除 / `empty` | 一摞盘子；浏览器后退按钮 |
| **Queue**（队列） | **FIFO**：First-In, First-Out | `enqueue` 加到尾 / `dequeue`(`serve`) 从头移除 / `empty` | 银行排队；打印机任务 |

记忆：Stack 像“摞起来”只能从上面取；Queue 像“排队”先到先服务。

---

### A.6 Preorder / Inorder / Postorder Traversal

> 都是 **DFS（Depth First Search）**，区别只在“**何时访问 root**”。

| 遍历名 | 顺序 | 何时访问 root |
|---|---|---|
| **Preorder（前序）** | **Root → Left → Right** | 先访问 root |
| **Inorder（中序）** | Left → **Root** → Right | 中间访问 root |
| **Postorder（后序）** | Left → Right → **Root** | 最后访问 root |

**重要性质**：BST 的 **inorder 遍历结果一定是升序**！

**示例**（讲义原题）：

```
            10
         /      \
        6        15
       / \       /
      2   8    13
       \       /
        5     11
```

- **Preorder**：`10, 6, 2, 5, 8, 15, 13, 11`
- **Inorder**：`2, 5, 6, 8, 10, 11, 13, 15`（注意是升序，因为这是 BST）
- **Postorder**：`5, 2, 8, 6, 11, 13, 15, 10`

**第二个 Exercise 示例**：

```
        1
       / \
      2   3
     / \
    4   5
```

- Inorder（L, Root, R）：`4 2 5 1 3`
- Preorder（Root, L, R）：`1 2 4 5 3`
- Postorder（L, R, Root）：`4 5 2 3 1`

---

### A.7 AVL Tree 四种旋转的判定速查表

> 设失衡节点（balance factor 绝对值 > 1 的最深节点）为 α。

| 失衡形态 | 简称 | 旋转类型 | 操作 |
|---|---|---|---|
| Left subtree of **Left child** 太高 | **LL** | Single Rotation | 在 α 做**右单旋**（promote left child） |
| Right subtree of **Right child** 太高 | **RR** | Single Rotation | 在 α 做**左单旋**（promote right child） |
| Right subtree of **Left child** 太高 | **LR** | Double Rotation | 先在 left child 做**左旋**，再在 α 做**右旋** |
| Left subtree of **Right child** 太高 | **RL** | Double Rotation | 先在 right child 做**右旋**，再在 α 做**左旋** |

**判定步骤**：
1. 找到从根往下**第一个**失衡的节点 α。
2. 看从 α 出发往下走两步（左或右）是哪条路径，就是哪种 case。
3. 按上表选择旋转方式。

详细例子见 [Part J](#part-j-advanced-tree-structureslecture-21-)。

---

### A.8 Upcasting vs Downcasting（向上/向下转型）

设 `Holiday` 是 `Christmas` 的 superclass：

| 类型 | 写法 | 是否需要 cast | 是否合法 |
|---|---|---|---|
| **Upcasting**（向上） | `Holiday day = new Christmas();` | 不需要 | ✅ 总是合法（is-a） |
| **Downcasting**（向下） | `Christmas xmas = (Christmas) day;` | **必须显式 cast** | 需要 `day` 实际指向 `Christmas` 对象；否则 `ClassCastException` |

**编译器规则**：只看“引用类型”能调用哪些方法，不看“对象实际类型”。

```java
Holiday day = new Christmas();
day.getTree();              // 编译错误：Holiday 没有 getTree()
((Christmas) day).getTree();// OK
```

---

### A.9 Abstract Class vs Interface

| 对比项 | Abstract Class | Interface |
|---|---|---|
| 关键字 | `abstract class` / `extends` | `interface` / `implements` |
| 实例化 | **不能** | **不能** |
| 方法 | 可有 abstract 也可有非 abstract；abstract 方法必须用 `abstract` 修饰 | （讲义版本）方法都是 abstract，**不需要写 `abstract`** |
| Instance variable | 可以有任意 instance variable | **不能有 instance variable**（只能有 constant） |
| Constructor | 可以有 | 不能有 |
| 继承数量 | 单一（Java 单继承）：只能 `extends` 一个 abstract class | 多重：可 `implements` 多个 interface；interface 之间也可 `extends` 多个 |
| 设计用途 | 表示“一类事物”的通用骨架（is-a） | 表示“具备某种行为/能力”（can-do） |

**记忆**：abstract class 是“缺胳膊少腿的 class”；interface 是“一组合同”。

---

### A.10 Visibility Modifiers 速查

| Modifier | 同一个 class | 同一个 package | 子类（不同 package） | 任意 class |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| (default / package-private) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

**UML 符号**：`-` private、`#` protected、`+` public。

`protected` 的作用：让子类可以访问父类成员，但不破坏 `public` 的封装性。

---

## Part B. 继承之前的核心概念（简要回顾）

> 此部分篇幅较短；详见课件 Lecture 10~13。

### B.1 Static Modifier

- **Static variable（class variable）**：所有该 class 的 object 共享一份。
- **Static method（class method）**：通过 `ClassName.method()` 调用，无需 object；**不能访问 instance variable / instance method**（因为没有 `this`）。
- `main` 方法之所以必须是 `static`，正是因为 JVM 启动时没有任何 object。
- 典型例子：`Math.PI`、`Math.sqrt()`、`Integer.parseInt()`。

### B.2 Object Comparison

- **Float 比较**：用 `Math.abs(f1 - f2) < TOLERANCE`，不能直接 `==`。
- **Char 比较**：基于 Unicode，可用 `<`、`>` 等。
- **String 比较**：
  - `equals()` 比较内容；
  - `compareTo()` 返回负 / 0 / 正，做 **lexicographic ordering**（字典序）；
  - 大写字母在 Unicode 中**早于**小写字母（`"Great" < "fantastic"`）；
  - 短串排在同前缀长串之前（`"book" < "bookcase"`）。
- **Object 比较**：默认 `equals` 等价 `==`，必须 **override** 才能按内容比较。

### B.3 Wrapper Class

- 每个 primitive 都有对应 Wrapper：`int → Integer`，`double → Double`，`char → Character` 等。
- **Autoboxing**：`Integer obj = 42;`（int 自动包装成 Integer）。
- **Unboxing**：`int n = obj;`（Integer 自动拆箱）。
- Wrapper 对象 **immutable**。
- `Integer.parseInt(str)`、`Integer.MIN_VALUE`、`Integer.MAX_VALUE` 等常用。
- `Integer` 在 **-128~127** 之间 `==` 可能为 `true`（缓存），其他情况要用 `equals`。

### B.4 Interface 与 `Comparable`

```java
public interface Doable {
    public void doThis();           // 全是 abstract，无需写 abstract
    public int doThat();
}

public class CanDo implements Doable {
    public void doThis() { /* ... */ }
    public int doThat() { return 0; }
}
```

- Interface 不能 instantiate，但**可以作为引用类型**：
  ```java
  Doable d = new CanDo();
  ```
- 一个 class 可 `implements` 多个 interface（用逗号隔开）。
- 必须实现所有 abstract method。
- **Comparable**（Java 内置）：
  ```java
  public interface Comparable {
      int compareTo(Object other);   // 负/0/正
  }
  ```
  String 已实现 Comparable；自己写的 class 也可实现，参见 Sorting 部分。

---

## Part C. Inheritance（Lecture 14）

### C.1 概念与术语

- **Inheritance**（继承）：从一个已有 class 派生新 class 的 OOD 技术。
- **Parent class / Superclass / Base class**（父类）：被继承的 class。
- **Child class / Subclass**（子类）：派生出来的 class。
- 子类**继承**父类所有 method 和 data（即使 `private` 也是“被继承”，只是不能直接命名访问）。
- 继承表示 **is-a relationship**（Car is-a Vehicle）。
- UML 表示：子类指向父类，**空心三角形箭头**（solid line + unfilled triangular arrowhead）。
- **好处**：software reuse（软件复用）。

### C.2 `extends` 与 `protected`

```java
public class Car extends Vehicle {
    // 新增字段和方法
}
```

- Java 用 `extends` 表明继承关系。
- `protected` 修饰符：父类的 `protected` 成员**子类可访问**；同一 package 内也可访问。UML 用 `#` 表示。
- `private` 成员：子类**不能直接命名访问**，但通过父类的 `public/protected` 方法可间接访问。

### C.3 `super` 引用

- Constructor **不被继承**，但子类经常需要调用父类 constructor 来初始化父类部分。
- `super(参数)` **必须放在子类 constructor 的第一行**。
- `super.method()`、`super.variable` 可访问被遮蔽 / override 的父类成员。

**例子（讲义 Book2/Dictionary2）**：

```java
public class Book2 {
    protected int pages;
    public Book2(int numPages) { pages = numPages; }
}

public class Dictionary2 extends Book2 {
    private int definitions;
    public Dictionary2(int numPages, int numDefinitions) {
        super(numPages);              // 第一行：调父类 constructor
        definitions = numDefinitions;
    }
}
```

### C.4 Overriding Methods

- 子类用**相同签名**重新定义父类方法 → override。
- 调用哪个版本由**对象实际类型**决定（runtime / dynamic binding）。
- 父类版本可通过 `super.method()` 显式调用。
- **shadowing variables**（变量遮蔽）：子类同名变量遮蔽父类变量。**应避免**，会造成混乱。
- `final` 方法**不能** override；`final` class **不能被继承**。

讲义 Quick Check 答案：
- 子类可定义同名 method？**True**（这就是 override）
- 子类可 override 父类 constructor？**False**
- 子类不能 override `final` method？**True**
- override 是糟糕设计？**False**
- 子类可定义同名 variable？**True, but shouldn't**

### C.5 Class Hierarchy 与 Object 类

- 子类的子类构成 **class hierarchy**；同一父类的子类称为 **siblings**。
- 共有特征**尽量上移**到 hierarchy 高处。
- **`java.lang.Object`** 是所有 class 的祖先（如果没有显式 extends，默认 extends `Object`）。
- Object 提供的常用方法：
  - `toString()`：默认返回类名 + hash code；通常被 override；
  - `equals(Object)`：默认等价 `==`；
  - `clone()`：浅克隆。

### C.6 Abstract Class

```java
abstract public class HKUPerson {
    protected String name, hkid;
    public HKUPerson(String name, String hkid) {
        this.name = name; this.hkid = hkid;
    }
    public String toString() { return name + " " + hkid; }
    public abstract int charge(int base);   // abstract method
}
```

- 用 `abstract` 修饰 class → 不能 instantiate。
- Abstract method **每个**都要写 `abstract`（不同于 interface）。
- Abstract class **可以**有非 abstract method、constructor、instance variable。
- 子类必须实现所有 abstract method，否则子类也是 abstract。
- Abstract method **不能** `final`（要被 override）也**不能** `static`（无对象绑定）。
- Abstract class **不能** `final`（否则没法派生）。

### C.7 Interface Hierarchy 与 Multiple Inheritance

- Java 只支持 **single inheritance**（class 只能 extends 一个 class）。
- 但 **interface 可以多继承**（一个 interface 可 extends 多个 interface；一个 class 可 implements 多个 interface）。

```java
interface Add_Sub { void add(double x, double y); void subtract(double x, double y); }
interface Mul_Div { void multiply(double x, double y); void divide(double x, double y); }

interface Calculator extends Add_Sub, Mul_Div {
    void printResult(double result);
}

public class MyCalculator implements Calculator { /* 必须实现全部 5 个方法 */ }
```

### C.8 Indirect Visibility

- `private` 成员**确实被继承**了，只是子类不能直接命名访问。
- 子类可通过父类的 `public/protected` 方法**间接**访问 `private` 成员。

### C.9 Restricting Inheritance（`final`）

- `final` method：不能被 override。
- `final` class：不能被继承（如 `String`）。
- 因此：abstract class **不能** 是 final。

---

## Part D. Polymorphism（Lecture 15）

### D.1 Late Binding（动态绑定）

- **Polymorphism** = "having many forms"，一个引用在不同时刻可指向不同类型的 object。
- Java 中方法调用 **runtime** 才确定具体执行哪个版本，叫 **dynamic binding / late binding**。
- 所有 Java object reference **都潜在地是 polymorphic 的**。

### D.2 Polymorphism via Inheritance

- 父类引用可指向子类对象：
  ```java
  Holiday day = new Christmas();
  day.celebrate();    // 实际调 Christmas 的 celebrate（如果 override 了）
  ```
- 子类引用指向父类对象**必须显式 cast**（且对象本身必须确实是子类才安全）。
- **编译器只看引用类型**，不看对象实际类型。所以：
  ```java
  Holiday day = new Christmas();
  day.getTree();              // 编译错误，Holiday 没有 getTree
  ((Christmas) day).getTree();// OK
  ```

**Quick Check 答案**：
- `MusicPlayer mplayer = new CDPlayer();` → ✅（向上转型）
- `CDPlayer cdplayer = new MusicPlayer();` → ❌（向下要 cast，且不应这样做）

**完整例子（StaffMember 体系）**：

```
              StaffMember (abstract)
              /             \
         Volunteer         Employee
                          /        \
                      Executive   Hourly
```

```java
abstract public class StaffMember {
    protected String name, address, phone;
    public abstract double pay();           // 子类必须实现
}

public class Volunteer extends StaffMember {
    public double pay() { return 0.0; }
}

public class Employee extends StaffMember {
    protected double payRate;
    public double pay() { return payRate; }
}

public class Executive extends Employee {
    private double bonus;
    public double pay() {
        double payment = super.pay() + bonus;
        bonus = 0;
        return payment;
    }
}

// 在 Staff.payday() 中：
for (int i = 0; i < staffList.length; i++) {
    double amount = staffList[i].pay();   // polymorphic！调对应类型的 pay
}
```

### D.3 Polymorphism via Interfaces

```java
public interface Speaker {
    public void speak();
}

Speaker guest = new Philosopher();
guest.speak();                  // Philosopher 的
guest = new Dog();
guest.speak();                  // Dog 的
```

- 编译器只允许调用 interface 中声明的方法：
  ```java
  Speaker s = new Philosopher();
  s.pontificate();              // 编译错误，即使 Philosopher 有
  ```

### D.4 多态排序例子（用 Comparable）

利用 polymorphism，可以写**一个**通用排序方法处理任何 `Comparable[]`：

```java
public static void selectionSort(Comparable[] list) {
    int min;
    Comparable temp;
    for (int index = 0; index < list.length - 1; index++) {
        min = index;
        for (int scan = index + 1; scan < list.length; scan++)
            if (list[scan].compareTo(list[min]) < 0)
                min = scan;
        // swap
        temp = list[min];
        list[min] = list[index];
        list[index] = temp;
    }
}
```

只要 `Contact`、`Book`、`Person` 等 class 实现 `Comparable`（提供 `compareTo`），都能用同一个 `selectionSort` 排序。

---

## Part E. Exception（Lecture 16）

### E.1 概念

- **Exception**：描述异常或错误情况的 object。
- **Error**：通常是不可恢复的（如 `OutOfMemoryError`），**不应被 catch**。
- 程序可：忽略 / 在产生位置处理 / 在上层处理。
- 若 uncaught：JVM 打印 **call stack trace**（异常类型、所在行、调用链）并终止。

### E.2 `try-catch-finally`

```java
try {
    // 可能抛异常的代码
} catch (StringIndexOutOfBoundsException e) {
    System.out.println("Improper length: " + e.getMessage());
} catch (NumberFormatException e) {
    System.out.println("Not numeric");
} finally {
    // 无论是否抛异常都执行（常用于关闭流/连接）
}
```

- 一个 `try` 可有**多个 `catch`**（按顺序匹配第一个符合的）。
- `finally` **可选**，但**一定执行**（无论是否异常，是否被处理）。

### E.3 Exception Propagation（异常传播）

- 若当前方法没 catch，异常会**沿方法调用链向上传播**，直到被 catch 或到达 `main` 而终止。
- 例：`main → level1 → level2 → level3`，`level3` 抛异常但只 `level1` catch，则异常穿过 `level2` 传到 `level1` 处理。

### E.4 Exception Class Hierarchy

```
              Throwable
              /        \
           Error      Exception
                       /     \
            (Checked)        RuntimeException (Unchecked)
            IOException      NullPointerException
            ClassNotFound    ArithmeticException
                             IndexOutOfBoundsException
                             NumberFormatException
```

- 所有 error / exception 都派生自 `Throwable`。
- 自定义 exception：extends `Exception` 或其子类。

### E.5 Checked vs Unchecked（详见 [A.4](#a4-checked-exception-vs-unchecked-exception)）

- **Checked**：必须 catch 或 `throws`，否则编译错误。
- **Unchecked**（`RuntimeException` 及其后代 + `Error`）：可不处理。

### E.6 `throw` 与 `throws`

| 关键字 | 用法 |
|---|---|
| `throw` | **语句**，抛出一个 exception 对象：`throw new OutOfRangeException("...")` |
| `throws` | **方法签名声明**，表示该方法可能传播该异常：`public void m() throws IOException` |

```java
public static void main(String[] args) throws OutOfRangeException {
    if (value < MIN || value > MAX)
        throw new OutOfRangeException("Input value is out of range.");
}
```

**注意**：`throw` 不放在条件中 → 后面所有代码 unreachable。

### E.7 自定义 Exception

```java
public class OutOfRangeException extends Exception {
    OutOfRangeException(String message) {
        super(message);
    }
}
```

继承 `Exception` 即为 checked exception；继承 `RuntimeException` 即为 unchecked。

### E.8 I/O Exception

- **Stream**：字节流（source → destination）。
- 三个标准流：`System.in`、`System.out`、`System.err`。
- 文件 I/O 常抛 **`IOException`**（checked！必须处理）。
- `PrintWriter` 用于写文本文件，写完务必 `close()`：

```java
PrintWriter outFile = new PrintWriter("test.txt");
outFile.println("hello");
outFile.close();   // 必须关闭
```

---

## Part F. Recursion（Lecture 17）

### F.1 基本概念

- **Recursion**（递归）：定义中使用自身。
- 必须有 **Base case**（基线情况）终止递归，否则 **infinite recursion**（类似死循环）。
- **Recursive case**（递归情况）：用更小规模问题表达原问题。

### F.2 经典例子

**Factorial（阶乘）**：
```
1! = 1                  (base case)
N! = N * (N-1)!         (recursive case)
```

**Sum of 1 to N**：
```java
public int sum(int num) {
    int result;
    if (num == 1)
        result = 1;                  // base case
    else
        result = num + sum(num - 1); // recursive case
    return result;
}
```

**5 * n 的递归定义（Quick Check）**：
```
5 * 1 = 5
5 * n = 5 + (5 * (n - 1))
```

### F.3 调用机制

- 每次递归调用都建立**新的 execution environment**（新的 parameter、新的 local variable）。
- 方法返回时控制回到调用者（可能是上一层的自己）。

### F.4 Direct vs Indirect Recursion

- **Direct recursion**：m1 调 m1。
- **Indirect recursion**：m1 调 m2，m2 调 m3，m3 又调 m1。同样要 base case，但更难调试。

### F.5 Maze Traversal（迷宫遍历）

- 思路：从当前格往四个方向尝试递归。
- Base case：走到终点（成功）或越界 / 撞墙（失败）。
- 用一个 grid（值含 1 表通路、0 表墙、3 表已尝试、7 表路径上）。

### F.6 Towers of Hanoi

**规则**：3 根柱子；每次只能移 1 个盘；大盘不能放小盘上。

**递归思路**（把 n 个盘从 `start` 移到 `end`，借助 `temp`）：
1. 把上面 n-1 个盘从 start 移到 temp（借助 end）。
2. 把最大的 1 个盘从 start 直接移到 end。
3. 把 n-1 个盘从 temp 移到 end（借助 start）。

```java
private void moveTower(int numDisks, int start, int end, int temp) {
    if (numDisks == 1)
        moveOneDisk(start, end);                // base case
    else {
        moveTower(numDisks - 1, start, temp, end);
        moveOneDisk(start, end);
        moveTower(numDisks - 1, temp, end, start);
    }
}
```

移动次数 = `2^n - 1`。

---

## Part G. Sorting（Lecture 18）

### G.1 简单排序（Part I：均为 O(n²)）

> 假设要排成 **ascending order（升序）**。

#### G.1.1 Bubble Sort

**思路**：每轮从**底部往顶部**比较相邻元素，**小的“冒泡”到顶**。第 i 轮后，前 i 个元素是最小的且就位。

```java
public static void bubbleSort(int[] list) {
    for (int i = 1; i < list.length; i++)
        for (int j = list.length - 1; j >= i; j--)
            if (list[j-1] > list[j]) {
                int temp = list[j-1];
                list[j-1] = list[j];
                list[j] = temp;
            }
}
```

**讲义例子**：列表 `[5, 2, 4, 6, 1, 3]`，第一轮从底往上比较，1 一路冒到顶 → `[1, 5, 2, 4, 6, 3]`。

#### G.1.2 Insertion Sort

**思路**：维护一个**已排序区**；每轮把下一个元素**插入**到已排序区的正确位置。

```java
public static void insertionSort(int[] list) {
    for (int i = 1; i < list.length; i++) {
        int key = list[i];
        int j = i;
        while (j > 0 && list[j-1] > key) {
            list[j] = list[j-1];
            j--;
        }
        list[j] = key;
    }
}
```

#### G.1.3 Selection Sort

**思路**：第 i 轮找出剩余未排序部分的**最小元素**，与第 i 位**交换**。

```java
public static void selectionSort(int[] list) {
    for (int i = 0; i < list.length - 1; i++) {
        int minimum = i;
        for (int j = i + 1; j < list.length; j++)
            if (list[j] < list[minimum])
                minimum = j;
        int temp = list[i];
        list[i] = list[minimum];
        list[minimum] = temp;
    }
}
```

**Swap 三件事**（任何排序都需要）：
```java
temp = first;
first = second;
second = temp;
```

### G.2 Big-O 复杂度

- **运行时间**：操作数关于 input size `n` 的函数。
- **Worst / Average / Best case**：最差 / 平均 / 最好情况。最常用 worst case。
- **Asymptotic analysis（渐近分析）**：忽略机器常数和低阶项。
- **Big-O 定义**：`f(n) = O(g(n))` 若存在常数 `c, n0` 使 `0 ≤ f(n) ≤ c·g(n)` 对所有 `n ≥ n0`。
- 例：`3n³ + 90n² - 2n + 5 = O(n³)`。
- 用**最紧的**：`2n² = O(n²)`（也可写成 O(n³)，但不写）；**不能**写 `2n² = O(n)`。

### G.3 递归排序（Part II：O(n log n)）

#### G.3.1 Merge Sort

**Divide-and-conquer 思路**：
1. **Divide**：把数组一分为二。
2. **Conquer**：递归排序两半。
3. **Combine**：把两个有序半合并成一个有序数组。

**示例**：
```
        [5, 7, 3, 1, 6, 4, 2, 8]
              divide
   [5, 7, 3, 1]       [6, 4, 2, 8]
       divide              divide
  [5, 7]  [3, 1]      [6, 4]  [2, 8]
       sort                  sort
  [5, 7]  [1, 3]      [4, 6]  [2, 8]
       merge                 merge
   [1, 3, 5, 7]       [2, 4, 6, 8]
              merge
   [1, 2, 3, 4, 5, 6, 7, 8]
```

**关键代码（合并阶段）**：
```java
while (iFirst < first.length && iSecond < second.length) {
    if (first[iFirst] <= second[iSecond])
        list[i++] = first[iFirst++];
    else
        list[i++] = second[iSecond++];
}
while (iFirst  < first.length)  list[i++] = first[iFirst++];
while (iSecond < second.length) list[i++] = second[iSecond++];
```

**复杂度推导**：
- `T(1) = c`
- `T(n) = 2T(n/2) + kn`（合并花 `kn`）
- 展开：`T(n) = kn·log₂n + cn = O(n log n)`

#### G.3.2 Quick Sort

**思路**：
1. 选一个 **pivot**（讲义版本：取第一个元素）。
2. **Partition**：把数组分成 `≤ pivot` 和 `> pivot` 两部分。
3. 递归排序两个子列表。

**Partition 步骤**（讲义示意）：
- 左指针从左向右扫，找到 **> pivot** 就停。
- 右指针从右向左扫，找到 **≤ pivot** 就停。
- 若左指针仍在右指针左边，交换并各自前进 / 后退一步。
- 重复直到左 ≥ 右，再把 pivot 放到正确位置。

**复杂度**：
- **Best case**：pivot 每次都把数组对半分 → `T(n) = 2T(n/2) + O(n)` → **O(n log n)**。
- **Worst case**：pivot 总是最大或最小（如对已排序数组取第一个为 pivot） → `T(n) = T(n-1) + O(n) = O(n²)`。

**In-place sorting**：排序就在原数组进行，不开新数组。Quick Sort 是 in-place。

### G.4 算法复杂度对比

| 算法 | Best | Average | Worst | 特点 |
|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | 简单，慢 |
| Insertion Sort | O(n) | O(n²) | O(n²) | 简单，对近乎有序的数据快 |
| Selection Sort | O(n²) | O(n²) | O(n²) | 简单，交换次数少 |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | 稳定，需额外空间 |
| Quick Sort | O(n log n) | O(n log n) | **O(n²)** | in-place，平均最快 |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | 见 [Part J](#part-j-advanced-tree-structureslecture-21-) |

---

## Part H. Data Structures（Lecture 19）

### H.1 ADT 概念

- **Abstract Data Type（ADT）**：一组数据 + 一组操作。
- Java 内置 ADT 接口：`List`、`Stack`、`Queue`、`Set`、`Map`。
- 常见数据结构：Array、Linked List、Stack、Queue、Binary Tree、BST、Heap、Hashing、Graph。

### H.2 Static vs Dynamic Structures

| 类型 | 大小 | 例子 |
|---|---|---|
| **Static** | 固定 | Array |
| **Dynamic** | 运行时增减 | Linked List, Tree, Graph |

注意：这里的 "static" 与 `static` modifier **意思不同**！

### H.3 Object Reference as Link

```java
class Node {
    int info;
    Node next;        // 指向同类的引用 → 形成链
}
```

- Reference 也称 **pointer**。
- 链式结构通过 reference 串起来。

### H.4 Singly Linked List

**结构**：一系列 Node，每个 Node 含 data 和 `next` 引用。

**插入 newNode 到 current 之后（Quick Check 1）**：
```java
newNode.next = current.next;
current.next = newNode;
```
**顺序很重要**：先把 newNode 的 next 设好，再断开 current。否则会丢失链表后半段。

**删除 current 之后的节点（Quick Check 2）**：
```java
if (current.next != null)
    current.next = current.next.next;
```

### H.5 Doubly Linked List

每个 Node 多一个 `prev` 引用。

**删除 current 之后的节点（Quick Check 3）**：
```java
if (current.next != null) {
    current.next = current.next.next;
    if (current.next != null)
        current.next.prev = current;
}
```

### H.6 Queue（FIFO）

- **Enqueue**：rear 加入；**Dequeue / Serve**：front 移除。
- **Empty**：判空。

**实现方式**：
- **Linked List**：references 从 front 指向 rear 最高效。
- **Array**：用 `%` 实现 circular buffer（环形缓冲）。

**练习**：
```
初始：空
enqueue(45);  → [45]
enqueue(12);  → [45, 12]
enqueue(28);  → [45, 12, 28]
dequeue();    → [12, 28]
dequeue();    → [28]
enqueue(69);  → [28, 69]
enqueue(27);  → [28, 69, 27]
```
**答案**：`(head) 28, 69, 27`

### H.7 Stack（LIFO）

- **Push**：顶部加入；**Pop**：顶部移除；**Peek / Top**：看顶不动；**Empty**。
- 实现可用 Linked List（首节点为栈顶）或 Array（索引 0 为栈底）。
- Java 标准库有 `java.util.Stack`。

**练习**：
```
初始：空
push(45);   → [45]
push(12);   → [45, 12]
push(28);   → [45, 12, 28]
pop();      → [45, 12]
pop();      → [45]
push(69);   → [45, 69]
push(27);   → [45, 69, 27]
push(99);   → [45, 69, 27, 99]
```
**答案**：`(bottom) 45, 69, 27, 99`

### H.8 Tree / Graph（非线性）

- **Tree**：root + 多层节点，hierarchy 结构。
  - **Leaf node**：无子节点。
  - **Internal node**：非 root 非 leaf。
  - **Binary tree**：每个节点至多 2 个 child。
- **Graph**：无 root；任意两节点间可有 edge。
  - **Digraph（directed graph）**：edge 有方向，又称 **arc**。

### H.9 Generics

```java
LinkedList<Book> myList = new LinkedList<Book>();
```

- 编译期类型检查；
- 取出元素时已知类型，无需 cast。

---

## Part I. Intro to Tree（Lecture 20）

### I.1 Tree 术语

- **Tree**：节点 + 边的非线性结构；**任两节点恰有一条路径**。
- N 个节点的 tree 恰有 **N - 1 条边**。
- **Binary Tree**：每个节点至多 2 个 child（left, right）。

### I.2 Binary Tree 术语

| 术语 | 解释 |
|---|---|
| Root | 顶端节点 |
| Internal node | 至少有一个 child 的节点 |
| Leaf | 无 child 的节点 |
| Parent | 上一层节点 |
| Sibling | 共父节点 |
| Subtree | 某节点及其所有后代构成的子树 |
| Ancestor of a | 从 a 到 root 路径上的节点 |
| Descendant of a | 在 a 的 subtree 中的节点 |
| **Height of tree** | **root 到叶子最长路径上的边数** |

> 注：空树的 height 在 AVL 定义中规定为 **-1**。

### I.3 Tree Traversal

详见 [A.6](#a6-preorder--inorder--postorder-traversal)。

```java
void preorder(Node n)  { if(n==null)return; visit(n); preorder(n.left);  preorder(n.right); }
void inorder(Node n)   { if(n==null)return; inorder(n.left);  visit(n);  inorder(n.right); }
void postorder(Node n) { if(n==null)return; postorder(n.left); postorder(n.right); visit(n); }
```

### I.4 Binary Search Tree（BST / Ordered Binary Tree）

**性质**（讲义版本）：
- 对每个节点 N，**左子树**所有节点值 **≤ N**；
- 对每个节点 N，**右子树**所有节点值 **> N**；
- → **inorder 遍历给出 ascending order**。

### I.5 BST Insertion（插入）

**规则**（讲义原文）：
- 若 root 为 null，新元素作 root。
- 若新值 **≤ root**，插入到左子树（递归）；若左子树空，作 left child。
- 若新值 **> root**，插入到右子树（递归）；若右子树空，作 right child。

**例子（讲义）**：依次插入 6, 3, 4, 9, 10：

```
add(6):                 6

add(3):  3 ≤ 6，去左    6
                       /
                      3

add(4):  4 ≤ 6 → 左;   6
         4 > 3 → 右   /
                     3
                      \
                       4

add(9):  9 > 6 → 右    6
                      / \
                     3   9
                      \
                       4

add(10): 10 > 6 → 右   6
         10 > 9 → 右  / \
                     3   9
                      \   \
                       4   10
```

### I.6 BST Deletion（删除）⭐ 三种情况

**情况 1：要删的节点是 leaf**
- 直接移除。

**情况 2：要删的节点只有一个 child**
- **Promote**（提升）那个 child 取代被删节点的位置。

**情况 3：要删的节点有两个 child**
- 找**右子树**中**最左**节点（即 right subtree 的最小值，也是 inorder successor），用它的值替换被删节点的值，然后从原位置删除那个最左节点。

**讲义例子**：

初始树：
```
            10
         /      \
        6        15
       / \       /
      2   8    13
       \       /
        5    11
```

**Delete 5（leaf，情况 1）**：直接删，得到：
```
            10
         /      \
        6        15
       / \       /
      2   8    13
              /
             11
```

**Delete 15（一个 child，情况 2）**：13 顶上去：
```
            10
         /      \
        6        13
       / \       /
      2   8    11
```

**Delete 10（两个 child，情况 3）**：右子树最左节点是 11，11 替换 10 的位置；11 原位置删除：
```
            11
         /      \
        6        13
       / \
      2   8
```

> 提示：讲义对应幻灯片上把 11 既画在新位置又留在旧位置，是排版上的一个 typo；**正确做法**是从旧位置移除。

### I.7 Java 实现（递归插入示意）

```java
// 找右子树中的最小值（用于 deletion 情况 3）
private int findSmallestValue(Node root) {
    if (root.left == null) return root.value;
    else return findSmallestValue(root.left);
}
```

---

## Part J. Advanced Tree Structures（Lecture 21）⭐

> 本部分是考试重点中的重点。AVL 旋转尤其要会做。

### J.1 Balanced Tree

- **Balance**：left 与 right child 的 height 相近。
- Height 为 `O(log₂ n)` 时，search / insertion worst case 都是 `O(log n)`。
- AVL 是一种自平衡 BST。

### J.2 AVL Tree 定义

- AVL Tree = **self-balancing BST**。
- 每个节点的 **left 与 right 子树 height 之差 ≤ 1**（即 `|BF| ≤ 1`，`BF = h_left - h_right`）。
- **空树的 height 规定为 -1**（叶节点的 height 为 0）。

**例子（讲义）**：

```
       8                   是 AVL Tree
      / \
     2   9
        / \
       4   ?
```

```
       3                 不是 AVL Tree（某处 |BF| ≥ 2）
```

### J.3 AVL Insertion 流程

1. 像普通 BST 一样按顺序插入新节点。
2. **从插入点向上**回溯，检查每个祖先的 balance factor。
3. 找到从下往上**第一个**失衡节点 α（|BF| > 1）。
4. 根据失衡形态选择**四种旋转之一**修复。
5. 修复一次即可恢复整棵树平衡（不需要继续往上调整）。

### J.4 四种旋转（完整例子）⭐⭐⭐

> 设失衡节点为 α。

#### J.4.1 Case 1：LL（左左）— Single Right Rotation at α

**触发条件**：插入到 α 的 **left child** 的 **left subtree**。

**通用图示**：
```
       α                          b
      / \                        / \
     b   Z      右旋(α)         X   α
    / \           ───>             / \
   X   Y                          Y   Z
   ↑
   插入这里
```

**具体例子**：依次插入 3, 2, 1

```
Step 1：insert 3
   3

Step 2：insert 2（2 < 3，往左）
     3
    /
   2          h(left)=0, h(right)=-1, BF(3) = 0 - (-1) = 1，仍平衡

Step 3：insert 1（1 < 3 → 左; 1 < 2 → 左）
       3       (失衡！)        ← α = 3，左左
      /        h_left=1, h_right=-1, BF=2
     2
    /
   1

判定：α=3 的 left child（2）的 left subtree 太高 → LL → 在 3 右单旋

右单旋：把 2 提为新 root，3 变成 2 的 right child
       2
      / \
     1   3
```

#### J.4.2 Case 2：RR（右右）— Single Left Rotation at α

**触发条件**：插入到 α 的 **right child** 的 **right subtree**。

**通用图示**：
```
     α                            b
    / \                          / \
   X   b        左旋(α)         α   Z
      / \         ───>          / \
     Y   Z                     X   Y
         ↑
         插入这里
```

**具体例子**：依次插入 1, 2, 3

```
Step 1：insert 1
   1

Step 2：insert 2（2 > 1，往右）
   1
    \
     2

Step 3：insert 3（3 > 1 → 右; 3 > 2 → 右）
   1       (失衡！)             ← α = 1，右右
    \      BF = -1 -1 = -2
     2
      \
       3

判定：α=1 的 right child（2）的 right subtree 太高 → RR → 在 1 左单旋

左单旋：把 2 提为新 root，1 变成 2 的 left child
       2
      / \
     1   3
```

#### J.4.3 Case 3：LR（左右）— Double Rotation（先 left child 左旋，再 α 右旋）

**触发条件**：插入到 α 的 **left child** 的 **right subtree**。

**通用图示**：
```
        α                           α                          c
       / \      左旋(b)             / \      右旋(α)           / \
      b   Z      ──>               c   Z      ──>            b   α
     / \                          / \                       / \  / \
    W   c                        b  Y2                     W  Y1 Y2 Z
       / \                      / \
      Y1 Y2                    W  Y1
```

**具体例子**：依次插入 3, 1, 2

```
Step 1：insert 3
   3

Step 2：insert 1
     3
    /
   1

Step 3：insert 2（2 < 3 → 左; 2 > 1 → 右）
       3       (失衡！)              ← α = 3
      /        h(left)=1, h(right)=-1, BF=2
     1
      \
       2

判定：α=3 的 left child（1）的 right subtree 太高 → LR → 双旋

Step A（在 left child 1 处左旋）：把 2 提到 1 的位置，1 变 2 的左
       3
      /
     2
    /
   1

Step B（在 α=3 处右旋）：把 2 提到 3 的位置
       2
      / \
     1   3
```

#### J.4.4 Case 4：RL（右左）— Double Rotation（先 right child 右旋，再 α 左旋）

**触发条件**：插入到 α 的 **right child** 的 **left subtree**。

**通用图示**：
```
     α                            α                            c
    / \         右旋(b)          / \          左旋(α)         / \
   W   b         ──>            W   c          ──>           α   b
      / \                          / \                      / \  / \
     c   Z                        Y1  b                    W  Y1 Y2 Z
    / \                              / \
   Y1 Y2                            Y2  Z
```

**具体例子**：依次插入 1, 3, 2

```
Step 1：insert 1
   1

Step 2：insert 3
   1
    \
     3

Step 3：insert 2（2 > 1 → 右; 2 < 3 → 左）
   1       (失衡！)                  ← α = 1
    \      BF = -1 - 1 = -2
     3
    /
   2

判定：α=1 的 right child（3）的 left subtree 太高 → RL → 双旋

Step A（在 right child 3 处右旋）：把 2 提到 3 的位置，3 变 2 的右
   1
    \
     2
      \
       3

Step B（在 α=1 处左旋）：把 2 提到 1 的位置
       2
      / \
     1   3
```

#### J.4.5 综合大例子（讲义版本，LR Case）

初始 AVL Tree（已平衡）：

```
            8
           / \
          4   9
         / \
        2   5
```

插入 **6**（6 < 8 → 左；6 > 4 → 右；6 > 5 → 右）：

```
            8       ← 失衡！α = 8
           / \      h_left=3, h_right=1, BF=2
          4   9
         / \
        2   5
             \
              6
```

**判定**：α=8 的 left child（4）的 right subtree（包含 5、6）太高 → **LR Case** → 双旋。

**Step A：在 left child（4）处左旋**

左旋(4)：把 4 的 right child（5）提为新 root，4 变成 5 的 left，5 原来的 left（无）变成 4 的 right。

```
            8
           / \
          5   9
         / \
        4   6
       /
      2
```

**Step B：在 α=8 处右旋**

右旋(8)：把 8 的 left child（5）提为新 root，8 变成 5 的 right，5 原来的 right（6）变成 8 的 left。

```
            5
           / \
          4   8
         /   / \
        2   6   9
```

平衡恢复。

#### J.4.6 单旋 / 双旋判定速查（再次复述）

| 失衡形态（α 出发往下两步） | Case | 操作 |
|---|---|---|
| Left – Left | LL | 在 α 右单旋（promote left child） |
| Right – Right | RR | 在 α 左单旋（promote right child） |
| Left – Right | LR | 先在 left child 左旋，再在 α 右旋 |
| Right – Left | RL | 先在 right child 右旋，再在 α 左旋 |

**速记**：
- 同向（LL/RR）→ 单旋；异向（LR/RL）→ 双旋。
- 双旋的第一步：在**中间那个孩子**处做一个**反向**旋转，把局面变成同向后，再做单旋。

---

### J.5 Heap

#### J.5.1 定义

- **Heap = Complete Binary Tree**：每层填满，**最底层从左向右排满**（可不满但必须靠左）。
- **Heap Property**（讲义讲的是 **min-heap**）：每个节点的值 **≤ 其 child 的值**。
- 所以 **root 永远是最小值**。

**例子**：

```
     4              是 heap（complete + min-heap property）
    / \
   5   7
  / \  /
 16 10 15
```

```
     4              不是 heap（虽然 complete，但 4 < 15，5 在子树中 < 4）
    / \             违反 heap property
   15  7
  / \  /
 5  10 16
```

#### J.5.2 性质

- 访问 root（最小值）：**O(1)**。
- Insertion：**O(log₂n)**。
- Removing minimum：**O(log₂n)**。

#### J.5.3 Removing the Minimum

1. 取走 root（最小值）。
2. **把最后一个节点的值复制到 root 位置**，删除原最后节点。
3. **Re-heapify**（下沉 / sift down）：
   - 比较当前节点与其 children；
   - 若 children 中有比它小的，与**更小的那个 child** 交换；
   - 继续向下，直到当前节点 ≤ 两 children 或到叶。

**讲义例子**：删除 4

```
初始：             把 15（最后一个节点）复制到 root：
      4                       15
     / \                      / \
    5   7                    5   7
   / \  /                   / \  /
  16 10 15                 16 10
                           （原 15 节点删除）

15 > 5 且 15 > 7，与较小者 5 交换 → 下沉到 5 的位置：

       5
      / \
     15  7
    / \
   16  10

15 > 10 但 15 < 16，与较小者 10 交换：

       5
      / \
     10  7
    / \
   16  15

满足 heap property，结束。
```

#### J.5.4 Adding a New Number

1. 把新值放到**最底层最左空位**（保持 complete）。
2. **Re-heapify**（上浮 / sift up）：
   - 比较与 parent；
   - 若比 parent 小，交换；
   - 继续向上，直到 ≥ parent 或到 root。

**讲义例子 1**：往如下 heap 插入 5

```
初始：             插入 5 到最右下空位：
     7                       7
    / \                     / \
   10  15                  10  15
  /                       / \
 16                      16  5

5 < 10，上浮交换：

      7
     / \
    5   15
   / \
  16 10

5 < 7，上浮交换：

      5
     / \
    7   15
   / \
  16 10
```

**讲义例子 2**：在上图基础上再插入 4

```
插入 4 到最右下空位：
      5
     / \
    7   15
   / \ /
  16 10 4

4 < 15，上浮：

      5
     / \
    7   4
   / \ /
  16 10 15

4 < 5，上浮：

      4
     / \
    7   5
   / \ /
  16 10 15
```

#### J.5.5 Heap Sort

**思路**：
1. 把全部元素逐个 insert 到 heap，O(n log n)。
2. 逐个 remove min，依次得到升序输出，O(n log n)。
3. **总时间 O(n log n)**。

#### J.5.6 Array Implementation

把 heap 按 level-order（从上到下、从左到右）存到 array：

```
     4                array: [4, 5, 7, 16, 10, 15]
    / \                       0  1  2  3   4   5
   5   7
  / \  /
 16 10 15
```

**索引关系**（设根 index 为 0）：

| 关系 | 公式 |
|---|---|
| `arr[i]` 的 **left child** | `arr[2*i + 1]` |
| `arr[i]` 的 **right child** | `arr[2*i + 2]` |
| `arr[i]` 的 **parent** | `arr[(i - 1) / 2]` |

数组实现的好处：sift up/down 直接做 index 算术，swap 高效，不需要 reference。

---

## 附：常见考试问句速对照

| 问题 | 简短答案 |
|---|---|
| What is overriding? | 子类用相同 signature 重新实现父类方法，runtime 决定调用版本。 |
| What is overloading? | 同名但参数列表不同的多个方法，compile time 决定调用版本。 |
| What is polymorphism? | 一个引用在不同时刻可指向不同类型的对象，由 dynamic binding 决定实际调用的方法。 |
| Difference between abstract class and interface? | 见 [A.9](#a9-abstract-class-vs-interface)。 |
| What is FIFO / LIFO? | FIFO = Queue（先进先出）；LIFO = Stack（后进先出）。 |
| 写出 BST 的 inorder/preorder/postorder | 见 [A.6](#a6-preorder--inorder--postorder-traversal)。 |
| AVL 怎么旋转？ | 见 [J.4](#j4-四种旋转完整例子)。 |
| Time complexity of Quick Sort worst case? | O(n²)。 |
| Time complexity of Merge Sort? | O(n log n)（任何情况）。 |
| Heap 的根是什么？ | min-heap 的 root 是最小值（O(1) 访问）。 |
| Checked exception 不处理会怎样？ | 编译错误。 |
| `finally` 何时执行？ | 无论是否抛异常都执行。 |

---

> **祝考试顺利！** 本笔记内容已逐项对照讲义校对；如发现具体题目与笔记冲突，**以讲义为准**。
