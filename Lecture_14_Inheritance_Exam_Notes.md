# Lecture 14: Inheritance - Exam Notes

> 范围：`Lecture/14_Inheritance.pdf`，主要是 `inheritance`、`protected`、`super`、`overriding`、`class hierarchy`、`abstract class`、visibility 和 inheritance design。
> 读法：先看第 0 节速查，再重点看第 2 节 `super`、第 3 节 `overriding`、第 4 节 `abstract class`。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Inheritance 必背

| 问题 | 答案 |
|---|---|
| `inheritance` 是什么？ | 从 existing class derive new class，让 child class 继承 parent class 的 data 和 methods |
| parent class 还叫什么？ | `superclass` / `base class` |
| child class 还叫什么？ | `subclass` / `derived class` |
| Java 用什么关键字建立 inheritance？ | `extends` |
| 正确的 inheritance 关系是什么？ | `is-a relationship`，例如 `Dictionary is a Book` |
| Java 支持多重 class inheritance 吗？ | 不支持，一个 class 只能 `extends` 一个 parent class |
| constructor 会被继承吗？ | 不会 |
| child constructor 如何调用 parent constructor？ | 用 `super(...)`，而且通常必须放在 child constructor 第一行 |
| `private` member 会被继承吗？ | 会存在于 child object 中，但 child class 不能直接用名字访问 |
| 如何间接访问 inherited private member？ | 通过 parent class 提供的 public/protected methods |
| `protected` 的意义 | child class 可访问；同 package 也可访问；比 `public` 更封装，比 `private` 更开放 |
| `overriding` 条件 | child method 和 parent method 有相同 method signature |
| `final method` 可以 override 吗？ | 不可以 |
| `final class` 可以被继承吗？ | 不可以 |
| `abstract class` 可以 instantiate 吗？ | 不可以 |
| abstract method 可以是 `final` 或 `static` 吗？ | 不可以 |
| 所有 Java class 的最终 root 是谁？ | `Object` |

### 关键词区别

| 概念 | 考试判断 |
|---|---|
| `overloading` | same class 中 same method name，但 parameter list 不同 |
| `overriding` | parent/child 中 same method signature，child 改写 behavior |
| `shadowing variables` | child 重新定义 parent 同名 variable，合法但非常容易混乱，应该避免 |
| `super(...)` | 调用 parent constructor |
| `super.method()` | 显式调用 parent version 的 method |
| `this` | 当前 object |
| `super` | 当前 object 里的 parent part |

---

## 1. Inheritance 基本概念

### 1.1 为什么需要 inheritance？

`Inheritance` 是 object-oriented design 的核心技术之一。它的目的不是单纯“少写代码”，而是：

1. 复用已经设计、实现、测试过的 class。
2. 建立一组 classes 之间的 logical hierarchy。
3. 把 common features 放在 parent class。
4. 让 child class 只负责更 specific 的 data 和 behavior。

最重要的一句话：

```text
Inheritance should model an is-a relationship.
```

例子：

```text
Car is a Vehicle      -> good inheritance
Dictionary is a Book  -> good inheritance
Student is a Person   -> good inheritance
```

不应该用 inheritance 表示 `has-a`：

```text
Car has an Engine
```

这更适合用 instance variable：

```java
class Car {
    private Engine engine;
}
```

### 1.2 Java 语法：`extends`

Java 用 `extends` 建立 inheritance：

```java
public class Car extends Vehicle {
    // class contents
}
```

含义：

- `Vehicle` 是 parent class / superclass / base class。
- `Car` 是 child class / subclass / derived class。
- `Car` object 拥有 `Vehicle` 中继承下来的 members。
- `Car` 可以新增 variables/methods，也可以 override inherited methods。

### 1.3 UML class diagram 记法

Inheritance 在 UML class diagram 中用：

```text
ChildClass  ------|>  ParentClass
```

实心线 + 空心三角箭头，箭头指向 parent class。

例子：

```text
Dictionary  ------|>  Book
```

读作：

```text
Dictionary is a Book.
```

---

## 2. Creating Subclasses

### 2.1 `Book` / `Dictionary` 例子的结构

Lecture 里的基本例子：

