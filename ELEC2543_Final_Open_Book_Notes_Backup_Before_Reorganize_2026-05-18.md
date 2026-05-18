# ELEC2543 Final 开卷考试复习资料草稿

> 状态：可继续修改的打印草稿。
> 范围：Past Paper 2025 + Chapters 13-21 的复习整理。
> 风格：解释性文字用简体中文；Java 关键字、class、object、sorting、Big-O 等考试术语保留英文。
> Merge working copy：这是从主笔记复制出来的融合草稿；原始 `ELEC2543_Final_Open_Book_Notes_Draft.md` 没有直接修改。
> 本轮 merge 来源：`Libera-sudo/ELEC2543-Course-Notes`。目前先合入高价值缺口，不覆盖原有结构。

---

## 0. 使用方式

- 先看 **Past Paper 速查表**，快速定位题目对应的知识点。
- 如果不知道某道题属于哪一类，先查 **0.2 问题快查表**。
- 再到对应的 **Part** 查看完整解释、代码和手算步骤。
- 代码块、输出结果、Java 关键字和算法名称保持英文，方便考试时直接对照题目。
- 我已经把目前发现的前后不一致处改成了明确说明；后续如果新增资料，再按同样方式归并和复查。

---

### 0.0 Merge 覆盖记录（本复制稿专用）

本文件是 merge working copy，不是主笔记终稿。本轮从朋友仓库先吸收“主笔记缺口明显、考试可能直接查”的内容；已经与原笔记重复的章节暂时不整段搬运，避免把打印稿变得太长。

| 朋友仓库章节 | 本复制稿处理方式 | 放到哪里 |
|---|---|---|
| Ch.1-Ch.9 Java basics / class / array / method | 原笔记 Part 0 已覆盖，暂不整段搬运 | Part 0 |
| Ch.10 Static Modifier | 补充 `static variable` 共享状态和 `static method` 访问规则 | Part 0 -> 0.6 |
| Ch.11 Variables and Object Comparison | 原笔记已有 comparison / reference / `equals`，暂不整段搬运 | Part 0 -> 0.3 / 0.7 |
| Ch.12 Wrapper Class | 补充 `valueOf`、`new Integer`、parse methods、constants、`null` unboxing | Part 0 -> 0.7 |
| Ch.13-Ch.15 Interface / Inheritance / Polymorphism | 原笔记 Part 1-3 已覆盖，保留原结构 | Part 1-3 |
| Ch.16 Exceptions | 原笔记 Part 4 已覆盖，朋友笔记主要作为复查参考 | Part 4 |
| Ch.17-Ch.18 Recursion / Sorting | 原笔记 Part 5-7 已覆盖，保留 Past Paper 手算结构 | Part 5-7 |
| Ch.19-Ch.20 Data Structures / Trees | 原笔记 Part 8-9 已覆盖，保留现有目录 | Part 8-9 |
| Ch.21 Advanced Tree Structures | 补充 Huffman Coding；AVL/Heap 用原 Past Paper 结构 | Part 10 -> 10.11 |

---

### 0.1 多层级目录

这份笔记按“先定位题型，再进入对应知识点”的方式组织。一级目录告诉你大方向；二级目录告诉你具体章节；三级目录告诉你最常查的小点。

| 一级目录 | 二级目录 | 三级目录 / 重点小节 | 适合什么时候查 |
|---|---|---|---|
| 0. 使用方式与索引 | 0.1 多层级目录 | 本表 | 不知道整份资料怎么用时 |
| 0. 使用方式与索引 | 0.2 问题快查表 | 按问题定位章节 | 看到题目但不知道查哪里时 |
| 1. 整体考试地图 | 1.1 Past Paper 2025 考点分布 | Q1-Q6 对应知识点 | 想知道每道大题考什么时 |
| 1. 整体考试地图 | 1.2 Part 0 + 10 个 Part 总览 | 基础区 + 10 个复习模块 | 想按章节系统复习时 |
| 2. Past Paper 速查表 | Q1-Q5 关键答案 | MCQ/output/BST/AVL/sorting 答案 | 考前快速背答案时 |
| Part 0. 第 13 讲以前 Java 基础区 | 0.1-0.11 | Java basics、class/object、reference、library、array、method、static variable、wrapper parse/valueOf、syntax、midterm templates | Q1 MCQ、Q2(a)、基础代码题、期中旧题 |
| Part 1. Interface | 1.1-1.8 | interface 默认规则、`implements`、多 interfaces、`Comparable` | Q6 interface 或 `Comparable` 题 |
| Part 2. Inheritance | 2.1-2.11 | `extends`、`protected`、`this/super`、overriding、abstract class | Q6 inheritance / abstract class 题 |
| Part 3. Polymorphism | 3.1-3.7 | late binding、declared type、casting、interface polymorphism | 看到 parent reference 指向 child object 时 |
| Part 4. Exceptions | 4.1-4.11 | `try-catch-finally`、propagation、checked/unchecked、`throw` | Q1 exception MCQ 或 Q2(b)(c) output |
| Part 5. Recursion | 5.1-5.8 | base case、factorial、sum、maze、Towers of Hanoi | 递归概念题或理解 merge/quick sort 前置知识 |
| Part 6. Sorting I | 6.1-6.9 | Bubble、Insertion、Selection、Big-O | 简单 sorting 和 complexity 基础 |
| Part 7. Sorting II | 7.1-7.11 | Merge Sort、Quick Sort、Past Paper array、complexity | Q5 手算 sorting 或复杂度 |
| Part 8. Data Structures | 8.1-8.12 | Linked List、Queue、Stack、Graph、Generics | stack/queue/linked list 概念题 |
| Part 9. Trees & BST | 9.1-9.10 | traversal、BST insertion、Past Paper Q3、deletion | Q3 BST 画图和 traversal |
| Part 10. Advanced Trees | 10.1-10.12 | AVL height、AVL rotation、insert 70、Heap、Heap Sort、Huffman Coding | Q4 AVL、heap 或 coding tree 题 |
| 最后一页速查 | Java Basics / OOP / Sorting / Trees | 最短公式表 | 打印后最后几分钟快速扫 |

### 0.2 问题快查表：遇到什么问题查哪里？

