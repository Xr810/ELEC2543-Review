# ELEC2543 Final 2025 Past Paper 答案

这份是答案版，不重复抄完整题目。重点写法按开卷考试使用：先给最终答案，再给可以直接写进答题纸的解释。题目选项保留英文原文，解释尽量用普通话；Java 术语例如 Object、Class、Wrapper Class、String、Exception 等保留英文。

## Question 1 选择题

### Q1(i)

**答案：a) Java Virtual Machine**

- a) Java Virtual Machine：正确。JVM 负责在具体平台上执行已经编译好的 Java bytecode，也就是 `.class` file。
- b) Java Compiler：错误。Compiler 的工作是把 `.java` source code 编译成 `.class` bytecode，不是执行 bytecode。
- c) Java Programming Manual：错误。Manual 只是文档，不会执行程序。
- d) Eclipse Editor：错误。Eclipse 是 IDE/editor，用来写和运行项目，但它本身不是 Java 语言执行 bytecode 的核心程序。

记忆：

```text
javac compiles source code.
JVM executes bytecode.
```

### Q1(ii)

**答案：c) False**

- a) Null：错误。对于 primitive `boolean` 来说，默认值不是 `null`。`null` 是 object reference 的默认值。如果题目写的是 Wrapper Class `Boolean`，默认值才是 `null`。
- b) True：错误。Java 的 `boolean` 默认值不是 `true`。
- c) False：正确。Class field / instance variable / static variable 如果是 primitive `boolean`，默认值是 `false`。
- d) 0：错误。`0` 是 numeric primitive type 例如 `int` 的默认值，不是 boolean 的值。

注意：local variable 没有默认值，必须先赋值才能使用。

### Q1(iii)

**答案：c) Supports multiple inheritance**

- a) Platform-independent：不是本题答案，因为这是 Java 的特点。Java source code 会先编译成 bytecode，然后不同平台用各自的 JVM 运行。
- b) High-performance：不是本题答案。Java 通常被认为有较好的 performance，尤其是有 JIT compilation。
- c) Supports multiple inheritance：正确。Java 的 Class 不支持 multiple inheritance。一个 Class 只能 `extends` 一个 superclass。
- d) Automatic memory management：不是本题答案。Java 有 garbage collection，所以这也是 Java 的特点。

重点：

```java
class C extends A, B { }      // 不合法
class C implements X, Y { }   // 如果 X 和 Y 是 interfaces，则合法
```

### Q1(iv)

**答案：c) java.lang**

- a) java.util：错误。`java.util` 常见的是 `Scanner`, `Random`, `ArrayList`, `HashMap`。
- b) java.io：错误。`java.io` 主要和 file、input/output 有关，例如 `File`, `FileReader`, `IOException`。
- c) java.lang：正确。`System`, `String`, `Math`, `Object` 都在 `java.lang`。
- d) java.text：错误。`java.text` 常用于格式化，例如 `NumberFormat`, `DecimalFormat`。

`java.lang` 会自动 import，所以平时不需要写：

```java
import java.lang.String;
import java.lang.System;
```

### Q1(v)

**答案：d) Compile-time exception**

- a) Checked exception：不是本题答案。Checked exception 是 Java Exception 的正式分类，compiler 会要求你处理，或者在 method header 用 `throws` 声明。
- b) Unchecked exception：不是本题答案。Unchecked exception 也是 Java Exception 分类，compiler 不强制处理。
- c) Runtime exception：不是本题答案。`RuntimeException` 是 unchecked exception 的重要类别。
- d) Compile-time exception：正确。Java 常见分类是 checked exception / unchecked exception，不把 "compile-time exception" 当作正式的 exception type。

例子：

```text
Checked: IOException, FileNotFoundException
Unchecked / Runtime: NullPointerException, ArithmeticException, ArrayIndexOutOfBoundsException
```

### Q1(vi)

**答案：d) All of the above**

- a) To define a constant：这个说法正确，但不完整。`final` variable 不能被重新赋值，所以常用于表示 constant。
- b) To prevent method overriding：这个说法正确，但不完整。`final` method 不能被 subclass override。
- c) To prevent inheritance：这个说法正确，但不完整。`final` class 不能被其他 Class extend。
- d) All of the above：正确。`final` 可以用于 variable、method、class，所以前三个都对。