```text
Book
  - pages
  - setPages(...)
  - getPages()

Dictionary extends Book
  - definitions
  - computeRatio()
  - setDefinitions(...)
  - getDefinitions()
```

直觉：

- `Book` 管理一本书都有的东西：pages。
- `Dictionary` 是一种更具体的 `Book`，额外有 definitions。
- 所以 `Dictionary` 可以直接使用 inherited `getPages()`。

简化版：

```java
class Book {
    protected int pages = 1500;

    public int getPages() {
        return pages;
    }
}

class Dictionary extends Book {
    private int definitions = 52500;

    public double computeRatio() {
        return (double) definitions / pages;
    }
}
```

如果写：

```java
Dictionary webster = new Dictionary();
System.out.println(webster.getPages());
System.out.println(webster.computeRatio());
```

`getPages()` 来自 parent class，`computeRatio()` 来自 child class。

### 2.2 `protected` modifier

Visibility 对 inheritance 很重要。

| Modifier | Same class | Same package | Child class | Other package non-child |
|---|---|---|---|---|
| `private` | yes | no | no direct access | no |
| package default | yes | yes | only if same package | no |
| `protected` | yes | yes | yes | no direct access unless through inheritance rules |
| `public` | yes | yes | yes | yes |

课件重点：

```text
private variables/methods cannot be referenced by name in a child class.
protected allows child class to reference inherited variables/methods.
```

为什么不用 `public` variable？

```text
public variable breaks encapsulation.
```

所以 inheritance 中常见选择：

- data 仍然尽量 `private`。
- 如果 child class 真的需要直接访问，可以用 `protected`。
- 更好的设计通常是：data `private` + 提供 protected/public method 控制访问。

### 2.3 Constructors are not inherited

最容易考的一句：

```text
Constructors are not inherited.
```

即使 parent constructor 是 `public`，child class 也不会自动继承一个同样 signature 的 constructor。

但是 child object 里面包含 parent part，所以创建 child object 时必须先初始化 parent part。

### 2.4 `super(...)` 调用 parent constructor

`super` 可以指向 parent class 的部分。

典型例子：

```java
class Book2 {
    protected int pages;

    public Book2(int numPages) {
        pages = numPages;
    }
}

class Dictionary2 extends Book2 {
    private int definitions;

    public Dictionary2(int numPages, int numDefinitions) {
        super(numPages);
        definitions = numDefinitions;
    }
}
```

关键规则：

```text
The first line of a child constructor should call the parent's constructor.
```

实际 Java 规则更准确地说：

- 如果你没有写 `super(...)`，compiler 会尝试自动插入 `super()`。
- 如果 parent class 没有 no-argument constructor，而你又没写 `super(args)`，会 compile error。
- `super(...)` 必须是 constructor body 里的第一条 statement。

### 2.5 `super.method()` 调用 parent method

`super` 不只用于 constructor，也可以调用 parent version 的 method：

```java
super.message();
```

用途：

- child override 了一个 method。
- 但 child method 里面仍然想复用 parent method 的一部分 behavior。

---

## 3. Overriding Methods

### 3.1 Overriding 是什么？

`Overriding`：

```text
A child class provides its own version of an inherited method.
```

条件：

```text
same method signature
```

也就是：

- method name 相同。
- parameter list 相同。
- 通常 return type 也要 compatible。

例子：

```java
class Thought {
    public void message() {
        System.out.println("Parent version");
    }
}

class Advice extends Thought {
    public void message() {
        System.out.println("Child version");
        super.message();
    }
}
```

如果执行：

```java
Thought t = new Thought();
Advice a = new Advice();
t.message();
a.message();
```

结果：

```text
Parent version
Child version
Parent version
```

因为 `Advice.message()` override 了 `Thought.message()`，但它内部又显式调用了 `super.message()`。

### 3.2 Object type 决定执行哪个 method

Lecture 14 的 wording：

```text
The type of the object executing the method determines which version of the method is invoked.
```

直觉：

- 如果 object 真正是 `Thought`，调用 `Thought.message()`。
- 如果 object 真正是 `Advice`，调用 `Advice.message()`。