| 你遇到的问题 / 题目关键词 | 先查哪里 | 具体位置 |
|---|---|---|
| JVM 是什么？Java 怎么执行？ | Part 0 基础区 | Part 0 -> 0.1 Java Basics |
| `boolean` 默认值是什么？`Boolean` 呢？ | Part 0 基础区 | Part 0 -> 0.1 Java Basics |
| `System`、`String`、`Scanner`、`IOException` 在哪个 package？ | Part 0 基础区 | Part 0 -> 0.4 Common Libraries 与 Enum |
| `final` 能做什么？ | Part 0 基础区 | Part 0 -> 0.1 Java Basics |
| `String.concat()` 为什么没有改变原来的 string？ | Part 0 基础区 | Part 0 -> 0.4 Common Libraries 与 Enum |
| `"Result: " + 10 + 20` 输出什么？ | Part 0 基础区 | Part 0 -> 0.4 Common Libraries 与 Enum |
| method 传 `int` 和传 `int[]` 有什么区别？ | Part 0 基础区 | Part 0 -> 0.3 / 0.6 |
| Q2(a) array output tracing 怎么做？ | Part 0 基础区 | Part 0 -> 0.3 / 0.6 |
| `main` method 每个词是什么意思？ | Part 0 基础区 | Part 0 -> 0.1 Java Basics |
| `char` 和 `String` 怎么区分？ | Part 0 基础区 | Part 0 -> 0.1 Java Basics |
| casting 是四舍五入吗？ | Part 0 基础区 | Part 0 -> 0.1 Java Basics |
| `switch` 忘写 `break` 会怎样？ | Part 0 基础区 | Part 0 -> 0.1 Java Basics |
| `nextInt()` 后面接 `nextLine()` 为什么会被跳过？ | Part 0 基础区 | Part 0 -> 0.1 Java Basics |
| 怎样按 UML 写 class？ | Part 0 基础区 | Part 0 -> 0.2 Class 与 Object |
| constructor 为什么不能写 `void`？ | Part 0 基础区 | Part 0 -> 0.2 Class 与 Object |
| `toString()` 什么时候自动调用？ | Part 0 基础区 | Part 0 -> 0.2 Class 与 Object |
| 有 `new` 和没有 `new` 的 reference 有什么区别？ | Part 0 基础区 | Part 0 -> 0.3 Object Reference 与 Memory |
| `NullPointerException` 常见原因是什么？ | Part 0 基础区 | Part 0 -> 0.3 / 0.9 |
| `String` 常用 methods 怎么用？ | Part 0 基础区 | Part 0 -> 0.4 Common Libraries 与 Enum |
| `Math.random()` 范围是什么？`Random.nextInt(10)` 范围是什么？ | Part 0 基础区 | Part 0 -> 0.4 Common Libraries 与 Enum |
| enum 怎么写？constructor 为什么 private？ | Part 0 基础区 | Part 0 -> 0.4 Common Libraries 与 Enum |
| array、String、ArrayList 的长度分别怎么写？ | Part 0 基础区 | Part 0 -> 0.5 Array 与 ArrayList |
| Object array 为什么初始全是 `null`？ | Part 0 基础区 | Part 0 -> 0.5 Array 与 ArrayList |
| ArrayList 能不能存 `int`？ | Part 0 基础区 | Part 0 -> 0.5 Array 与 ArrayList |
| method overloading 的 signature 是什么？ | Part 0 基础区 | Part 0 -> 0.6 Method、Parameter Passing 与 Static |
| `varargs` 怎么写？有什么限制？ | Part 0 基础区 | Part 0 -> 0.6 Method、Parameter Passing 与 Static |
| `static` method 为什么不能访问 instance variable？ | Part 0 基础区 | Part 0 -> 0.6 Method、Parameter Passing 与 Static |
| `static variable` 是不是所有 object 共享？ | Part 0 基础区 | Part 0 -> 0.6 Method、Parameter Passing 与 Static |
| `String`、object、double、wrapper class 应该怎么比较？ | Part 0 基础区 | Part 0 -> 0.7 Comparison、Wrapper Class、`equals` |
| `equals` 怎么重写？ | Part 0 基础区 | Part 0 -> 0.7 Comparison、Wrapper Class、`equals` |
| `Integer` 的 `==` 为什么有时候 true 有时候 false？ | Part 0 基础区 | Part 0 -> 0.7 Comparison、Wrapper Class、`equals` |
| `Integer.valueOf()`、`new Integer()`、autoboxing 有什么区别？ | Part 0 基础区 | Part 0 -> 0.7 Comparison、Wrapper Class、`equals` |
| `parseInt()`、`MAX_VALUE`、`MIN_VALUE` 怎么用？ | Part 0 基础区 | Part 0 -> 0.7 Comparison、Wrapper Class、`equals` |
| 分号、括号、命名规则、operator precedence 怎么查？ | Part 0 基础区 | Part 0 -> 0.8 语法细节速查 |
| 常见 compile error 怎么修？ | Part 0 基础区 | Part 0 -> 0.9 常见错误速查 |
| midterm 题型：while 条件怎么补？ | Part 0 基础区 | Part 0 -> 0.10 Midterm 题型模板 |
| midterm 题型：Array 改 ArrayList 哪些会变？ | Part 0 基础区 | Part 0 -> 0.10 Midterm 题型模板 |
| midterm 题型：rotateLeft 怎么写？ | Part 0 基础区 | Part 0 -> 0.10 Midterm 题型模板 |
| 写代码题最后怎么检查？ | Part 0 基础区 | Part 0 -> 0.11 考前代码检查清单 |
| interface 是什么？method 为什么没有 body？ | Interface | Part 1 -> 1.1 什么是 Interface？ |
| interface method 默认是什么 modifier？ | Interface | Part 1 -> 1.1 什么是 Interface？ |
| interface variable 默认是什么？ | Interface | Part 1 -> 1.1 什么是 Interface？ |
| class 怎样实现 interface？ | Interface | Part 1 -> 1.2 怎样 implement 一个 Interface？ |
| interface 能不能当作 variable type 或 parameter type？ | Interface | Part 1 -> 1.3 / 1.4 |
| Java 能不能 class 多继承？能不能 implements 多个 interfaces？ | Interface / Inheritance | Part 1 -> 1.5 一个 Class 可以 implement 多个 Interfaces |
| `Comparable` 是什么？`compareTo` 返回值怎么看？ | Interface | Part 1 -> 1.6 `Comparable` |
| `extends` 是什么？inheritance 的 is-a 关系是什么？ | Inheritance | Part 2 -> 2.1 / 2.2 |
| `private` 和 `protected` 在 subclass 中有什么区别？ | Inheritance | Part 2 -> 2.3 `protected` |
| `this` 和 `super` 怎么区分？ | Inheritance | Part 2 -> 2.4 `super` |
| `super()` 必须写在哪里？ | Inheritance | Part 2 -> 2.4 `super` |
| Overloading 和 Overriding 怎么区分？ | Inheritance | Part 2 -> 2.6 Overloading vs Overriding |
| abstract class 和 interface 有什么区别？ | Inheritance | Part 2 -> 2.9 Abstract Class |
| parent class reference 指向 child object 时，调用哪个 method？ | Polymorphism | Part 3 -> 3.2 Late Binding |
| 为什么 reference type 是 parent/interface 时，不能调用 child 独有 method？ | Polymorphism | Part 3 -> 3.2 Late Binding |
| upcasting / downcasting 是什么？ | Polymorphism | Part 3 -> 3.4 References 和 Inheritance |
| `try-catch-finally` 有 exception 时怎么输出？ | Exceptions | Part 4 -> 4.4 `finally` / 4.5 Past Paper Q2b/c |
| `try-catch-finally` 没有 exception 时怎么输出？ | Exceptions | Part 4 -> 4.4 `finally` / 4.5 Past Paper Q2b/c |
| checked exception 和 unchecked exception 怎么区分？ | Exceptions | Part 4 -> 4.8 Checked vs Unchecked Exceptions |
| `Compile-time exception` 是不是 Java exception 类型？ | Exceptions | Part 4 -> 4.8 Checked vs Unchecked Exceptions |
| exception propagation 是什么？ | Exceptions | Part 4 -> 4.6 Exception Propagation |
| recursion 必须有什么？ | Recursion | Part 5 -> 5.2 两个必要部分 |
| factorial / sum recursion 怎么 trace？ | Recursion | Part 5 -> 5.3 / 5.4 |
| Towers of Hanoi 需要几步？ | Recursion | Part 5 -> 5.7 Towers Of Hanoi |
| Bubble Sort、Insertion Sort、Selection Sort 怎么区分？ | Sorting I | Part 6 -> 6.2 / 6.3 / 6.4 / 6.5 |
| Big-O 怎么化简？ | Sorting I | Part 6 -> 6.7 Big-O |
| 为什么简单 sorting 是 `O(n^2)`？ | Sorting I | Part 6 -> 6.8 为什么简单 sorting 是 `O(n^2)`？ |
| Merge Sort 怎么手算 Past Paper 数组？ | Sorting II | Part 7 -> 7.3 Past Paper 数组 |
| Merge Sort complexity 是什么？ | Sorting II | Part 7 -> 7.5 Merge Sort Complexity |
| Quick Sort 用 first element as pivot 怎么手算？ | Sorting II | Part 7 -> 7.7 Past Paper 数组，选择第一个元素作为 pivot |
| Quick Sort average / worst complexity 是什么？ | Sorting II | Part 7 -> 7.9 Quick Sort Complexity |
| Linked List 插入时为什么顺序不能反？ | Data Structures | Part 8 -> 8.5 Linked List 基本操作 |
| Queue 是 FIFO 还是 LIFO？ | Data Structures | Part 8 -> 8.8 Queue |
| Stack 是 FIFO 还是 LIFO？ | Data Structures | Part 8 -> 8.9 Stack |
| Preorder / Inorder / Postorder 顺序是什么？ | Trees & BST | Part 9 -> 9.3 Tree Traversal |
| BST 为什么 inorder 得到 sorted order？ | Trees & BST | Part 9 -> 9.4 Binary Search Tree |
| Past Paper Q3 BST 怎么画？ | Trees & BST | Part 9 -> 9.6 Past Paper Q3 BST 构造 |
| Past Paper Q3 traversal 答案是什么？ | Trees & BST | Part 9 -> 9.7 Past Paper Q3 Traversal Answers |
| BST 删除三种情况是什么？ | Trees & BST | Part 9 -> 9.8 BST Deletion |
| AVL Tree 的定义是什么？ | Advanced Trees | Part 10 -> 10.2 AVL Definition |
| AVL Tree 有 `n` 个 nodes 的 minimum height 是什么？ | Advanced Trees | Part 10 -> 10.3 Past Paper Q4a |
| AVL search/insert/delete complexity 是什么？ | Advanced Trees | Part 10 -> 10.4 Past Paper Q4b |
| AVL rotation 怎么判断 single / double？ | Advanced Trees | Part 10 -> 10.5 AVL Rotations |
| Past Paper Q4 插入 `70` 后怎么 rotate？ | Advanced Trees | Part 10 -> 10.6 Past Paper Q4c |
| Heap 是什么？min-heap root 是什么？ | Advanced Trees | Part 10 -> 10.7 什么是 Heap？ |
| Heap delete minimum / insert 怎么做？ | Advanced Trees | Part 10 -> 10.8 Heap Operations |
| Heap 用 array 怎么存？child/parent index 是什么？ | Advanced Trees | Part 10 -> 10.10 Heap 的 Array Implementation |
| Huffman Coding 怎么构造？为什么高频字符短？ | Advanced Trees | Part 10 -> 10.11 Huffman Coding |

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