例子：

```java
final int MAX = 100;          // constant
public final void run() { }   // 不能 override
public final class Test { }   // 不能 extend
```

### Q1(vii)

**答案：b) Runtime error**

- a) Compilation error：错误。一个 Java Class 即使没有 `main` method，也可以成功 compile。
- b) Runtime error：正确。如果你直接运行这个 Class，Java launcher 找不到程序入口，就会报告 main method missing。
- c) The program will run normally：错误。没有合法的 `main` method，就没有 standalone program 的正常入口。
- d) None of the above：错误，因为 b 是最合适的答案。

合法的 main method：

```java
public static void main(String[] args)
```

### Q1(viii)

**答案：a) It refers to the immediate superclass object**

- a) It refers to the immediate superclass object：正确。`super` 用来访问 immediate superclass 的 constructor、method 或 field。
- b) It refers to the current class object：错误。当前 object 应该用 `this`，不是 `super`。
- c) It refers to the constructor：错误。`super()` 可以调用 superclass constructor，但 `super` 本身不是 constructor。
- d) It refers to a static method：错误。Static method 属于 Class；`super` 主要和 superclass 的 instance member 或 constructor call 有关。

例子：

```java
super();          // 调用 superclass constructor
super.eat();      // 调用 superclass method
super.name;       // 如果可见，则访问 superclass field
```

### Q1(ix)

**答案：d) Compilation Error**

按题目原代码：

```java
String s = "blue";
s.concat("berry");
system.out.println(s);
```

- a) blue：如果最后一行写的是 `System.out.println(s);`，那输出会是 `blue`，因为 `String` 是 immutable，`concat` 会返回一个新的 String，不会直接改掉原本的 `s`。但题目里写的是小写 `system`。
- b) blueberry：错误。即使把 `System` 大写，`s.concat("berry")` 也不会直接修改 `s`，除非写成 `s = s.concat("berry");`。
- c) berry：错误。代码没有把 `"berry"` 赋值给 `s`。
- d) Compilation Error：正确。Java 大小写敏感，标准 class 是 `System`，不是 `system`。

考试注意：如果老师把小写 `system` 当作印刷错误，那么概念上会输出 `blue`。但按照题目代码原样判断，MCQ 答案应是 compilation error。

### Q1(x)

**答案：a) Result: 1020**

代码：

```java
System.out.println("Result: " + 10 + 20);
```

- a) Result: 1020：正确。`+` 从左到右计算。只要 `"Result: " + 10` 已经变成 String，后面的 `+ 20` 也会继续做 string concatenation。
- b) Result: 30：错误。要得到 30，必须先做 arithmetic，例如 `"Result: " + (10 + 20)`。
- c) Result: Error：错误。这段是合法 Java code。
- d) Result: 2010：错误。这里不会从右往左先算 `20 + 10`。

关键规则：

```text
"Result: " + 10 + 20
-> "Result: 10" + 20
-> "Result: 1020"
```

## Question 2 程序输出

### Q2(a)

**输出：**

```text
2 3 0 0 10 0
1 0 10 0 10 0
```

详细追踪：

在 `main` 里面：

```java
int y = 1;
int x = 3;
int[] a = new int[4];   // [0, 0, 0, 0]
mystery(a, y, x);
```

第一次调用是：

```java
mystery(a, 1, 3)
```

`mystery` 里面的参数名是：

```java
public static void mystery(int[] a, int x, int y)
```

所以第一次调用进入 method 后：

```text
x = 1
y = 3
a = [0, 0, 0, 0]
```

因为 `x < y` 也就是 `1 < 3` 成立，所以执行 `if` 分支：

```java
x++;       // x 变成 2
a[x] = 10; // a[2] = 10
```

然后输出：

```text
2 3 0 0 10 0
```

回到 `main`：

```java
x = y - 1;
```

