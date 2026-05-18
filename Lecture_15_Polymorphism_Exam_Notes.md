# Lecture 15: Polymorphism - Exam Notes

> 范围：`Lecture/15_Polymorphism.pdf`，主要是 `late binding`、polymorphic references、inheritance/interface polymorphism、以及用 `Comparable` 做 generic selection sort。
> 读法：先看第 0 节速查，再重点看第 2 节 reference type vs object type、第 3 节 Staff 例子、第 5 节 `Comparable` sorting。
> 目标：够详细，可以带进考场查；但不把每一页课件都原样堆上来。

---

## 0. 一页速查

### Polymorphism 必背

| 问题 | 答案 |
|---|---|
| `polymorphism` 字面意思 | many forms |
| polymorphic reference 是什么？ | 一个 reference variable 可以在不同时间指向不同 compatible object types |
| Java method call 什么时候决定调用哪个 override version？ | run time |
| 这种 run-time method binding 叫什么？ | `dynamic binding` / `late binding` |
| compatibility 可以由什么建立？ | `inheritance` 或 `interfaces` |
| parent reference 能指向 child object 吗？ | 可以，因为 child is-a parent |
| child reference 能直接指向 parent object 吗？ | 不可以，除非 cast；而且 knowingly cast parent object to child 通常不安全 |
| compiler 看什么限制 method calls？ | reference type |
| runtime 看什么决定 override method version？ | actual object type |
| interface name 可以作为 reference type 吗？ | 可以 |
| `Comparable` 的核心 method | `compareTo` |
| `compareTo` 返回 `< 0` | current object 小于 other object |
| `compareTo` 返回 `0` | 两者排序意义上相等 |
| `compareTo` 返回 `> 0` | current object 大于 other object |

### 最核心区分

```text
Reference type decides what methods are allowed by compiler.
Object type decides which overridden method body runs at runtime.
```

例子：

```java
Holiday day = new Christmas();
day.celebrate();  // allowed if Holiday has celebrate()
day.getTree();    // compile error if Holiday does not have getTree()
```

即使 object 真正是 `Christmas`，compiler 只看到 `day` 的 type 是 `Holiday`。

---

## 1. Binding and Late Binding

### 1.1 Binding 是什么？

考虑：

```java
obj.doIt();
```

`binding` 就是把这个 method invocation 绑定到某一个 method definition。

如果 binding 在 compile time 就完全决定：

```text
The same line of code calls the same method every time.
```

但 Java 对 overridden instance methods 采用：

```text
dynamic binding / late binding
```

也就是说，具体调用哪个 method version，等到 run time 看 actual object type。

### 1.2 为什么 late binding 重要？

因为同一个 reference variable 可以指向不同子类对象：

```java
StaffMember member;

member = new Employee(...);
member.pay();   // Employee pay

member = new Volunteer(...);
member.pay();   // Volunteer pay

member = new Hourly(...);
member.pay();   // Hourly pay
```

同一行：

```java
member.pay();
```

可以在不同 run-time object 上表现出不同 behavior。

这就是 polymorphism 的实际价值。

---

## 2. Polymorphic References via Inheritance

### 2.1 Parent reference 指向 child object

如果：

```text
Christmas extends Holiday
```

那么：

```java
Holiday day;
day = new Christmas();
```

合法，因为：

```text
Christmas is a Holiday.
```

更一般：

```java
Parent ref = new Child();
```

合法，前提是：

```text
Child extends Parent
```

### 2.2 Child reference 指向 parent object

下面通常不合法：

```java
CDPlayer cdplayer = new MusicPlayer();
```

原因：

```text
A MusicPlayer is not necessarily a CDPlayer.
```

可以 cast，但如果 actual object 不是 child type，run time 会出问题：

```java
MusicPlayer m = new MusicPlayer();
CDPlayer c = (CDPlayer) m; // compiles, but likely ClassCastException
```

Quick Check：

```java
MusicPlayer mplayer = new CDPlayer();     // valid
CDPlayer cdplayer = new MusicPlayer();    // invalid without cast, conceptually unsafe
```

### 2.3 Compiler restriction：reference type 决定能不能调用

假设：

```java
Holiday day = new Christmas();
```

`Christmas` 有：

```java
public void getTree() { }
```

但 `Holiday` 没有 `getTree()`。

则：

```java
day.getTree(); // compiler error
```

因为 compiler 只看：