这个概念会在 Lecture 15 的 `polymorphism` 里继续出现。

### 3.3 `final` method 不能 override

```java
class Parent {
    public final void lockBehavior() {
        // cannot be overridden
    }
}
```

如果 child class 试图写同 signature method，会 compile error。

考试判断：

```text
A child class cannot override a final method of the parent class.
```

答案：True。

### 3.4 Overloading vs Overriding

| 对比 | `overloading` | `overriding` |
|---|---|---|
| 发生在哪里 | usually same class | parent-child classes |
| method name | same | same |
| parameter list | different | same |
| 目的 | 同名操作适配不同参数 | child 改写 inherited behavior |
| binding 重点 | compile-time signature selection | runtime object type matters |

例子：Overloading

```java
void print(int x) { }
void print(String s) { }
```

例子：Overriding

```java
class Animal {
    void speak() { }
}

class Dog extends Animal {
    void speak() { }
}
```

### 3.5 Shadowing variables

`Shadowing variables` 是指 child class 定义了和 parent class 同名的 variable。

这在 Java 里可以发生，但课件强调：

```text
Shadowing variables should be avoided.
```

原因：

- 读代码时很难判断当前用的是 parent variable 还是 child variable。
- 容易制造 subtle bug。
- 通常违反清晰的 class hierarchy 设计。

---

## 4. Class Hierarchies

### 4.1 多层 inheritance

一个 child class 也可以成为另一个 class 的 parent：

```text
StaffMember
  -> Employee
       -> Executive
       -> Hourly
  -> Volunteer
```

规则：

```text
A child class inherits from all its ancestor classes.
```

也就是说 `Executive` 不只继承 `Employee`，也继承 `StaffMember` 中可继承的 members。

### 4.2 Siblings

两个 classes 有同一个 parent，就叫 siblings。

例子：

```text
Executive and Hourly are siblings if both extend Employee.
```

### 4.3 Common features should be high in hierarchy

设计原则：

```text
Common features should be put as high in the hierarchy as is reasonable.
```

例子：

- `name`, `address`, `phone` 应该放在 `StaffMember`。
- `socialSecurityNumber`, `payRate` 应该放在 `Employee`，因为 `Volunteer` 不需要。
- `bonus` 只属于 `Executive`，不应放在 `Employee`。

这能减少 duplication，也让 hierarchy 的 meaning 更清晰。

### 4.4 `Object` class

Java 所有 classes 最终都来自：

```java
java.lang.Object
```

如果你写：

```java
class Book {
}
```

没有显式 `extends`，Java 会把它当成：

```java
class Book extends Object {
}
```

`Object` 提供一些所有 class 都有的 inherited methods，例如：

| Method | 作用 |
|---|---|
| `toString()` | 返回 object 的 string representation |
| `equals(Object obj)` | 默认判断两个 references 是否 aliases |
| `clone()` | 复制 object，实际使用有额外限制 |

考试重点：

```text
Every time we define toString(), we are overriding Object.toString().
```

### 4.5 `equals` 默认和 override

`Object.equals` 默认做的是 reference equality：

```text
true only if two references are aliases of the same object
```

但是很多 classes 会 override 它。

例子：

```java
String a = new String("HKU");
String b = new String("HKU");
```

如果用 `a == b`：

```text
false, because they are different objects
```

如果用 `a.equals(b)`：

```text
true, because String overrides equals to compare characters
```

---

## 5. Abstract Classes

### 5.1 Abstract class 是什么？

`Abstract class` 是 class hierarchy 中的 generic placeholder。

它代表一个太 general、不适合直接创建 object 的概念。

例子：

```java
public abstract class Product {
    // common product members
}
```

核心规则：

```text
An abstract class cannot be instantiated.
```

不能写：

```java
Product p = new Product(); // compile error if Product is abstract
```

### 5.2 Abstract method

`abstract method` 只有 method header，没有 method body：

```java
public abstract int charge(int base);
```

规则：

- abstract method 必须在 abstract class 或 interface 中。
- child class 必须 override abstract methods。
- 如果 child class 没有 override 所有 abstract methods，它自己也必须是 abstract。

### 5.3 Abstract class vs Interface