因为 main 里面的 `y` 仍然是 `1`，所以 `x` 变成 `0`。array 的内容仍然被保留修改，因为 array 是 object，method 改的是同一个 array：

```text
a = [0, 0, 10, 0]
```

第二次调用是：

```java
mystery(a, 1, 0)
```

第二次调用进入 method 后：

```text
x = 1
y = 0
a = [0, 0, 10, 0]
```

现在 `x < y` 也就是 `1 < 0` 不成立，所以执行 `else` 分支：

```java
a[y] = 10;  // a[0] = 10
```

然后输出：

```text
1 0 10 0 10 0
```

重要概念：

```text
primitive `int` 参数：传进去的是值的 copy，所以 method 里面改参数，不会改 main 里面的原变量。
array 参数：传进去的是 reference 的 copy，所以 method 里面改 array element，会影响原本那个 array object。
```

### Q2(b)

**输出：**

```text
abcde
```

过程：

1. `System.out.print("a");` 输出 `a`。
2. 进入 `try`。
3. `System.out.print("b");` 输出 `b`。
4. `throw new IllegalArgumentException();` 抛出一个 exception。
5. `IllegalArgumentException` 是 `RuntimeException` 的 subclass，所以 `catch (RuntimeException e)` 可以抓住它。
6. catch block 输出 `c`。
7. `finally` 一定会执行，所以输出 `d`。
8. try-catch-finally 结束后，程序继续执行后面的语句，输出 `e`。

所以输出是：

```text
a b c d e
```

去掉空格后：

```text
abcde
```

### Q2(c)

**如果 throw 那一行被注释掉，输出是：**

```text
abde
```

过程：

1. 输出 `a`。
2. 进入 `try`。
3. 输出 `b`。
4. 没有 exception 被抛出。
5. 因为没有 exception，所以跳过 `catch`。
6. `finally` 仍然会执行，并输出 `d`。
7. try-catch-finally block 结束后，输出 `e`。

所以：

```text
a b d e
```

去掉空格后：

```text
abde
```

### Q2(d)

**输出：**

```text
num1 == num2
num3 != num4
```

解释：

```java
Integer num1 = 100;
Integer num2 = 100;
Integer num3 = 500;
Integer num4 = 500;
```

`Integer` 是 Wrapper Class，所以 `num1`、`num2` 这类变量是 object reference。对 object 来说，`==` 比较的是两个 reference 是否指向同一个 object，不是比较数值是否相等。

不过 Java 会 cache 很多小的 `Integer` object，通常范围是 `-128` 到 `127`。所以：

```java
Integer num1 = 100;
Integer num2 = 100;
```

通常会指向同一个 cached object，所以：

```text
num1 == num2
```

对于 `500`，它超出常见 cache 范围，所以：

```java
Integer num3 = 500;
Integer num4 = 500;
```

通常会创建或指向不同 object，所以：

```text
num3 != num4
```

考试保险写法：

```java
num3.equals(num4)
```

会比较数值，并返回 `true`。但 `num3 == num4` 比较的是 reference。

## Question 3 Binary Search Tree

插入顺序：

```text
60, 41, 74, 16, 53, 65, 25, 46, 55, 63, 70
```

### Q3(a)

**最终 BST：**

```text
            60
          /    \
        41      74
       /  \    /
     16   53  65
       \  / \ /  \
       25 46 55 63 70
```

同一棵树，用结构方式写是：

```text
60
├── left: 41
│   ├── left: 16
│   │   └── right: 25
│   └── right: 53
│       ├── left: 46
│       └── right: 55
└── right: 74
    └── left: 65
        ├── left: 63
        └── right: 70
```

插入逻辑：

- 60 成为 root。
- 41 < 60，所以 41 是 60 的 left child。
- 74 > 60，所以 74 是 60 的 right child。
- 16 < 60，而且 16 < 41，所以 16 是 41 的 left child。
- 53 < 60，但 53 > 41，所以 53 是 41 的 right child。
- 65 > 60，但 65 < 74，所以 65 是 74 的 left child。
- 25 < 60，25 < 41，但 25 > 16，所以 25 是 16 的 right child。
- 46 < 60，46 > 41，46 < 53，所以 46 是 53 的 left child。
- 55 < 60，55 > 41，55 > 53，所以 55 是 53 的 right child。
- 63 > 60，63 < 74，63 < 65，所以 63 是 65 的 left child。
- 70 > 60，70 < 74，70 > 65，所以 70 是 65 的 right child。