```text
day is declared as Holiday.
```

如果确定 `day` 真的是 `Christmas`，可以 cast：

```java
((Christmas) day).getTree();
```

但考试中要说明：

```text
Cast only makes sense if the actual object is compatible with the target type.
```

### 2.4 Runtime dispatch：actual object type 决定 method body

假设 parent 和 child 都有：

```java
public void celebrate()
```

代码：

```java
Holiday day = new Christmas();
day.celebrate();
```

执行：

```text
Christmas version of celebrate()
```

因为 actual object 是 `Christmas`。

这就是：

```text
The type of the object being referenced, not the reference type, determines which overridden method is invoked.
```

---

## 3. Staff Example: Polymorphism via Inheritance

### 3.1 Class hierarchy

Lecture 里的人员工资例子结构：

```text
StaffMember
  -> Volunteer
  -> Employee
       -> Executive
       -> Hourly
```

含义：

- `StaffMember` 是 abstract parent，代表所有 staff 的 common part。
- `Volunteer` 不拿工资，`pay()` 返回 `0.0`。
- `Employee` 有 `socialSecurityNumber` 和 `payRate`。
- `Executive` 是一种 `Employee`，可以有 `bonus`。
- `Hourly` 是一种 `Employee`，用 `payRate * hoursWorked` 算工资。

### 3.2 `StaffMember` 为什么是 abstract？

`StaffMember` 代表 generic staff member。

它有 common data：

```java
protected String name;
protected String address;
protected String phone;
```

它可以提供 common `toString()`。

但 `pay()` 对不同 staff type 不同：

```java
public abstract double pay();
```

所以：

```text
StaffMember should not be instantiated directly.
Derived classes must define pay().
```

### 3.3 `StaffMember[]` 可以装不同 subclass objects

关键代码结构：

```java
private StaffMember[] staffList;

staffList[0] = new Executive(...);
staffList[1] = new Employee(...);
staffList[2] = new Hourly(...);
staffList[3] = new Volunteer(...);
```

为什么合法？

```text
Executive is a StaffMember.
Employee is a StaffMember.
Hourly is a StaffMember.
Volunteer is a StaffMember.
```

这个 array 的 declared element type 是 `StaffMember`，但每个 slot 的 actual object type 可以不同。

### 3.4 `payday()` 的 polymorphic call

核心 loop：

```java
for (int i = 0; i < staffList.length; i++) {
    System.out.println(staffList[i]);
    double amount = staffList[i].pay(); // polymorphic
}
```

`staffList[i].pay()` 是 polymorphic 的原因：

- compiler 知道 `StaffMember` 有 `pay()`，所以 method call 合法。
- run time 根据 actual object type 调用具体 version。

| actual object type | `pay()` behavior |
|---|---|
| `Volunteer` | returns `0.0` |
| `Employee` | returns `payRate` |
| `Executive` | returns `super.pay() + bonus`，然后 reset bonus |
| `Hourly` | returns `payRate * hoursWorked`，然后 reset hours |

### 3.5 为什么需要 cast？

在 Staff constructor 中有类似逻辑：

```java
((Executive) staffList[0]).awardBonus(500.00);
((Hourly) staffList[3]).addHours(40);
```

为什么要 cast？

因为 `staffList[0]` 的 declared type 是：

```java
StaffMember
```

compiler 只允许调用 `StaffMember` 中 declared 的 methods，例如 `pay()`、`toString()`。

`awardBonus` 只在 `Executive` 中定义。

所以必须：

```java
((Executive) staffList[0]).awardBonus(500.00);
```

考试注意：

```text
This cast is safe only because the programmer knows staffList[0] actually refers to an Executive object.
```

### 3.6 `toString()` 也可以 polymorphic

在 loop 里：

```java
System.out.println(staffList[i]);
```

实际会调用 object 的 `toString()`。

如果 actual object 是 `Hourly`：

- 先调用 `Employee.toString()`。
- `Employee.toString()` 可能调用 `super.toString()`。
- `Hourly.toString()` 再加上 hours 信息。

这说明：

```text
Polymorphism is not only for pay(); it applies to overridden instance methods in general.
```

---

## 4. Polymorphism via Interfaces

### 4.1 Interface reference

Interface name 可以作为 reference type：

```java
Speaker current;
```

如果 class implements `Speaker`：

```java
class Dog implements Speaker { }
class Philosopher implements Speaker { }
```