### 1.2 Part 0 + 10 个 Part 总览

| Part | 内容 | 考试重要性 |
|---|---|---|
| Part 0 | 第 13 讲以前 Java 基础区：Java basics、class/object、reference、array、method、static、wrapper、syntax | 很高 |
| Part 1 | Interface：定义、`implements`、`Comparable` | 高 |
| Part 2 | Inheritance：`extends`、`protected`、`super`、abstract class | 很高 |
| Part 3 | Polymorphism：late binding、inheritance/interface polymorphism | 高 |
| Part 4 | Exceptions：`try-catch-finally`、propagation、checked vs unchecked | 很高 |
| Part 5 | Recursion：base case、recursive case、Towers of Hanoi | 中 |
| Part 6 | Sorting I：Bubble、Insertion、Selection Sort + Big-O | 很高 |
| Part 7 | Sorting II：Merge Sort + Quick Sort | 很高 |
| Part 8 | Data Structures：Linked List、Stack、Queue | 高 |
| Part 9 | Trees & BST | 很高 |
| Part 10 | Advanced Trees：AVL Tree + Heap + Huffman Coding | 很高 |

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

# Part 0：第 13 讲以前 Java 基础区

这一块来自期中开卷笔记，覆盖第 13 讲以前的基础内容。它放在 Part 1 前面，因为这些知识是后面 interface、inheritance、exception、sorting、tree 的基础。考试时如果你看到的是基础语法、代码输出、写 class、array、ArrayList、method、static、wrapper、`equals` 这类问题，先查 Part 0。

零基础使用方式：

```text
如果题目问定义：先写一句定义，再写 2-3 条规则，再给一个小例子。
如果题目让看 output：逐行 trace，遇到 new 画 object，遇到 method call 对参数。
如果题目让写 code：先套模板，再改 class name / variable name / method body。
如果题目问“会不会变”：判断 primitive value、reference、object 内部是否被改。
```

## 0.1 Java Basics

### Java 执行流程和 JVM

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