| 对比 | `abstract class` | `interface` |
|---|---|---|
| 能否有 instance variables | 可以 | 通常只能 constants |
| 能否有 concrete methods | 可以 | 现代 Java 可以有 default/static，但本课程重点多是 method contract |
| 是否表示 is-a class hierarchy | 是 | 表示 capability / contract |
| 一个 class 可以继承几个 | 只能 `extends` 一个 class | 可以 `implements` 多个 interfaces |
| 适合 | shared state + shared behavior + common abstraction | unrelated classes share same behavior contract |

### 5.4 HKUPerson 例子的直觉

Lecture 里的结构：

```java
abstract class HKUPerson {
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

含义：

- `HKUPerson` 提供所有 HKU people 都有的 data：`name`, `hkid`。
- 它也提供 common `toString()`。
- 但 `charge(int base)` 对 `Student` 和 `Staff` 可能不同，所以留给 child classes 实现。

如果考试问为什么 `HKUPerson` 适合 abstract：

```text
Because it represents a general concept and gathers common elements,
but a generic HKUPerson is too general to instantiate directly.
```

---

## 6. Interface Hierarchies and Multiple Inheritance

### 6.1 Java 不支持 class multiple inheritance

Java 中：

```java
class C extends A, B { } // not allowed
```

原因之一：

```text
If A and B contain the same variable/method name, collisions must be resolved.
```

Java 为了避免复杂冲突，不支持 multiple inheritance of classes。

### 6.2 Interface 可以 multiple inheritance

Interfaces 可以继承多个 interfaces：

```java
interface AddSub {
    void add(double x, double y);
    void subtract(double x, double y);
}

interface MulDiv {
    void multiply(double x, double y);
    void divide(double x, double y);
}

interface Calculator extends AddSub, MulDiv {
    void printResult(double result);
}
```

如果 class implements `Calculator`，它必须实现：

- `add`
- `subtract`
- `multiply`
- `divide`
- `printResult`

考试重点：

```text
A class implementing a child interface must define all methods from both the child interface and its parent interfaces.
```

---

## 7. Visibility Revisited

### 7.1 Private members 继承但不能直接访问

课件中特别强调的 subtle point：

```text
All variables and methods of a parent class, even private members, are inherited by its children.
```

但是：

```text
private members cannot be referenced by name in the child class.
```

直觉：

- child object 里确实有 parent private field。
- 但 child class code 不能直接写 `fatGrams`。
- 它只能通过 parent 的 public/protected methods 间接使用。

### 7.2 FoodItem / Pizza 例子

结构：

```java
class FoodItem {
    private int fatGrams;
    protected int servings;

    public FoodItem(int numFatGrams, int numServings) {
        fatGrams = numFatGrams;
        servings = numServings;
    }

    private int calories() {
        return fatGrams * 9;
    }

    public int caloriesPerServing() {
        return calories() / servings;
    }
}