### Q3(b)

**会得到 sorted order 的 traversal：inorder traversal。**

Inorder 的意思是：

```text
left subtree -> root -> right subtree
```

对 BST 来说，inorder traversal 会按照从小到大的顺序输出数值。

**结果：**

```text
16, 25, 41, 46, 53, 55, 60, 63, 65, 70, 74
```

### Q3(c)

**Preorder traversal：**

Preorder 的意思是：

```text
root -> left subtree -> right subtree
```

**结果：**

```text
60, 41, 16, 25, 53, 46, 55, 74, 65, 63, 70
```

过程：

```text
先访问 60
再访问 60 的 left subtree： 41, 16, 25, 53, 46, 55
再访问 60 的 right subtree： 74, 65, 63, 70
```

### Q3(d)

**Postorder traversal：**

Postorder 的意思是：

```text
left subtree -> right subtree -> root
```

**结果：**

```text
25, 16, 46, 55, 53, 41, 63, 70, 65, 74, 60
```

过程：

```text
60 的 left subtree 给出： 25, 16, 46, 55, 53, 41
60 的 right subtree 给出： 63, 70, 65, 74
最后访问 root： 60
```

## Question 4 AVL Tree

### Q4(a)

**n 个 nodes 的 AVL tree 最小高度：**

如果 height 按 edge 数计算：

```text
minimum height = ceil(log2(n + 1)) - 1
```

如果 height 按最长路径上的 level/node 数计算：

```text
minimum height = ceil(log2(n + 1))
```

多数 data structure 课程把 height 定义为从 root 到 leaf 的最长路径上的 edge 数，所以第一条公式通常是预期答案。

原因：

对于任何 binary tree，如果 height `h` 按 edge 数计算，最多 nodes 数量是：

```text
2^(h + 1) - 1
```

如果要用尽量小的 height 存下 `n` 个 nodes：

```text
n <= 2^(h + 1) - 1
n + 1 <= 2^(h + 1)
log2(n + 1) <= h + 1
h >= log2(n + 1) - 1
```

所以：

```text
h = ceil(log2(n + 1)) - 1
```

### Q4(b)

**AVL tree 的复杂度：**

```text
search:    O(log n)
insertion: O(log n)
deletion:  O(log n)
```

解释：

AVL tree 会保持平衡，所以 tree 的 height 一直是 `O(log n)`。Search 最多只需要从 root 走一条路径到 leaf，因此是 `O(log n)`。

Insertion 和 deletion 会先按照普通 BST 规则沿着 root-to-leaf path 操作，然后回溯时用 rotation 重新平衡。需要检查的 ancestors 数量和 height 成正比，而每次 rotation 是 `O(1)`，所以 insertion 和 deletion 也是 `O(log n)`。

### Q4(c)

初始 AVL tree：

```text
        60
       /  \
     20    100
          /   \
        80     120
```

按照普通 BST 规则插入 `70`：

```text
70 > 60，往右走到 100
70 < 100，往左走到 80
70 < 80，插入为 80 的 left child
```

插入后暂时得到：

```text
        60
       /  \
     20    100
          /   \
        80     120
       /
     70
```

现在检查 balance：

- Node 80 仍然平衡：left height 是 0；如果 empty tree height 记作 -1，那么 right height 是 -1。
- Node 100 仍然平衡：left subtree height 是 1，right subtree height 是 0。
- Node 60 变得不平衡：left subtree height 是 0，right subtree height 是 2，差值是 2。

新插入的值走的是：

```text
right child of 60 -> left child of 100
```

所以这是 node 60 上的 **Right-Left (RL) case**。

RL case 要做两次 rotation：

1. 先在 100 做 right rotation。
2. 再在 60 做 left rotation。

Step 1：在 100 做 right rotation：

```text
        60
       /  \
     20    80
          /  \
        70    100
                \
                120
```