Java 跨平台的原因：`.java` 会先被 compiler 编译成 bytecode，bytecode 可以在不同平台的 JVM 上执行。

### `main` method：程序入口

```java
public static void main(String[] args) {
}
```

逐词解释：

| 词 | 含义 |
|---|---|
| `public` | 谁都能访问 |
| `static` | 不需要先创建 object，就可以运行 |
| `void` | 不返回值 |
| `main` | Java 程序入口 method name |
| `String[] args` | 命令行参数，类型是 String array |

如果一个 class 没有 `main` method，通常可以 compile，但直接运行时 launcher 会报 runtime error：找不到 main method。

### 8 种 Primitive Types

| Type | 大小 | 说明 |
|---|---:|---|
| `int` | 32-bit | 最常用整数 |
| `double` | 64-bit | 最常用小数 |
| `char` | 16-bit | 单个字符，用单引号，例如 `'A'` |
| `boolean` | - | 只有 `true` / `false` |
| `byte` | 8-bit | 很少用 |
| `short` | 16-bit | 很少用 |
| `long` | 64-bit | 大整数 |
| `float` | 32-bit | 小数，不常用 |

重点坑：

```text
char 用单引号: 'A'
String 用双引号: "hello"
String 不是 primitive，是 class。
```

记忆：

```text
大多数小写开头的基本类型是 primitive，复制 value。
大多数大写开头的是 class/object reference，复制 reference。
例外：array 例如 int[] 虽然写着 int，但 array 本身是 object。
```

### 变量与常量

```java
int age = 20;           // variable，可以改
final double PI = 3.14; // constant，不能改
```

`final` 修饰 variable 后，这个 variable 不能重新 assign。

`final` 常见用途：

```text
final variable  -> constant，不能重新 assign
final method    -> 不能被 override
final class     -> 不能被 extended
```

所以 Past Paper MCQ 里如果问 `final` 的用途，通常是 `All of the above`。

### Type Conversion

Widening：自动转换，小类型到大类型，不丢数据。

```java
int x = 5;
double y = x;  // y = 5.0
```

Narrowing：手动 casting，大类型到小类型，可能丢数据。

```java
double a = 3.99;
int b = (int) a;  // b = 3
```

重点坑：

```text
casting 是截断，不是四舍五入。
(int)3.99 = 3，不是 4。
```

### Control Flow

`if-else`：

```java
if (condition) {
    ...
} else if (condition2) {
    ...
} else {
    ...
}
```

`switch`：

```java
switch (value) {
    case 1:
        ...
        break;
    default:
        ...
}
```

重点坑：

```text
switch 里忘写 break 会 fall through。
也就是从匹配到的 case 开始，后面 case 会继续执行，直到遇到 break 或 switch 结束。
```

`for loop`：

```java
for (int i = 0; i < 5; i++) {
    ...
}
```

执行次数：

```text
i = 0, 1, 2, 3, 4
一共 5 次
```

重点坑：

```text
i < 5  执行 5 次：0 到 4
i <= 5 执行 6 次：0 到 5
```

Enhanced for loop：

```java
for (int n : nums) {
    System.out.println(n);
}
```

### Scanner 输入

```java
import java.util.Scanner;

Scanner scan = new Scanner(System.in);
int num = scan.nextInt();       // 读整数
String word = scan.next();      // 读一个单词
String line = scan.nextLine();  // 读一整行
```

重点坑：

```text
nextInt() 后面直接接 nextLine()，nextLine() 可能会读到上一行剩下的回车，所以看起来像被跳过。
解决：中间加一个空的 scan.nextLine() 吃掉回车。
```

```java
int num = scan.nextInt();
scan.nextLine();          // eat leftover newline
String line = scan.nextLine();
```

### `print` vs `println`

```java
System.out.print("A");    // 打印后不换行
System.out.println("A");  // 打印后换行
```

### 逻辑运算符

| Operator | 含义 |
|---|---|
| `&&` | AND，两边都 true 才 true |
| `||` | OR，一个 true 就 true |
| `!` | NOT，取反 |
| `==` / `!=` | 等于 / 不等于 |
| `>` `<` `>=` `<=` | 大小比较 |

### Default Values

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

## 0.2 Class 与 Object

Class = 蓝图 / 模板。Object = 按照 class 这个蓝图创建出来的实例。

每个 object 通常有：

```text
state    = 属性 / instance variables
behavior = methods
```

### 完整 Class 结构模板

```java
public class Student {
    private String name;  // instance variable
    private int age;

    public Student(String name, int age) {  // constructor
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return name + " (age: " + age + ")";
    }
}
```

各部分说明：

| 部分 | 作用 | 注意 |
|---|---|---|
| instance variable | object 的数据，每个 object 各一份 | 通常用 `private` |
| constructor | 创建 object 时自动调用 | 名字必须等于 class 名；没有返回类型 |
| getter / setter | 读写 private variables | 通常用 `public` |
| `toString()` | `println(object)` 时自动调用 | 不写可能输出类似 `Student@1fee6fc` |
| `this` | 当前 object | 用来区分 instance variable 和 parameter |

### Constructor 规则

```text
如果没有写任何 constructor，Java 会送一个空的 default constructor。
一旦自己写了任何 constructor，Java 不再自动送 default constructor。
constructor 没有返回类型，连 void 也不能写。
```

错误例子：

```java
public void Student(String name, int age) {
    ...
}
```

这不是 constructor，而是普通 method，因为它写了 `void`。

### 创建和使用 Object

```java
Student s = new Student("Tom", 20);
s.getName();
System.out.println(s);  // 自动调用 s.toString()
```

### `this` 的用途

```java
public Student(String name, int age) {
    this.name = name;
    this.age = age;
}
```

解释：

```text
this.name = instance variable
name      = parameter
```

重点坑：

```java
name = name;
```

如果 parameter 也叫 `name`，这行会把 parameter 赋给 parameter 自己，instance variable 没变。为了安全，constructor 和 setter 里建议用 `this.`。

### Instance Variable vs Local Variable

| | Instance Variable | Local Variable |
|---|---|---|
| 定义位置 | class 里，method 外 | method 里面 |
| 作用范围 | 整个 object / class 内可用 | 只在那个 method 或 block 内 |
| default value | 有，例如 `int=0`, `boolean=false`, reference=`null` | 没有，必须手动赋值 |

重点坑：

```text
local variable 不赋值就使用 -> compile error
常见信息：variable might not have been initialized
```

### UML 类图

UML class diagram 通常分三格：

```text
ClassName
attributes
methods
```

例子：

```text
Student
- name: String
- age: int
+ Student(name: String, age: int)
+ getName(): String
+ setAge(age: int): void
+ toString(): String
```

UML 符号：