那么：

```java
Speaker guest = new Philosopher();
guest.speak();

guest = new Dog();
guest.speak();
```

同一个 `Speaker` reference 可以指向不同 implementing class 的 objects。

### 4.2 Compiler 仍然看 reference type

假设：

```java
public interface Speaker {
    void speak();
    void announce(String str);
}
```

`Philosopher` 额外有：

```java
void pontificate()
```

如果：

```java
Speaker special = new Philosopher();
special.pontificate(); // compiler error
```

原因：

```text
The compiler bases method calls on the reference type Speaker.
Speaker does not declare pontificate().
```

可以：

```java
((Philosopher) special).pontificate();
```

前提是 actual object 确实是 `Philosopher`。

### 4.3 Quick Check

题目：

```java
Speaker first = new Dog();
Philosopher second = new Philosopher();
second.pontificate();
first = second;
```

判断：

- `Speaker first = new Dog();` valid if `Dog implements Speaker`。
- `Philosopher second = new Philosopher();` valid。
- `second.pontificate();` valid because reference type is `Philosopher`。
- `first = second;` valid if `Philosopher implements Speaker`。

答案：

```text
All assignments and method calls are valid as written.
```

---

## 5. Polymorphism in Sorting

### 5.1 Sorting 的目标

`Sorting` 是把 list items 按某种 order 排列。

例子：

- test scores：ascending numeric order。
- people：alphabetical order by last name。

问题是：

```text
Different classes define "less than" differently.
```

所以 Java 用 `Comparable` 把 comparison 规则交给 class 自己定义。

### 5.2 Selection Sort 策略

`Selection Sort`：

1. 找到 list 中最小的 value。
2. 和第一个位置交换。
3. 从第二个位置 onward 找剩余最小值。
4. 和第二个位置交换。
5. 重复直到所有 value 在正确位置。

模板：

```java
for (int index = 0; index < list.length - 1; index++) {
    int min = index;

    for (int scan = index + 1; scan < list.length; scan++) {
        if (list[scan].compareTo(list[min]) < 0) {
            min = scan;
        }
    }

    Comparable temp = list[min];
    list[min] = list[index];
    list[index] = temp;
}
```

### 5.3 为什么这个 sort 是 polymorphic？

method signature：

```java
public static void selectionSort(Comparable[] list)
```

含义：

- 只要 object 的 class implements `Comparable`，就能放进来 sort。
- `selectionSort` 不需要知道 objects 是 `Contact`、`Book`、还是其他 class。
- 它只调用统一接口：`compareTo`。

真正的 comparison logic 在各个 class 里：

```java
list[scan].compareTo(list[min])
```

run time 根据 actual object type 执行对应的 `compareTo`。

### 5.4 `compareTo` 返回值

`a.compareTo(b)`：

| 返回值 | 含义 | sort 中解释 |
|---:|---|---|
| `< 0` | `a` comes before `b` | `a` 更小 |
| `0` | same order | 排序意义上相等 |
| `> 0` | `a` comes after `b` | `a` 更大 |

所以 selection sort 里：

```java
if (list[scan].compareTo(list[min]) < 0)
    min = scan;
```

含义：

```text
If current scanned object should come before current minimum,
update minimum position.
```

### 5.5 Contact sorting example

`Contact` 实现 `Comparable`。

排序规则：

1. 先按 `lastName`。
2. 如果 `lastName` 相同，再按 `firstName`。

简化版：

```java
public int compareTo(Object other) {
    Contact otherContact = (Contact) other;

    if (lastName.equals(otherContact.getLastName())) {
        return firstName.compareTo(otherContact.getFirstName());
    } else {
        return lastName.compareTo(otherContact.getLastName());
    }
}
```

例子：

```text
Smith, John
Smith, Larry
```

last name 都是 `Smith`，所以比较 first name：

```text
John comes before Larry
```

### 5.6 `equals` 和 `compareTo` 的关系

Lecture 里的 `Contact` 还 override 了 `equals`：

```java
public boolean equals(Object other) {
    return lastName.equals(((Contact) other).getLastName())
        && firstName.equals(((Contact) other).getFirstName());
}
```

考试理解：

- `equals` 判断两个 contacts 是否同名。
- `compareTo` 判断排序先后。
- 对很多 class 来说，如果 `compareTo` 返回 `0`，最好也和 `equals` 的 equality idea 一致。