Step 2：在 60 做 left rotation：

```text
          80
        /    \
      60      100
     /  \       \
   20    70      120
```

**最终更新后的 AVL tree：**

```text
          80
        /    \
      60      100
     /  \       \
   20    70      120
```

检查最终 tree：

- Node 60 有 children 20 和 70，平衡。
- Node 100 只有 right child 120，仍然平衡。
- Root 80 的 left subtree height 和 right subtree height 都是 1，平衡。

## Question 5 Sorting

数组：

```text
2, 6, 5, 7, 9, 8, 3, 4
```

### Q5(a) Merge Sort

Merge Sort 思路：

```text
分成左右两半 -> 分别 sort -> merge 两个 sorted halves
```

步骤：

```text
[2, 6, 5, 7, 9, 8, 3, 4]

拆分：
[2, 6, 5, 7]            [9, 8, 3, 4]

继续拆分：
[2, 6] [5, 7]           [9, 8] [3, 4]

拆到 single element：
[2] [6] [5] [7]         [9] [8] [3] [4]

两两 merge：
[2, 6] [5, 7]           [8, 9] [3, 4]

merge 两个 half：
[2, 5, 6, 7]            [3, 4, 8, 9]

最终 merge：
[2, 3, 4, 5, 6, 7, 8, 9]
```

最终 sorted array：

```text
2, 3, 4, 5, 6, 7, 8, 9
```

### Q5(b) Quick Sort

题目没有指定 pivot rule，所以这里用一个清楚常见的规则：**选择第一个 element 作为 pivot**。

Quick Sort 思路：

```text
选择 pivot -> 把较小值 partition 到左边、较大值 partition 到右边 -> 递归 sort 左右两边
```

步骤：

```text
[2, 6, 5, 7, 9, 8, 3, 4]
pivot = 2
left = []
right = [6, 5, 7, 9, 8, 3, 4]
=> [] 2 [6, 5, 7, 9, 8, 3, 4]
```

Sort 右边部分：

```text
[6, 5, 7, 9, 8, 3, 4]
pivot = 6
left = [5, 3, 4]
right = [7, 9, 8]
=> [5, 3, 4] 6 [7, 9, 8]
```

Sort `[5, 3, 4]`：

```text
[5, 3, 4]
pivot = 5
left = [3, 4]
right = []
=> [3, 4] 5 []
```

Sort `[3, 4]`：

```text
[3, 4]
pivot = 3
left = []
right = [4]
=> [] 3 [4]
=> [3, 4]
```

所以 6 的左边部分变成：

```text
[3, 4, 5]
```

Sort `[7, 9, 8]`：

```text
[7, 9, 8]
pivot = 7
left = []
right = [9, 8]
=> [] 7 [9, 8]
```

Sort `[9, 8]`：

```text
[9, 8]
pivot = 9
left = [8]
right = []
=> [8] 9 []
=> [8, 9]
```

所以 6 的右边部分变成：

```text
[7, 8, 9]
```

合并：

```text
[] 2 [3, 4, 5, 6, 7, 8, 9]
=> [2, 3, 4, 5, 6, 7, 8, 9]
```

最终 sorted array：

```text
2, 3, 4, 5, 6, 7, 8, 9
```

考试注意：Quick Sort 的中间步骤可能因为 pivot choice 不同而不同。只要 partition 逻辑合理，最后 sorted result 一样。

### Q5(c)

**Merge Sort average time complexity：**

```text
O(n log n)
```

**Merge Sort worst case time complexity：**

```text
O(n log n)
```

原因：

Merge Sort 每次都会把 array 分成两半，所以大约有 `log n` 层。每一层把所有 elements merge 起来，总成本是 `O(n)`。所以：

```text
每层 O(n) * O(log n) 层 = O(n log n)
```

这个复杂度不太依赖 input 原本是不是 sorted，所以 average case 和 worst case 都是 `O(n log n)`。

### Q5(d)

**Quick Sort average time complexity：**

```text
O(n log n)
```

**Quick Sort worst case time complexity：**

```text
O(n^2)
```

原因：