| 符号 | Java modifier |
|---|---|
| `+` | `public` |
| `-` | `private` |
| `#` | `protected` |

UML 格式：

```text
attribute: type
method(parameterName: ParameterType): ReturnType
```

UML 箭头：

| 箭头 | 含义 |
|---|---|
| 虚线箭头 | dependency / uses |
| 实线 + 空心三角 | inheritance / `extends` |
| 虚线 + 空心三角 | interface implementation / `implements` |

箭头方向：child class 指向 parent class；implementation class 指向 interface。

从 UML 写代码例子：

```text
Book
- title: String
- price: double
+ getTitle(): String
+ setPrice(price: double): void
```

对应代码：

```java
public class Book {
    private String title;
    private double price;

    public String getTitle() {
        return title;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}
```

## 0.3 Object Reference 与 Memory

### Primitive vs Reference Assignment

| 类型 | assignment 行为 |
|---|---|
| primitive | copy value，改一个不影响另一个 |
| object reference | copy address / reference，两个变量可能指向同一个 object |

Primitive 例子：

```java
int a = 5;
int b = a;
b = 10;
```

结果：

```text
a = 5
b = 10
```

Reference 例子：

```java
Student s1 = new Student("Tom", 20);
Student s2 = s1;
s2.setAge(99);
```

结果：`s1` 和 `s2` 指向同一个 object，所以 `s1` 的 age 也变成 99。

记忆：

```text
有 new = 新 object
没有 new = 复制 reference，可能 alias 同一个 object
assignment 只是复制那一刻的 value/reference
```

### 参数传递三规则

| 情况 | method 里改了以后，外面会变吗？ | 例子 |
|---|---|---|
| primitive parameter | 不会 | `change(num)` 后 `num` 不变 |
| object parameter + 修改 object 内部 | 会 | `s.setAge(99)` 外面也变 |
| object parameter + 指向 new object | 不会 | `s = new Student()` 外面的 reference 不变 |
| array parameter + 修改元素 | 会 | `arr[0] = 99` 外面也变 |

记忆：

```text
改内部会变
指新 object 不变
primitive 永远不变
```

### Garbage Collection

当一个 object 没有任何 reference 指向它时，它变成 garbage，Java 会自动回收。

```java
s1 = s2;
```

如果原来 `s1` 指向的 object 没有其他 reference 了，它就变成 garbage。

### 声明不等于创建

```java
Student s;      // 只声明，没有 new object
s.getName();    // 可能 NullPointerException
```

正确：

```java
Student s = new Student("Tom", 20);
s.getName();
```

### Array 也是 Object

```java
int[] a = {1, 2, 3};
int[] b = a;
b[0] = 99;
```

结果：

```text
a[0] 也变成 99
```

因为 `a` 和 `b` 指向同一个 array object。

## 0.4 Common Libraries 与 Enum

### Package 记忆

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

### String 常用 Methods

`String` 是 immutable，所以这些 methods 通常返回一个新 `String`，不会修改原 object。

| Method | 作用 | 例子 |
|---|---|---|
| `s.length()` | 字符数 | `"hello".length()` -> `5` |
| `s.charAt(i)` | 第 `i` 个字符 | `"hello".charAt(0)` -> `'h'` |
| `s.indexOf(str)` | 找 substring 位置 | `"hello".indexOf("ll")` -> `2` |
| `s.toUpperCase()` | 转大写，返回新的 String | 原 `s` 不变 |
| `s.substring(a, b)` | 截取 `[a, b)` | `"hello".substring(0,3)` -> `"hel"` |
| `s.equals(other)` | 比较内容 | 比较 String 必须用这个 |
| `s.compareTo(other)` | 字典序比较 | 负数 / 0 / 正数 |

重点：

```java
String s = "hello";
s.toUpperCase();
System.out.println(s);  // hello
```

要保存结果：

```java
s = s.toUpperCase();
```

String 拼接规则：

| Expression | Result | 原因 |
|---|---|---|
| `1 + 2 + "abc"` | `"3abc"` | 先算 `1+2`，再拼接 |
| `"abc" + 1 + 2` | `"abc12"` | 遇到 String 后继续拼接 |
| `1 + 2 + "abc" + 3 + 4` | `"3abc34"` | 左到右 |
| `"" + 1 + 2` | `"12"` | 空 String 开头，后面全拼接 |

记忆：

```text
见 String 前算术，见 String 后拼接。
想先算就用括号："abc" + (1 + 2) -> "abc3"
```

### Math

`Math` 的 methods 都是 static，所以直接用 `Math.method()`。

```java
Math.abs(-5);      // 5
Math.pow(2, 3);    // 8.0
Math.sqrt(16);     // 4.0
Math.max(3, 7);    // 7
Math.random();     // 0.0 <= value < 1.0
```

### Random

```java
import java.util.Random;

Random rand = new Random();
rand.nextInt(10);     // 0 到 9，不包括 10
rand.nextInt(6) + 1;  // 1 到 6，模拟骰子
```

### NumberFormat / DecimalFormat

```java
NumberFormat fmt = NumberFormat.getCurrencyInstance();
fmt.format(1234.5);  // $1,234.50，具体符号可能受 locale 影响
```

```java
DecimalFormat df = new DecimalFormat("0.00");
df.format(3.1);  // 3.10
```

### Enum

enum 用来表示固定的一组值。

```java
public enum Season {
    SPRING, SUMMER, FALL, WINTER
}
```

使用：

```java
Season s = Season.SUMMER;
s.ordinal();  // 1，从 0 开始
s.name();     // "SUMMER"
```

enum 可以有 constructor 和 method：

```java
public enum Season {
    SPRING("warm"), SUMMER("hot"), FALL("cool"), WINTER("cold");

    private String weather;

    private Season(String weather) {
        this.weather = weather;
    }

    public String getWeather() {
        return weather;
    }
}
```

```java
Season.SUMMER.getWeather();  // "hot"
```

重点坑：

```text
enum constructor 必须 private，因为 enum values 由 Java 自动创建，不能用 new。
普通 class constructor 通常 public。
```

## 0.5 Array 与 ArrayList

### Array 创建

```java
int[] nums = new int[5];             // 5 个格子，默认 0
int[] nums = {10, 20, 30};           // 声明时直接给值
int[] nums = new int[]{10, 20, 30};  // 重新赋值或传参时可用
```

### Array 使用

```java
nums[0];       // 读取，index 从 0 开始
nums[2] = 99;  // 修改
nums.length;   // 长度，没有括号
```

重点坑：

```text
5 个元素的 index 是 0 到 4，没有 index 5。
越界访问 -> ArrayIndexOutOfBoundsException。
```

### Object Array