---

## 6. Cast and Safety

### 6.1 Cast 改变 compiler view，不改变 object 本身

```java
((Executive) staffList[0]).awardBonus(500.00);
```

这个 cast 的作用：

```text
Tell compiler to treat this reference as Executive for this expression.
```

它不会把 object 变成 `Executive`。

如果 object 本来不是 `Executive`：

```java
StaffMember s = new Volunteer(...);
((Executive) s).awardBonus(500.00); // ClassCastException at runtime
```

### 6.2 安全 cast 的判断

考试中可以这样想：

```text
Can the actual object pass the is-a test for the target type?
```

| actual object | target cast | 安全？ |
|---|---|---|
| `Executive` | `Employee` | yes |
| `Executive` | `StaffMember` | yes |
| `Employee` | `Executive` | only if actual object is really Executive; plain Employee no |
| `Volunteer` | `Executive` | no |
| `Dog implements Speaker` | `Speaker` | yes |
| `Speaker ref to Dog` | `Dog` | yes if actual object is Dog |

---

## 7. 考试答题模板

### 7.1 判断 assignment 是否 valid

模板：

```text
Left-hand reference type must be same as, or an ancestor/interface of, the actual object type.
```

例子：

```java
Parent p = new Child(); // valid
Child c = new Parent(); // invalid
Interface i = new ImplementingClass(); // valid
```

如果有 cast：

```text
It may compile, but it is safe only if the actual object is compatible.
```

### 7.2 判断 method call 是否 compile

模板：

```text
Compiler checks the declared reference type.
The method must be declared in that type or inherited by that type.
```

例子：

```java
Holiday day = new Christmas();
day.celebrate(); // compile if Holiday declares celebrate
day.getTree();   // compile error if Holiday does not declare getTree
```

### 7.3 判断 method call 执行哪个 version

模板：

```text
If the method is overridden, Java uses dynamic binding.
The actual object's class determines which method body runs.
```

例子：

```java
StaffMember s = new Hourly(...);
s.pay(); // Hourly.pay()
```

### 7.4 写 generic `Comparable` sorting 的说明

```text
The sorting method takes Comparable[].
It calls compareTo polymorphically.
Each class decides its own ordering by implementing compareTo.
Therefore the same selectionSort method can sort different object types.
```

---

## 8. 最容易错的点

### 8.1 Reference type 和 object type 混淆

```java
Parent p = new Child();
```

这里：

- reference type = `Parent`
- actual object type = `Child`

Compiler 看 `Parent`。

Runtime overridden method dispatch 看 `Child`。

### 8.2 Parent reference 不能调用 child-only method

错误：

```java
StaffMember s = new Executive(...);
s.awardBonus(500); // compile error
```

正确：

```java
((Executive) s).awardBonus(500);
```

前提：

```text
s actually refers to an Executive object.
```

### 8.3 Cast 不保证 runtime 安全

```java
StaffMember s = new Volunteer(...);
Executive e = (Executive) s; // runtime error
```

原因：

```text
A Volunteer is not an Executive.
```

### 8.4 Interface reference 也有同样限制

```java
Speaker sp = new Philosopher();
sp.pontificate(); // compile error if Speaker lacks pontificate
```

即使 actual object 是 `Philosopher`，compiler 只看 `Speaker`。

### 8.5 `Comparable[]` 不是只能装一种 class 的原理

`Comparable[]` 的重点不是 array magically understands all objects。

重点是：

```text
Every object in the array must provide compareTo.
```

sorting algorithm 只依赖这个 common contract。

---

## 9. 最后 30 秒记忆版

```text
Polymorphism = many forms.
A polymorphic reference can refer to different compatible object types.

Compatibility comes from inheritance or interfaces.
Parent ref = new Child(); is valid.
Child ref = new Parent(); is not valid without cast and is usually unsafe.

Compiler checks reference type:
Can I call this method?

Runtime checks actual object type for overridden methods:
Which version should run?

Dynamic binding / late binding = method binding deferred until runtime.

StaffMember[] can store Employee, Executive, Hourly, Volunteer
because all are StaffMember.
staffList[i].pay() is polymorphic.

Interface reference works the same way:
Speaker s = new Dog();
s can call only methods declared in Speaker.

Comparable enables generic sorting.
compareTo < 0 means current object comes before other object.
selectionSort(Comparable[] list) can sort any objects that implement Comparable.
```