Average case 里，pivot 通常能把 array 分得比较平均，所以大约有 `log n` 层；每一层做 `O(n)` 的 partition 工作：

```text
average = O(n log n)
```

Worst case 里，pivot 每次都刚好是最小或最大 element，导致 partition 极度不平衡：

```text
一边有 n-1 个 elements，另一边有 0 个 elements
```

那么总工作量变成：

```text
n + (n - 1) + (n - 2) + ... + 1 = O(n^2)
```

## Problem 6 OOP

### Q6(a)

一种正确写法：

```java
class Animal {
    public void eat() {
        System.out.println("The animal is eating");
    }
}

class Bird extends Animal {
    @Override
    public void eat() {
        System.out.println("The bird is eating worms");
    }
}

class Fish extends Animal {
    @Override
    public void eat() {
        System.out.println("The fish is eating shrimps");
    }
}
```

解释：

- `Animal` 是 superclass。
- `Bird extends Animal` 表示 Bird 是 Animal 的 subclass。
- `Fish extends Animal` 表示 Fish 也是 Animal 的 subclass。
- `Bird` 和 `Fish` 都 override 了 `eat()`。
- 调用 `eat()` 时，如果 actual object 是 subclass object，Java 会执行 subclass 里面 override 后的 method。

测试例子：

```java
Animal a1 = new Bird();
Animal a2 = new Fish();

a1.eat();  // 输出：The bird is eating worms
a2.eat();  // 输出：The fish is eating shrimps
```

这体现了 polymorphism：变量类型可以写成 `Animal`，但真正运行哪个 overridden method，由 actual object 决定。

### Q6(b)

一种正确写法：

```java
interface Animal {
    void eat();
}

interface Wings {
    void fly();
}

class Eagle implements Animal, Wings {
    @Override
    public void eat() {
        System.out.println("The eagle is eating");
    }

    @Override
    public void fly() {
        System.out.println("The eagle is flying");
    }
}
```

测试例子：

```java
Eagle eagle = new Eagle();
eagle.eat();  // 输出：The eagle is eating
eagle.fly();  // 输出：The eagle is flying
```

解释：

- `Animal` 是一个 interface，规定必须有 `eat()` 这个行为。
- `Wings` 是另一个 interface，规定必须有 `fly()` 这个行为。
- `Eagle implements Animal, Wings` 表示 Eagle 承诺会实现这两个 interfaces 里面要求的 methods。
- Java 的 Class 可以 implement 多个 interfaces，所以这可以避开 Class 只能 single inheritance 的限制。

### Q6(c)

**偏好的答案：如果目标是灵活性，我更推荐 interfaces。**

原因：

Java 只允许一个 Class extend 一个 superclass，但允许一个 Class implement 多个 interfaces。在这题里，`Eagle` 既需要 eating behavior，也需要 flying behavior。Interfaces 更适合表达 ability 或 contract，例如 `eat()` 和 `fly()`，因为一个 Class 可以组合多个 abilities。

可以直接写进考试的答案：

```text
我更推荐 interface implementation，因为它更灵活。Java 不支持 Class 的 multiple inheritance，所以一个 Class 只能 extend 一个 superclass。但是一个 Class 可以 implement 多个 interfaces。在这个例子里，Eagle 可以同时 implement Animal 和 Wings，所以它可以同时有 eat() 和 fly() 行为。Interfaces 适合用来规定必须实现的方法，而不强迫所有 Class 共享同一个 superclass implementation。
```

不过，如果 subclasses 真的需要共享 common code，inheritance 仍然有用：

```text
如果 Bird 和 Fish 需要共享 Animal 里面的 common fields 或 methods，那么 class inheritance 有用，因为它表达的是 is-a relationship，并且可以 code reuse。
```

所以比较完整的最终答案是：

```text
当对象之间有强烈的 is-a relationship，而且需要 shared implementation 时，用 class inheritance。 当设计重点是 abilities，或者一个 Class 需要组合多个 behaviors 时，用 interfaces。对这题来说，我更推荐 interfaces，因为 Eagle 可以同时 implement Animal 和 Wings。
```