```java
Student[] students = new Student[3];  // 初始全是 null
students[0] = new Student("Tom", 20); // 必须逐个 new
```

重点坑：

```java
students[0].getName();  // 如果 students[0] 还是 null，会 NullPointerException
```

### 二维数组

```java
int[][] grid = new int[3][4];  // 3 行 4 列
grid[1][2] = 5;

grid.length;     // 行数
grid[0].length;  // 第 0 行的列数
```

### ArrayList

```java
import java.util.ArrayList;

ArrayList<String> list = new ArrayList<String>();
```

常用 methods：

| Method | 作用 |
|---|---|
| `list.add("Tom")` | 加到末尾 |
| `list.add(0, "Bob")` | 插入到 index 0 |
| `list.get(i)` | 读取 index `i` |
| `list.set(i, "Jack")` | 修改 index `i` |
| `list.remove(i)` | 删除 index `i`，后面元素自动前移 |
| `list.size()` | 元素个数 |
| `list.contains("Jack")` | 是否包含，返回 boolean |

重点坑：

```text
ArrayList 只能存 object，不能直接存 primitive。
int -> Integer
double -> Double
char -> Character
boolean -> Boolean
```

### Array vs String vs ArrayList 长度

| 类型 | 写法 | 有没有括号 |
|---|---|---|
| Array | `array.length` | 没有括号 |
| String | `string.length()` | 有括号 |
| ArrayList | `list.size()` | 有括号，叫 `size` |

## 0.6 Method、Parameter Passing 与 Static

### Method 格式

```java
public static int add(int a, int b) {
    return a + b;
}
```

结构：

```text
visibility static? returnType methodName(parameter list) { body }
```

### 参数传递

| 情况 | 外面会变吗？ | 例子 |
|---|---|---|
| primitive parameter | 不会 | `change(num)` 后 `num` 不变 |
| object parameter + 修改内部 | 会 | `s.setAge(99)` 外面也变 |
| object parameter + 指向 new object | 不会 | `s = new Student()` 外面不变 |
| array parameter + 修改元素 | 会 | `arr[0] = 99` 外面也变 |

Past Paper Q2(a) 这类题的读法：

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

输出 array 的常见代码：

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

Past Paper Q2(a) 答案输出：

```text
2 3 0 0 10 0
1 0 10 0 10 0
```

### Method Overloading

overloading = 同名 method，但 parameter list 不同。

parameter list 不同可以是：

```text
parameter type 不同
parameter number 不同
parameter order 不同
```

重点坑：

```text
return type 不算 overloading。
如果 method name 和 parameter list 一样，只是 return type 不同，会 compile error。
```

Method signature：

```text
method name + parameter list
```

### Varargs

varargs = 可变长度参数。

```java
public static int sum(int... nums) {
    int total = 0;
    for (int n : nums)
        total += n;
    return total;
}
```

调用：

```java
sum(1, 2);
sum(1, 2, 3, 4);
```

重点坑：

```text
varargs 必须放在 parameter list 最后。
一个 method 只能有一个 varargs。
```

### Static

| | 普通 method | static method |
|---|---|---|
| 属于 | object | class |
| 调用方式 | `object.method()` | `Class.method()` |
| 能访问 instance variable？ | 可以 | 不可以直接访问 |
| 例子 | `s.getName()` | `Math.abs(-5)` |

重点坑：

```text
static method 不能直接访问 non-static instance variable。
原因：static 属于 class，不知道你指的是哪个 object。
```

`System.out.println()` 为什么不用 `new System()`？

```text
out 是 System class 的 static field，类型是 PrintStream，所以可以直接写 System.out.println()。
```

### Static Variable 与共享状态（朋友笔记补充）

`static variable` 属于 class，不属于某一个 object。也就是说，同一个 class 创建出来的所有 objects，会共享同一份 `static variable`。

最常见用途：记录“这个 class 总共创建过多少个 object”。

```java
public class Slogan {
    private String phrase;
    private static int count = 0;

    public Slogan(String str) {
        phrase = str;
        count++;
    }

    public static int getCount() {
        return count;
    }
}
```

调用：

```java
Slogan s1 = new Slogan("Hello");
Slogan s2 = new Slogan("Java");

System.out.println(Slogan.getCount());  // 2
```

考试判断规则：

```text
instance variable：每个 object 自己一份。
static variable：整个 class 共享一份。
instance method：可以直接访问 instance variable，也可以访问 static variable。
static method：只能直接访问 static variable，不能直接访问 instance variable。
```

为什么 `static method` 不能直接访问 instance variable？

```text
因为 static method 可以在没有任何 object 的情况下被调用。
如果没有 object，instance variable 根本不知道属于哪一个 object。
```

如果真的要在 `static method` 里改某个 object 的 instance variable，就要把 object 作为 parameter 传进去：

```java
public static void increment(Counter c) {
    c.value++;
}
```

## 0.7 Comparison、Wrapper Class、`equals`

### 各种类型怎么比较

| 类型 | 用什么比较 | 注意 |
|---|---|---|
| `int`, `char`, `boolean` | `==` | primitive 直接比 value |
| `double`, `float` | `Math.abs(a - b) < tolerance` | 不建议直接用 `==` |
| `String` | `.equals()` | `==` 比 reference，`.equals()` 比内容 |
| 自己写的 object | override `.equals()` | 默认 `.equals()` 通常也是比 reference |
| `Integer` 等 wrapper | `.equals()` | `==` 只在 cache 范围内看起来可靠 |
| `null` | `== null` 或 `!= null` | 不能对 `null` 调 `.equals()` |

重点坑：

```java
null.equals("abc");  // NullPointerException
```

更安全：

```java
"abc".equals(variable);
```

### 重写 `equals` 模板

```java
public boolean equals(Object obj) {
    if (!(obj instanceof Student))
        return false;

    Student other = (Student) obj;

    return this.name.equals(other.name)
        && this.age == other.age;
}
```

规则：

```text
equals 的 parameter type 是 Object。
primitive field 用 ==。
object field 用 .equals()。
先做 instanceof 检查，再 cast。
```

### Wrapper Class

| Primitive | Wrapper Class | 常见转换 |
|---|---|---|
| `int` | `Integer` | `Integer.parseInt("123")` |
| `double` | `Double` | `Double.parseDouble("3.14")` |
| `char` | `Character` | - |
| `boolean` | `Boolean` | - |

Autoboxing / unboxing：

```java
Integer x = 5;  // autoboxing: int -> Integer
int y = x;      // unboxing: Integer -> int
```

`Integer` cache：

```java
Integer num1 = 100;
Integer num2 = 100;
Integer num3 = 500;
Integer num4 = 500;
```

通常：