class Pizza extends FoodItem {
    public Pizza(int fatGrams) {
        super(fatGrams, 8);
    }
}
```

重点：

- `Pizza` 不能直接访问 `fatGrams`。
- `Pizza` 也不能直接调用 private `calories()`。
- 但 `Pizza` object 可以调用 inherited public method `caloriesPerServing()`。
- `caloriesPerServing()` 在 parent class 内部可以访问 parent private data。

如果：

```java
Pizza special = new Pizza(275);
System.out.println(special.caloriesPerServing());
```

计算：

```text
fat calories = 275 * 9 = 2475
servings = 8
2475 / 8 = 309 with integer division
```

输出：

```text
309
```

---

## 8. Designing for Inheritance

### 8.1 Good inheritance checklist

设计 inheritance 时可以按下面检查：

1. 是否是真正的 `is-a relationship`？
2. common data/methods 是否放在足够高的 parent class？
3. child class 是否只添加自己独有的 data/methods？
4. child class 是否用 `super(...)` 让 parent 管理 parent part 的初始化？
5. 是否避免了 shadowing inherited variables？
6. 是否适当 override 了 `toString()`、`equals()` 等 general methods？
7. 是否用 `abstract class` 表示不能直接 instantiate 的 general concept？
8. visibility 是否既支持 child class，又没有破坏 encapsulation？

### 8.2 什么时候用 `final`

`final method`：

```java
public final void methodName() { }
```

意思：

```text
Child classes cannot override it.
```

`final class`：

```java
public final class Utility { }
```

意思：

```text
No class can extend it.
```

因此：

```text
An abstract class cannot be final.
```

因为：

- `abstract class` 需要 child class 去 complete abstract methods。
- `final class` 又禁止被继承。
- 两者目标冲突。

---

## 9. 考试答题模板

### 9.1 判断 inheritance 是否合理

```text
Use inheritance only if the relationship is is-a.
If it is has-a, use composition with instance variables.
```

例子：

```text
Student is a Person       -> inheritance
Car has an Engine         -> composition
Dictionary is a Book      -> inheritance
Book has Pages            -> field, not inheritance
```

### 9.2 写 child constructor

```java
public Child(int parentData, int childData) {
    super(parentData);
    this.childData = childData;
}
```

模板解释：

1. 第一行调用 parent constructor。
2. parent data 交给 parent constructor 设置。
3. child constructor 只设置 child 自己新增的 data。

### 9.3 写 overriding method

```java
@Override
public String toString() {
    return super.toString() + "\nchild field: " + childField;
}
```

考试如果没有要求 `@Override`，也可以不写；但写上有助于 compiler 检查你是不是真的 override。

### 9.4 写 abstract class

```java
public abstract class Parent {
    protected String common;

    public Parent(String common) {
        this.common = common;
    }

    public String toString() {
        return common;
    }

    public abstract int requiredBehavior(int x);
}
```

child class：

```java
public class Child extends Parent {
    public Child(String common) {
        super(common);
    }

    public int requiredBehavior(int x) {
        return x * 2;
    }
}
```

---

## 10. 最容易错的点

### 10.1 Constructor 不是 inherited method

错误：

```text
Child class inherits parent's public constructor.
```

正确：

```text
Constructors are not inherited.
Child constructor must call parent constructor directly or indirectly.
```

### 10.2 `private` 是 inherited 但不能 direct access

错误：

```java
class Child extends Parent {
    void f() {
        System.out.println(parentPrivateField); // compile error
    }
}
```

正确：

```text
Use parent public/protected methods to access it indirectly.
```

### 10.3 `protected` 不是完全 private

`protected` 比 `private` 更开放：

- child class 可访问。
- same package 也可访问。

所以不要把所有东西都改成 `protected`。如果没有必要，保持 `private`。

### 10.4 Overloading 和 Overriding 不要混

如果 parameter list 不同：

```java
void message()
void message(String s)
```

这是 `overloading`，不是 `overriding`。

如果 child method signature 完全相同：

```java
class Parent { void message() {} }
class Child extends Parent { void message() {} }
```

这是 `overriding`。

### 10.5 Abstract class 不能 new

```java
StaffMember s = new StaffMember(...); // if StaffMember is abstract, compile error
```

但可以用 abstract class reference 指向 concrete subclass object：

```java
StaffMember s = new Employee(...);
```

这会在 Lecture 15 的 polymorphism 中继续使用。

### 10.6 Java class 只能 extends 一个 parent

错误：

```java
class C extends A, B { }
```

正确：

```java
class C extends A implements I1, I2 { }
```

---

## 11. 最后 30 秒记忆版

```text
Inheritance = derive child class from parent class.
Correct relationship: is-a.
Java syntax: class Child extends Parent.

Parent = superclass/base class.
Child = subclass/derived class.

Constructors are not inherited.
Child constructor usually starts with super(...).
super.method() calls parent version.

private members exist in child object but cannot be directly named in child class.
protected allows child access but is less encapsulated than private.

Overriding = same signature in parent and child.
Overloading = same method name, different parameter list.
final method cannot be overridden.
final class cannot be extended.

Object is root of all Java class hierarchies.
toString and equals often override Object methods.

Abstract class cannot be instantiated.
Child must implement inherited abstract methods, or it also becomes abstract.

Java does not support multiple class inheritance,
but interfaces can extend multiple interfaces.
```