```text
num1 == num2  -> true，因为 Java 保证 -128 到 127 范围内的 Integer 会被 cache
num3 == num4  -> false，因为两个 500 通常是不同 Integer object
```

考试写法：

```java
num1.equals(num2);
num3.equals(num4);
```

### Wrapper Class 额外规则（朋友笔记补充）

Wrapper class 的作用：把 primitive value 包成 object。这样它就可以放进 `ArrayList<Integer>` 这种只收 object 的结构里，也可以作为 `Object` parameter 传递。

```java
int a = 5;
Integer obj = Integer.valueOf(a);  // int -> Integer object
int b = obj.intValue();            // Integer object -> int
```

现代 Java 会自动做这件事：

```java
Integer x = 5;  // autoboxing，本质上接近 Integer.valueOf(5)
int y = x;      // unboxing，本质上取出 x 里面的 int value
```

`Integer.valueOf()` 和 `new Integer()` 的区别：

```java
Integer a = Integer.valueOf(100);
Integer b = Integer.valueOf(100);
Integer c = new Integer(100);
Integer d = new Integer(100);
```

```text
a == b  是 true，因为 100 在 Java 保证的 Integer cache 范围内。
c == d  是 false，因为 new 每次都创建新的 object。
```

说明：`new Integer(100)` 在新版 Java 里已经不推荐使用，但考试如果给了这种代码，你要按“每次 `new` 都创建一个新的 object”来判断。

考试建议：

```text
比较 wrapper class 的 value：用 .equals()
不要用 == 当作 value comparison。
```

常用 parse methods：

```java
int n = Integer.parseInt("42");
double d = Double.parseDouble("3.14");
boolean ok = Boolean.parseBoolean("true");
```

如果字符串不是合法数字：

```java
Integer.parseInt("abc");  // NumberFormatException
```

常用 constants：

```java
Integer.MIN_VALUE  // -2147483648
Integer.MAX_VALUE  //  2147483647
Double.MAX_VALUE
```

特别容易混淆的一点：

```text
Float.MIN_VALUE 不是最负的 float。
Double.MIN_VALUE 不是最负的 double。
它们表示的是对应类型能表示的最小正数。
如果要表示负方向的大数，通常看 -Float.MAX_VALUE 或 -Double.MAX_VALUE。
```

`null` unboxing 会出错：

```java
Integer x = null;
int y = x;  // NullPointerException
```

原因：`int y = x` 需要把 `Integer` 拆成 `int`，但 `x` 是 `null`，没有 value 可以拆。

### `compareTo`

如果使用 generic interface：

```java
public class Student implements Comparable<Student> {
    public int compareTo(Student other) {
        return this.age - other.age;
    }
}
```

返回值：

```text
负数 -> this 排前面 / this < other
0    -> 相等
正数 -> this 排后面 / this > other
```

排序方向：

```java
return this.age - other.age;  // 从小到大
return other.age - this.age;  // 从大到小
```

注意：

```text
如果 implements Comparable<Student>，compareTo 的 parameter type 是 Student。
如果使用 raw Comparable，compareTo 的 parameter type 是 Object。
equals 的 parameter type 通常是 Object。
```

## 0.8 语法细节速查

### 分号 `;`

需要加分号：

```text
variable declaration
method call
return
import
new statement
break / continue
do-while 结尾
```

不加分号：

```text
class declaration
method declaration
if / else
for
while
switch
interface
enum
单独的右花括号 }
```

重点坑：

```java
if (x > 0); {
    ...
}
```

这个多出来的 `;` 会让 `if` 变成空语句，后面的 `{ ... }` 无论如何都会执行。`for` 后面误加分号也类似。

### 括号

| 括号 | 用途 | 例子 |
|---|---|---|
| `( )` | method 参数、条件、casting | `setAge(25)`, `if (x > 0)`, `(int) 3.14` |
| `{ }` | code block | `class Student { }` |
| `[ ]` | array 声明和访问 | `int[] nums`, `nums[0]` |
| `< >` | generics | `ArrayList<String>` |

### 命名规则

| 类别 | 规则 | 例子 |
|---|---|---|
| Class / Interface | 大写开头，CamelCase | `Student`, `ArrayList` |
| Method | 小写开头，camelCase | `getName()`, `compareTo()` |
| Variable | 小写开头，camelCase | `firstName`, `myAge` |
| Constant (`final`) | 全大写 + 下划线 | `MAX_SIZE`, `PI` |
| Enum value | 全大写 | `SPRING`, `SUMMER` |

### Operator Precedence

从高到低：

```text
() [] .
! ++ -- (type)
* / %
+ -
< > <= >= instanceof
== !=
&&
||
= += -=
```

重点坑：

```text
&& 优先级高于 ||。
a || b && c 等于 a || (b && c)，不是 (a || b) && c。
不确定就加括号。
```

### Java 文件命名规则

```text
一个 .java 文件最多只能有一个 public class。
文件名必须和 public class 名一模一样，大小写敏感。
```

## 0.9 常见错误速查

| 错误信息 / 现象 | 常见原因与修复 |
|---|---|
| `';' expected` | 语句末尾忘了分号 |
| `cannot find symbol` | 变量/method 名拼错，或者忘了 import |
| `incompatible types` | 类型不匹配，检查 type 或加 casting |
| `missing return statement` | 有 return type 的 method 某些路径没有 `return` |
| `non-static cannot be referenced` | static method 里直接用了 non-static member |
| `NullPointerException` | 对 `null` 调用了 method；先检查是否 `null`，或先 `new` object |
| `ArrayIndexOutOfBoundsException` | array index 越界，检查 `0` 到 `length - 1` |

## 0.10 Midterm 题型模板

### 题型 1：补 while loop 条件

例子：在一个 lowercase word 中找第一个 vowel，补全 while condition。

答案模式：

```java
while (position < word.length() && !found) {
    ...
}
```

解题步骤：

1. 先问：循环什么时候应该停止？
2. 停止条件通常是：找到了，或者已经查到末尾。
3. `while` 里写的是继续条件，不是停止条件。
4. 继续条件 = 还没查完 AND 还没找到。

```text
还没查完: position < word.length()
还没找到: !found
合起来: position < word.length() && !found
```

### 题型 2：Array 改 ArrayList，哪些部分会变？

核心原则：encapsulation。内部 implementation 变了，但 public interface 和 client code 不应该变。

| 部分 | 会变吗？ | 原因 |
|---|---|---|
| instance variables | YES | `String[] + numNames` 变成 `ArrayList<String>` |
| public method headers | NO | 对外 method signature 不变 |
| representation invariant | YES | 内部数据结构变了 |
| public method comments | NO | 对外功能描述不变 |
| public method implementations | YES | 内部用 ArrayList API 重写 |
| private helper methods | 通常 YES | 它们依赖内部 representation |
| client code | NO | 调用方式不变 |
| class comment | 通常 NO | class 整体功能没变 |

记忆：

```text
public API 不变，private implementation 会变。
```

### 题型 3：Inheritance vs Interface + private access

先背表：

| | Inheritance | Interface |
|---|---|---|
| 关键字 | `extends` | `implements` |
| 多继承 | class 只能 extends 一个 class | class 可以 implements 多个 interfaces |
| 内容 | 继承代码和状态 | 定义 behavior contract |
| 关系 | is-a | can-do / 能力合同 |

private access 例子：

```java
public class Super {
    private int priv;
    public int pub;
}

public class Sub extends Super {
    public void printSub(Sub sub) {
        // sub.priv;  // compile error
        // sub.pub;   // OK
    }
}
```

解决方法：

```text
方案 A: 把 private 改成 protected。
方案 B: 在 Super 里加 public getter。
```

### 题型 4：Array rotateLeft

目标：把 array 左旋转 `k` 位。

解法 1：一个 loop + `%`

```java
public static int[] rotateLeft(int[] nums, int k) {
    int[] result = new int[nums.length];
    k = k % nums.length;

    for (int dest = 0; dest < nums.length; dest++) {
        result[dest] = nums[(dest + k) % nums.length];
    }

    return result;
}
```

解法 2：两个 loops

```java
public static int[] rotateLeft(int[] nums, int k) {
    int[] result = new int[nums.length];
    k = k % nums.length;

    int dest = 0;

    for (int src = k; src < nums.length; src++) {
        result[dest++] = nums[src];
    }

    for (int src = 0; src < k; src++) {
        result[dest++] = nums[src];
    }

    return result;
}
```

关键：

```text
k = k % nums.length
长度 7 左旋 10 位 = 左旋 3 位
```

### 题型 5：完整 Class 编程模板

写 class 时按这个顺序检查：

```text
1. public class ClassName { }
2. private instance variables
3. constructor: 名字和 class 一样，没有 return type
4. getters
5. setters
6. toString()
7. equals(Object obj)
8. compareTo(...) 如果题目要求 Comparable
9. 每条语句末尾加 ;
```

### 题型 6：Object output tracing

看到代码追踪题，用内存图：

```text
看到 new -> 画一个新 object
看到 object assignment -> 复制 reference，画到同一个 object
看到 .setXxx() -> 修改 object 内部
看到 println(object) -> 调用 toString()
```

## 0.11 考前代码检查清单

写完代码后按顺序检查：

1. 每条 statement 末尾有 `;`。
2. class name 大写开头；method/variable name 小写开头。
3. constructor 没有 return type，`void` 也不能写。
4. instance variables 用 `private`。
5. 比较 `String` 用 `.equals()`，不用 `==`。
6. 比较 `double` 用 `Math.abs(a - b) < tolerance`。
7. array 用 `.length`；String 用 `.length()`；ArrayList 用 `.size()`。
8. array index 从 0 开始，最后一个是 `length - 1`。
9. ArrayList 不能直接写 primitive type，要用 wrapper class。
10. `equals` parameter type 通常是 `Object`。
11. `compareTo` parameter type 取决于是否使用 generic `Comparable<T>`。
12. static method 不能直接访问 non-static member。
13. subclass 不能直接访问 parent class 的 `private` variable。

快速口诀：

```text
有 new 新对象，没 new 同地址。
改内部会变，指新不变，primitive 永不变。
见 String 前算术，见 String 后拼接。
```

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

**已修正说明：** 原始整理中曾把 `63` 放在 `55` 下面；按照 insertion rule 和 traversal answers，`63` 应该是 `65` 的 left child。

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

# Part 10 / 10：Advanced Trees - AVL Tree + Heap + Huffman Coding

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

## 10.11 Huffman Coding（朋友笔记新增）

Huffman Coding 是一种用 tree 做压缩编码的方法。核心想法：

```text
出现频率高的字符 -> 用更短的 binary code
出现频率低的字符 -> 可以用更长的 binary code
```

这样整篇文章平均需要的 bits 会更少。

### Huffman Tree 怎么构造？

假设每个字符都有一个 frequency。

步骤：

1. 每个字符先单独成为一棵 tree，node 里放它的 frequency。
2. 每次找出 frequency 最小的两棵 trees。
3. 把这两棵 trees 合并成一棵新 tree。
4. 新 root 的 frequency = 两个子树 frequency 之和。
5. 重复，直到只剩一棵 tree。

直觉：

```text
frequency 小的 nodes 会更早被合并，通常会被放得更深。
frequency 大的 nodes 会更靠近 root，所以 code 更短。
```

### 怎么给字符编码？

建好 Huffman Tree 后，给每条 left/right edge 分配 `0` 和 `1`。只要整棵树里规则一致即可，例如：

```text
left edge  = 0
right edge = 1
```

某个字符的 code，就是从 root 走到该字符 leaf node 的路径。

重要性质：Huffman code 是 prefix code。

```text
prefix code = 没有任何一个字符的 code 是另一个字符 code 的开头。
```

为什么这很重要？

```text
解码时从左到右读 bits。
每当走到 leaf node，就确定读出了一个字符，然后回到 root 继续读。
不会出现“到底该停在哪里”的歧义。
```

### 课件风格例子

如果 `a, b, c, d, e, f` 的 frequencies 是：

```text
a:1, b:2, c:3, d:4, e:5, f:6
```

一种可能的 Huffman code 是：

```text
a -> 1111
b -> 1110
c -> 110
d -> 01
e -> 00
f -> 10
```

用这张 code table 解码：

```text
0001110100111010
```

从左到右切分：

```text
00 | 01 | 110 | 10 | 01 | 110 | 10
 e    d    c    f    d    c    f
```

所以结果是：

```text
edcfdcf
```

考试如果看到 Huffman Coding，先写这三句话：

```text
1. 每次合并 frequency 最小的两棵 trees。
2. 高频字符靠近 root，所以 code 短；低频字符更深，所以 code 长。
3. Huffman code 是 prefix code，所以可以无歧义解码。
```

## 10.12 小结

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

Huffman Coding：

- 每次合并 frequency 最小的两棵 trees。
- 高频字符用短 code，低频字符用长 code。
- Huffman code 是 prefix code，可以无歧义解码。

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
constructor: 名字等于 class 名，没有 return type
array.length / string.length() / list.size()
String 比内容用 .equals()
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

Huffman:
merge two lowest-frequency trees each time
high frequency -> shorter code
prefix code -> no ambiguous decoding
```
